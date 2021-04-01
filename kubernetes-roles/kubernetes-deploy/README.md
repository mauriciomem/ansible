# Kubernetes cluster deployment

_**work in progress**_

A quick alternative to deploy kubernetes clusters.

## Role

Deploy a single or multi control plane kubernetes cluster. This role uses kubeadm commands for boostrapping worker and control plane nodes.

_**Working on:**_
 - single control plane endpoint
 - deploying optional components:
   - default CNI
   - On premise LoadBalancer Service [metalLB](https://metallb.universe.tf/)
   - On premise Kubernetes Ingress controller [nginx](https://kubernetes.github.io/ingress-nginx/)

This role should not be used on production deployments. It doesn't cover:
 - node certificate management.
 - roles and authentication.
 - external high availability etcd database cluster.

## Requirements

  - Install ansible: [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
  - Run docker role: [mauriciomem/ansible/docker-role](https://github.com/mauriciomem/ansible/tree/main/docker-role)
  - Run kubernetes install: [mauriciomem/ansible/kubernetes-roles/kubernetes-install](https://github.com/mauriciomem/ansible/tree/main/kubernetes-roles/kubernetes-install)

 > **Note**: Also, if you are interested in bootstrapping compute instances also, I suggest you looking solutions like packer to build server images from ISO files and vagrant/terraform to deploy them locally or on cloud environments. You can check some packer examples on [mauriciomem/packer_vsphere](https://github.com/mauriciomem/packer_vsphere) using the vsphere-iso builder.

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
      # docker container runtime install.
      - { role: docker-role, dir: 'docker-role' }
      # kubernetes binaries and os preparation.
      - { role: kubernetes-install, dir: 'kubernetes-install' }
      # kubernetes cluster deployment
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
