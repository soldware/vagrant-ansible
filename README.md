# README — Vagrant + Ansible Infrastructure Setup
This repository contains configuration files to provision a **3-node local development environment** simulating a production-like infrastructure using:

* **Vagrant** to spin up 3 Debian 12 virtual machines (Flask app server, Nginx reverse proxy, MongoDB server)
* **Ansible** to provision and configure each VM automatically

---

## Infrastructure layout

| VM Name | IP Address    | Role                        |
| ------- | ------------- | --------------------------- |
| flask   | 192.168.56.10 | Dockerized Flask API server |
| nginx   | 192.168.56.11 | Nginx reverse proxy + SSL   |
| mongodb | 192.168.56.12 | Bare-metal MongoDB database |

---

## Prerequisites

* [Vagrant](https://www.vagrantup.com/downloads)
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads) (or another Vagrant provider)
* [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) installed on your host machine

---

## Setup instructions

1. **Clone this repository**

```bash
git clone <your-repo-url>
cd <repo-folder>
```

2. **Start the Vagrant environment**

```bash
vagrant up
```

* This command creates 3 Debian VMs and runs the Ansible playbook to provision each.

3. **Access your VMs**

```bash
vagrant ssh flask
vagrant ssh nginx
vagrant ssh mongodb
```

4. **Your Flask app**

* The Flask app is dockerized and runs as a systemd service on the `flask` VM.
* Accessible via the Nginx proxy at `http://192.168.56.11`

5. **Nginx Reverse Proxy**

* Installed and configured on the `nginx` VM.
* Proxies requests to the Flask app server.
* Ready for SSL setup (e.g., via Certbot).

6. **MongoDB**

* Installed bare metal on the `mongodb` VM.
* Runs as a dedicated non-root user `mongouser`.
* Accessible from the Flask app VM for persistent data storage.

---

## File structure

* **Vagrantfile** — Defines the 3 VMs and uses Ansible for provisioning.
* **inventory.ini** — Ansible inventory listing VM IPs and groups.
* **site.yml** — Main Ansible playbook provisioning Flask, Nginx, and MongoDB.
* **flaskapp/** — Folder containing your Flask app source code and Dockerfile (you need to add this).

---

## Notes

* Modify `site.yml` and `inventory.ini` to match your network or VM preferences.
* The playbook installs all necessary software, configures services, and ensures each runs under proper users.
* The Flask app Docker image is built locally inside the flask VM.
* Nginx config is cleanly managed in `/etc/nginx/sites-available` and enabled via symlink.
* MongoDB runs as non-root user with proper permissions.

---

## Useful commands

* `vagrant halt` — Stop all VMs.
* `vagrant destroy` — Remove all VMs.
* `vagrant reload --provision` — Restart and re-provision VMs.
* `ansible-playbook -i inventory.ini site.yml` — Run Ansible playbook separately if needed.

---

## Next steps

* Add your Flask app code and Dockerfile inside the `flaskapp/` directory.
* Customize Nginx config for your domain and enable SSL with Certbot.
* Extend Ansible playbook for monitoring, logging, or CI/CD integration.

