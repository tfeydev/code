# SOP: Deploying a RHEL Image to OCI with SSH/SFTP Access

This Standard Operating Procedure (SOP) documents my process for deploying a custom RHEL image to Oracle Cloud Infrastructure (OCI) using Red Hat Insights Image Builder on my RHEL 10 dual-boot system. I used OCI’s Always Free tier to keep costs at zero, setting up a VM with SSH and SFTP access for secure management.

## 0. Prepare Prerequisites

- **Red Hat Insights**: Created an account at `console.redhat.com` for Image Builder access.
- **OCI Tenancy**: Signed up for Always Free tier at oracle.com/cloud/free; created a compartment.
- **Network Setup**: Configured a Virtual Cloud Network (VCN) and subnet with TCP/22 open for SSH. In OCI Console: Networking → Virtual Cloud Networks → Create VCN → Add subnet → Add ingress rule (Source: 0.0.0.0/0, Protocol: TCP, Port: 22).
- **Local SSH Client**: Used OpenSSH on RHEL 10 (`sudo dnf install openssh-clients` if not installed).

**Verification**:
- Confirm OCI network: In OCI Console, check VCN’s security list for TCP/22.
- Test SSH client: `ssh -V` (should show OpenSSH version, e.g., OpenSSH_9.x).

## 1. Build Image in Red Hat Insights

- Accessed `console.redhat.com/insights/image-builder`.
- Selected RHEL 10, added `openssh-server` package for SSH access, set hostname (e.g., `rhel-test`).
- Generated SSH key pair locally: `ssh-keygen -t ed25519 -C "my_email@example.com"` (saved to `~/.ssh/id_ed25519`).
- Uploaded public key to Image Builder to embed in `cloud-user` account.
- Built `.qcow2` image (takes ~15 minutes); noted download URL.

**What I Did**: Ensured minimal packages to fit OCI Always Free storage limits (< 100 GB).

**Verification**: Download image and run `qemu-img info image.qcow2` (confirms QCOW2 format).

## 2. Import Image into OCI

- Uploaded image to OCI Object Storage: Created bucket (Storage → Buckets → Create Bucket → Upload).
- In OCI Console: Compute → Custom Images → Import Image → Select Object Storage, set format to QCOW2, launch mode to Emulated.
- Waited for "Available" status (checked via Work Requests).

**What I Did**: Tested uploads to ensure no storage fees; verified image integrity.

**Verification**: Run `oci compute image list --compartment-id <your_compartment_ocid>` (install OCI CLI: `curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.py -o install.py && python3 install.py --accept-all-defaults`). Confirm image appears.

## 3. Launch VM from Custom Image

- In OCI Console: Compute → Instances → Create Instance.
- Selected custom image, used Always Free shape (e.g., VM.Standard.A1.Flex), chose VCN/subnet.
- Pasted SSH public key for `cloud-user`.
- Enabled public IP in Networking section; launched instance.

**What I Did**: Ran multiple test launches to confirm RHEL boots correctly.

**Verification**: Check instance status (Running) in OCI Console; ping public IP: `ping <public_IP>` (should respond).

## 4. Configure SSH for Intended User

- Confirmed default user as `cloud-user` (via OCI Console Connection if SSH fails).
- Derived public key locally: `ssh-keygen -y -f ~/.ssh/id_ed25519`.
- Accessed VM via Console Connection (Compute → Instances → Console Connection → Connect):
  - `mkdir -p ~/.ssh && chmod 700 ~/.ssh`
  - `echo "<your_public_key>" >> ~/.ssh/authorized_keys`
  - `chmod 600 ~/.ssh/authorized_keys`

**What I Did**: Set up secure SSH access without passwords; tested on multiple VMs.

**Verification**: In VM, run `ls -ld ~/.ssh` (should show `drwx------`); `ls -l ~/.ssh/authorized_keys` (should show `-rw-------`).

## 5. Verify SSH Access

- Connected: `ssh -i ~/.ssh/id_ed25519 cloud-user@<public_IP>`

**What I Did**: Tested from RHEL 10 local system; ensured firewall allows outbound SSH.

**Verification**: Run `whoami` in SSH session (should return `cloud-user`); retry connection to confirm stability.

## 6. Configure SFTP in FileZilla

- In FileZilla: Site Manager → New Site.
- Settings: Protocol: SFTP; Host: <public_IP>; Port: 22; Logon Type: Key file; User: cloud-user; Key file: ~/.ssh/id_ed25519.
- Connected and transferred test files.

**What I Did**: Uploaded files to VM to verify SFTP functionality.

**Verification**: Upload a file (e.g., `echo "test" > test.txt` locally, upload via FileZilla); in VM, run `cat test.txt` (should show "test").

## 7. Optimize Storage

- Deleted unused images via OCI Console (Compute → Custom Images → Delete) to stay within Always Free limits.
- Optionally exported to Object Storage first: Select image → Export → To Object Storage.

**What I Did**: Monitored storage usage to avoid exceeding free tier limits.

**Verification**: Check storage in OCI Console (should decrease after deletion); run `oci os object list --bucket-name <your_bucket>` to confirm exports.