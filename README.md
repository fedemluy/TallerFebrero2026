# TallerFebrero2026 
# DOCUMENTO EN DESARROLLO! Momentaneamente No USAR

Proyecto git de infraestructura como codigo. usando los temas dados en el taller Linux donde vamos a crear una infraestructura mínima y reproducible, automatizada con Ansible. La cual va a contar con un Server NFS y 2 clientes que van a montar automaticamente la particion de dicho server. 

## Infraestrucura general.
Vamos a contar con 4 servidores sin entorno grafico, los cuales van a estar compuestos por 2 Centos Stream 9 y 2 Ubuntu Server 24.04. Ademas de una maquina bastion el cual va a estar utilizando Centos Stream 9 con entorno grafico.

## Topologia de red general

<img width="882" height="515" alt="image" src="https://github.com/user-attachments/assets/edee1321-7bf0-4cf8-8a29-e1492a4c11d1" />

Todos los equipos van a estar conectados atravez de un router el cual brinda servicio dhcp y por el cual podemos navegar a internet y una segunda interface utilizada como lan local,

## Diagrama de la red Lan local (las ips pueden variar, se debe modificar en el inventario)

<img width="871" height="423" alt="image" src="https://github.com/user-attachments/assets/45827ec3-6f4f-4a65-968c-a9ab6277cfb8" />


## Primeros pasos, preparacion del bastion

Generamos una clave privada/publica ssh para poder ingresar a los equipos de manera segura y copiamos la llave publica a todos los equipos.
```bash
ssh-keygen -t ed25519
ssh-copy-id sysadmin@192.168.10.11
ssh-copy-id sysadmin@192.168.10.12
ssh-copy-id sysadmin@192.168.10.21
ssh-copy-id sysadmin@192.168.10.22
```

Segundo, instalamos el modulo de [ansible](https://docs.ansible.com/projects/ansible/latest/getting_started/index.html).
```bash
sudo dnf -y install ansible-core
```

Tercero, instalamos git.
```bash
sudo dnf install git -y
```
Pre-configuracion Recomendada:
```bash
Tu nombre:
git config --global user.name "Tu Nombre"

Tu correo:
git config --global user.email "tu@email.com"
```

Cuarto, descargamos nuestro proyecto Git, dentro [del proyecto](https://github.com/fedemluy/TallerFebrero2026#) vamos al boton <>CODE y copiamos la URL. Puede ser HTTP [https://github.com/fedemluy/TallerFebrero2026.git](https://github.com/fedemluy/TallerFebrero2026.git) 

Luego hacemos un git clone de la url
```bash
 git clone https://github.com/fedemluy/TallerFebrero2026.git
```

## Dentro del proyecto podemos encontrar la siguente estructura 

```bash
.
├── collections
│   └── requirements.yaml
├── file
│   ├── auto.nfs
│   └── nfs.autofs
├── inventories
│   ├── group_vars
│   │   └── linux.yaml
│   └── hosts.ini
├── LICENSE
├── playbooks
│   ├── hardening.yaml
│   ├── nfsclient.yaml
│   └── nfsserver.yaml
├── README.md
└── templates
    └── README-NFS.j2
```

La cual esta organizada en diferentes secciones: (por carpeta).
En collections vamos a encontrar un archivo requirements.yaml el cual tiene identificado todos los modulos que No son aparte del core, los cuales tenemos que instalar para que funcione correctamente el proyecto. 

## Para la instalacion de los modulos vamos a utilizar 

Sitio oficial de Ansible para ver mas infomracion sobre [Modulos](https://docs.ansible.com/projects/ansible/latest/collections/ansible/builtin/file_module.html)

```bash
ansible-galaxy collection install -r collections/requirements.yaml
```

Dentro de la carpeta File, vamos a encontrar los archivos de configuracion que vamos a copiar en los equipos de forma remota a traves de los diferentes playbooks

En inventories, vamos a encontrar un archivo ini con el inventario de los equipos.

En playbook los diferentes archivos que van a realizar las tareas que deseamos
En templates vamos a encontrar una plantilla o molde, con el cual vamos a generar archivos para poder hacer las pruebas y verificaciones. 

## Uso de playbook
Para ejecutar el playbook vamos a usar el comando ansible-playbook con la opcion -i para pasar el archivo donde tenemos el inventario de equipos en los cuales vamos a ejecutar el playbook deseado, igualmente dentro del playbook podemos tener definido que grupo o host dentro del inventario vamos a ejecutar. Despues vamos a selecionar el archivo con el playbook deseado y ademas vamos a agregar --ask-become-pass para que nos solicite la contraseña para realizar dicha tarea

## Ejecutar un harderinig basico en los equipos
```bash
ansible-playbook -i inventories/hosts.ini playbooks/hardening.yaml --check --ask-become-pass
```

## Preparar el servidor NFS
```bash
ansible-playbook -i inventories/hosts.ini playbooks/nfsserver.yaml --ask-become-pass
```
## Probar el servidor NFS, abre el archivo que generamos con el template
```bash
ansible -i inventories/hosts.ini fileserver -m command -a 'cat /srv/nfs/shared/README-NFS.txt'
```

## Preparar los clientes para que monten el servidor NFS
```bash
ansible-playbook -i inventories/hosts.ini playbooks/nfsclient.yaml --ask-become-pass
```



```python
```

