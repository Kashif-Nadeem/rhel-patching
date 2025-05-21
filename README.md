# Automated Red Hat Enterprise Linux (RHEL) Patching with Ansible & GitHub Actions

This repository automates the OS patching and post-patching verification of Red Hat Enterprise Linux 8/9 hosts using **Ansible** and **GitHub Actions** with a self-hosted runner.

---

## Features

- **Automated patching workflow:** Triggered manually or via schedule using GitHub Actions.
- **Multi-host patching:** Updates all RHEL hosts defined in the Ansible inventory.
- **Pre/post-update hooks:** Easily customize with your own scripts for pre- and post-patching steps.
- **Notification support:** Send email notifications (via Ansible or shell) about patching status.
- **Idempotent and safe:** Includes error handling and host health checks.

---

## Repository Structure


---

## Prerequisites

- **GitHub repository** with a [self-hosted runner](https://docs.github.com/en/actions/hosting-your-own-runners/about-self-hosted-runners) (recommended: Ubuntu).
- **Ansible** installed on the runner.
- **SSH key-based authentication** set up from the runner to all target RHEL hosts.
- **RHEL 8/9 servers** as targets, with `python3`, `sudo`, and optionally `mailx` installed (if using shell mail notifications).

---

## Setup and Usage

### 1. **Clone the repository and configure inventory**
- Edit `ansible/inventory/hosts` to list all RHEL hosts to patch.
- Add group or host variables as needed under `group_vars` or `host_vars`.

### 2. **(Optional) Customize Pre/Post Scripts**
- Edit scripts in `playbooks/scripts/` as required.
- Remove or modify `env_verify.sh` and related calls if not needed for your environment.

### 3. **Set up SSH keys**
- Ensure your self-hosted runner can SSH into each target host as a user with `sudo` privileges.

### 4. **Configure GitHub Actions**
- The workflow is set up to:
    - Check out this repo
    - Install Ansible and any required collections
    - Run the main playbook (`playbooks/update.yaml`)
    - Export any necessary secrets (e.g., for email notifications)

### 5. **Trigger the Workflow**
- Go to the **Actions** tab in GitHub and run the `patch` workflow manually.

---

## Customization

- **Email notifications:** You can use either the Ansible `mail` module (with a local MTA/sendmail) or `community.general.mail` (with SMTP credentials).
- **Application checks:** Add your own logic to `env_verify.sh`, `pre_update.sh`, or `post_update.sh` if you have custom apps to monitor.
- **Comment out/remove** any app-specific checks/scripts that do not apply to your environment.

---

## Troubleshooting

- **Missing mail command:** Install `mailx` with `sudo yum install -y mailx` if scripts fail on `/bin/mail`.
- **User does not exist:** Comment/remove lines referencing non-existent users (e.g., `ppmadmin`) if not needed.
- **Out of memory:** If playbook is killed with code 137, add RAM or swap to the target host.
- **Authentication errors:** Double-check SSH keys, `sudo` access, and any workflow secrets for email.

---

## Security

- **Never commit sensitive secrets** to the repository. Use GitHub Actions Secrets for passwords, tokens, or SMTP app passwords.
- SSH keys should be stored securely on the runner and not in the repo.

---

## Credits & References

- Based on ideas from [Encore Technologies' Ansible patching blog series](https://encoretechnologies.github.io/blog/2018/06/ansiblepatchingautomation/).

---

## License

MIT License (see LICENSE file if included).


