# Ansible Role Apache 2.4.x

Rol que permite gestionar (instalacion y desinstalacion) del servicio web de apache para sistemas operativos basados en RedHat y Debian.
La instalacion permite preparar un virtual host listo para usar por la aplicacion.

## Requirements

Interprete python en agente
Usuario con privilegios para instalar paquetes, editar configuracion en `/etc/[paquete apache]` y en `[www root]/[vhost]`

## Role Variables

Las siguientes variables se pueden ajustar para dejar el servicio operativo y se encuentran definidas en `defaults/main.yml`
Las variables propias a cada familia de sistema operativo se encuentran en `vars/[sistema operativo].yml`

Puertos a utilizar solo por vhost:

```
	http_default_port: 80
	http_default_ssl_port: 443
```

Carpeta raiz para presentar la aplicacion a traves del servicio web:

```
	http_root_site: "/var/www"
```

Valores por defecto de indice de vhost y nombre de configuracion de apache:

```
	http_vhost_file: "index.html"
	http_vhost_site: "defaultvhost"
```

Valores por defecto de definicion de nombre al que atendera el vhost:

```
	http_vhost_servername: "defaultvhost.labmvl.local"
	http_vhost_serveralias: "vhost.labmvl.local"
```

Si se activa la variable se procede a eliminar el servicio de apache y todos sus archivos generados.

```
	#apache_remove: true`
```

Si se activa la variable apache_remove se debera seleccionar el estado de eliminacion de directorios y archivos relacionados para que el servicio tambien elimine la configuracion completamente:

```
	http_pcks_state: present
	#http_pcks_state: absent
	http_folder_state: directory
	#http_folder_state: absent
```

Variables que verifican el estado del servicio con la finalizacion de la instalacion y configuracion:

```
	http_state: started
	http_restart_state: restarted
```

Habilitacion de puertos servicio firewalld para familia de sistemas basados en RedHat. Se puede eliminar regla al eliminar paquetes y configuracion:

```
	firewalld_service: started
	firewalld_restart_state: restarted
	firewalld_http_rule: enabled
```

Dependencies
------------

Sin dependencias, pero es conveniente utilizar otros roles de instalacion de interpretes acompañados de este servicio.

Example Playbook
----------------

Playbook de ejemplo para eliminar el servicio web por completo:

```
- hosts: linux
  become: yes
  roles:
  - role: mauriciomem.apache
    apache_remove: true
    http_pcks_state: absent
    http_folder_state: absent
    firewalld_http_rule: disabled
```

Playbook de ejemplo para instalar el servicio de apache y configurar el vhost `webfoo.example.com.ar`

```
- hosts: linux
  become: yes
  roles:
  - role: mauriciomem.apache
    http_root_site: "/opt/www"
    http_vhost_file: "index.html"
    http_vhost_site: "webfoo"
    http_vhost_servername: "webfoo.example.com.ar"
    http_vhost_serveralias: "www.webfoo.example.com.ar"
```

License
-------

BSD

Author Information
------------------

mmitolo
