# Keycloak Development Environment with Vagrant
This repository provides an **automated way to deploy Keycloak** inside a **Vagrant-managed Ubuntu VM**. The setup provisions Keycloak with **Java 17**, configures **systemd** for automatic startup, and creates the **Keycloak admin user**.

**This setup is intended for development and testing only. It is NOT for production use.**

## Features

- **Vagrant-based setup** – Easily spin up a Keycloak instance inside a virtual machine.
- **Systemd integration** – Configures Keycloak to start automatically as a systemd service.
- **Admin user creation** – Sets up an initial administrator account for Keycloak.

## Getting Started

1. Clone this repository and start the VM:
   ```sh
   git clone https://github.com/denispavlysh/keycloak_vagrant_dev.git
   cd keycloak_vagrant_dev
   vagrant up

2. Access Keycloak in your browser
   ```
   http://localhost:8080
3. Login into Administration area at `/admin` using `admin`/`admin` as credentials
