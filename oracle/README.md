# Oracle Cloud Infrastructure & Red Hat

This section contains documentation and resources for working with **Oracle Cloud Infrastructure (OCI)** in combination with **Red Hat Enterprise Linux (RHEL)**.

## 📄 Documentation

- [SOP: Deploying a RHEL Image to OCI with SSH/SFTP Access](./docs/rhel-oci-ssh-sop.md)  
  A detailed, step‑by‑step Standard Operating Procedure covering:
  - Building a custom RHEL image in **Red Hat Insights – Image Builder**
  - Importing the image into **OCI** as a Custom Image
  - Launching a VM instance from the image
  - Configuring secure SSH access for a chosen login user
  - Enabling SFTP access via FileZilla
  - Optimizing OCI storage usage after deployment

## 📂 Folder Structure

```text
oracle/ 
    ├── docs/ 
        └── rehel-oci-ssh-sop.nd  # Full SOP for RHEL → OCI → SSH/SFTP
```

## 🛠 Intended Audience

This documentation is aimed at developers, system administrators, and cloud engineers who work with Oracle Cloud Infrastructure and Red Hat Enterprise Linux.

## 🚀 Next Steps

Future updates will include:

- Advanced OCI networking configurations for RHEL VMs
- Automation scripts for deployment and provisioning
- etc. ...
