# Ansible Repository

## Introduction

This repository contains Ansible playbooks and roles for configuring and managing servers. Ansible is an open-source automation tool that automates software provisioning, configuration management, and application deployment.

Docker configurations are available to run Ansible inside a Docker container and manage other Ubuntu containers via SSH.

## Requirements

- Docker
- Docker Compose

## Structure

```
.
├── ansible
│   ├── ansible.cfg
│   ├── inventory
│   ├── playbooks
│   │   ├── playbook1.yml
│   │   └── playbook2.yml
│   └── roles
│       ├── role1
│       │   ├── tasks
│       │   │   └── main.yml
│       │   └── vars
│       │       └── main.yml
│       └── role2
│           ├── tasks
│           │   └── main.yml
│           └── vars
│               └── main.yml
└── docker
    ├── docker-compose.yml
    ├── Dockerfile-ansible
    ├── Dockerfile-ubuntu
    ├── id_ansible
    └── id_ansible.pub
```

In this structure:

- The `ansible` directory contains all the Ansible related files:
    - `ansible.cfg` is the configuration file for Ansible.
    - `inventory` file contains the list of hosts to be managed by Ansible.
    - `playbooks` directory contains all the Ansible playbooks.
    - `roles` directory contains all the Ansible roles. Each role has its own directory, and inside each role directory, there are `tasks` and `vars` directories containing the `main.yml` files.
- The `docker` directory contains all the Docker related files:
    - `docker-compose.yml` is the Docker Compose file for orchestrating the containers.
    - `Dockerfile-ansible` is the Dockerfile for building the Ansible container.
    - `Dockerfile-ubuntu` is the Dockerfile for building the Ubuntu containers.
    - `id_ansible` is the private key for the Ansible container to SSH into the Ubuntu containers.
    - `id_ansible.pub` is the public key for the Ansible container. It is added to the authorized_keys file in the Ubuntu containers.

This structure keeps all the Ansible and Docker related files organized in separate directories, making it easier to manage and understand.

## How to use

1. Make sure you have Docker and Docker Compose installed on your machine.

2. Build the Docker images and start the containers using Docker Compose. Run the following command in the same directory as the `docker-compose.yml` file:

```
docker-compose up --build -d
```

3. Once the containers are up and running, you can use the Ansible container to run Ansible commands on the Ubuntu containers. First, exec into the Ansible container:

```
docker exec -it ansible /bin/sh
```

4. Then, you can run Ansible commands. For example, to ping all hosts in the inventory file, run:

```
ansible all -m ping
ansible all -m gather_facts
```
Ansible will use the ansible/ansible.cfg in the ansible directory (Setting the `$ANSIBLE_CONFIG` env var to `ansible.cfg` to get around the world writable warning for demo purposes).
```
[WARNING]: Ansible is being run in a world writable directory (/data/ansible), ignoring it as an ansible.cfg source. For more information see
https://docs.ansible.com/ansible/devel/reference_appendices/config.html#cfg-in-world-writable-dir
```

5. Running sudo commands using ansible. For example, to apt update all hosts in the inventory file, run:

```
ansible all -m apt -a update_cache=true --become --become-ask-pass
```
The password is `ansible`

5. Running playbooks using ansible. For example, to run the playbook install_apache on all hosts in the inventory file, run:

```
ansible-playbook --ask-become-pass ansible/playbook/install_apache.yml
```
The password is `ansible`

## Contributing

Contributions are welcome! If you have any ideas, suggestions, or bug reports, please open an issue or submit a pull request. Follow the [CONTRIBUTING.md](CONTRIBUTING.md)

## License & Code of Conduct

By contributing, you agree to the license of this project ([LICENSE](LICENSE)) and the terms of the Code of Conduct ([CONDUCT](CONDUCT.md)).