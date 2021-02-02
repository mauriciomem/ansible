# Kubernetes cluster deployment

_**work in progress**_

Install all necesary Kubernetes binaries and ajust the minimal configuration parameters on a target machine to join a Kubernetes instance.

## Role

This role install kubeadm, kubectl and kubelet binaries, and configure a machine to run Kubernetes with docker as a container runtime. 
Also, it can remove all Kubernetes binaries and restore the machine previous configuration if it's necessary.

## Requirements

  - Install ansible: [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
  - Run docker-role: [mauriciomem/ansible/docker-role](https://github.com/mauriciomem/ansible/tree/main/docker-role)

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
  - name: kubernetes install
    hosts: servers
    gather_facts: yes
    become: true

    roles:
      # docker container runtime install.
      - { role: docker-role, dir: 'docker-role' }
      # kubernetes binaries and os preparation.
      - { role: kubernetes-install, dir: 'kubernetes-install' }
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
```

## License

MIT

## References

[Bootstrapping clusters with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/)

## Author

[Mauricio Mitolo](https://github.com/mauriciomem)
