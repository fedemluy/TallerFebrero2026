# TallerFebrero2026

Proyecto git de infraestructura como codigo. usando los temas dados en el taller Linux donde vamos a crear una infraestructura mínima y reproducible, automatizada con Ansible. La cual va a contar con un Server NFS y 2 clientes que van a montar automaticamente la particion de dicho server. 

## Infraestrucura general.
Vamos a contar con 4 servidores sin entorno grafico, los cuales van a estar compuestos por 2 Centos Stream 9 y 2 Ubuntu Server 24.04. Ademas de una maquina bastion el cual va a estar utilizando Centos Stream 9 con entorno grafico.

## Topologia de red general

<img width="882" height="515" alt="image" src="https://github.com/user-attachments/assets/edee1321-7bf0-4cf8-8a29-e1492a4c11d1" />

Todos los equipos van a estar conectados atravez de un router el cual brinda servicio dhcp y por el cual podemos navegar a internet y una segunda interface utilizada como lan local,

## Diagrama de la red Lan local

<img width="871" height="423" alt="image" src="https://github.com/user-attachments/assets/45827ec3-6f4f-4a65-968c-a9ab6277cfb8" />


## Dentro del Github podemos encontrar la estructura del proyecto 

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
En collections vamos a encontrar un archivo requirements.yaml el cual tiene identificado todos los modulos que son aparte del core los cuales tenemos que instalar para que funcione correctamente el proyecto. 

## Para la instalacion de los modulos vamos a utilizar el comando 

Sitio oficial de Ansible para ver mas infomracion sobre [Modulos](https://docs.ansible.com/projects/ansible/latest/collections/ansible/builtin/file_module.html)

```bash
ansible-galaxy collection install -r collections/requirements.yaml
```

## Usage

```python
import foobar

# returns 'words'
foobar.pluralize('word')

# returns 'geese'
foobar.pluralize('goose')

# returns 'phenomenon'
foobar.singularize('phenomena')
```

## Contributing

Pull requests are welcome. For major changes, please open an issue first
to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License

[MIT](https://choosealicense.com/licenses/mit/)
