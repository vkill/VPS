## Ansible

### Install

Ref https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-ubuntu

```
sudo apt update
sudo apt install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

Ref https://galaxy.ansible.com/ansible/posix

```
ansible-galaxy collection install ansible.posix
```
