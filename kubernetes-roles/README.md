# Kubernetes install & deploy

_**work in progress**_

Quickly deploy Kubernetes clusters for lab and app development. It follows the official Kubernetes practices, but it's not intended for use in production.

## Roles

Single or multi control plane kubernetes cluster. _**work in progress**_

 * kubernetes-install: Prepare instances to join a kubernetes cluster
 * kubernetes-deploy: kubeadm init & kubeadm join as worker or control plane nodes

**Working on:**
 - single control plane endpoint
 - default cni

## Requirements

  - Install ansible: [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
  - Run docker role: [mauriciomem/docker-role](https://github.com/mauriciomem/ansible/tree/main/docker-role)

**Tested on**
 - CentOS 7
 - Debian 10

**Tested with**
 - packer 1.6.6 _(go1.15.6)_
 - ansible 2.9.16 _(python 2.7.16 [GCC 8.3.0])_

## Run

Example playbook file

```
  ---
  - name: kubernetes cluster deploy
    hosts: servers
    gather_facts: yes
    become: true

    roles:
      - { role: kubernetes-install, dir: 'kubernetes-install' }
      - { role: kubernetes-deploy, dir: 'kubernetes-deploy' }
```
Run command example

```
  ansible-playbook -u ansible -i inventory playbook.yml
```
Example inventory file

```
  [servers]
  master01  ansible_host=10.1.1.149
  master02  ansible_host=10.1.1.251
  worker01  ansible_host=10.1.1.252

  [masters]
  master01
  master02

  [workers]
  worker01
```

## License

MIT

## Author

[Mauricio Mitolo](https://github.com/mauriciomem)
