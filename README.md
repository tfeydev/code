# Code Repository: Learning Linux, OCI, and Java Integration

This repository organizes my learning progress with RHEL 10 Linux (dual-boot setup), Oracle Cloud Infrastructure (OCI) Always Free tier, Bash scripting, and Java integration. It focuses on deploying RHEL images to OCI and testing related tools like Podman for future containerization.

## What I've Done

- Configured a dual-boot system with RHEL 10 for local development and testing.
- Used OCI Always Free tier to deploy custom RHEL images, including SSH/SFTP access setup.
- Documented the OCI deployment process in a detailed SOP (see [oracle/docs/oci-deployment-sop.md](oracle/docs/oci-deployment-sop.md)).
- Created a Java project to test OCI SDK connections (linked below).
- Structured folders for future work with Podman and Bash scripts.

## Folder Structure

- **containers**: Placeholder for Podman experiments (planned for future containerized Java or Wildfly apps).
- **linux**: Notes on RHEL 10 dual-boot setup and Bash scripting experiments.
- **oracle**: OCI-specific content, including deployment guides in `docs/`.
- **scripts**: Bash scripts for automation, such as OCI resource checks.

## Getting Started

1. Clone the repo: `git clone https://github.com/tfeydev/code.git`
2. Explore folders: For OCI deployment, see `oracle/docs/oci-deployment-sop.md`.

To verify setup:
- Check folder structure: `ls -R` (should show `containers`, `linux`, `oracle`, `scripts`).
- Verify RHEL environment: Run `cat /etc/redhat-release` (should show RHEL 10 if on your dual-boot system).

## Related Projects

- [ConnectionTestOciJavaSdk](https://github.com/tfeydev/ConnectionTestOciJavaSdk): A Maven-based Java project testing OCI SDK connections, complementing the OCI deployment work here.