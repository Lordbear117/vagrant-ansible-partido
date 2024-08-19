# Generación de Partidos de Tenis con Vagrant y Ansible

## Descripción

Esta solución utiliza **Vagrant** y **Ansible** para generar automáticamente un archivo de información de partidos de tenis. La configuración incluye el uso de una red privada y un rol de Ansible para crear el archivo `partido.txt` con los datos de dos tenistas. La maquina host es una Laptop Acer con **Windows 11 Pro** y la maquina virtual se creo con **Oracle VM Virtual Box** además  de usar **Ubuntu bionic64**.

## Estructura de Directorios

La estructura del proyecto es la siguiente:

- **`ansible/playbook.yml`**: Archivo principal del playbook de Ansible.
- **`ansible/roles/partido/tasks/main.yml`**: Tareas definidas para el rol de `partido`.
- **`ansible/roles/partido/vars/partido_vars.yml`**: Archivo de variables específicas del rol.
- **`ansible/roles/partido/templates/partido_template.j2`**: Plantilla Jinja2 para generar el archivo `partido.txt`.
- **`Vagrantfile`**: Archivo de configuración de Vagrant.

## Configuración del Entorno

1. **Asegúrate de tener instalados los siguientes paquetes:**
   - **Vagrant**: Herramienta para la gestión de entornos virtuales.
   - **VirtualBox**: Plataforma de virtualización.
   - **Ansible**: Herramienta para la automatización de configuración.

2. **Configuración del `Vagrantfile`:**

   El `Vagrantfile` configura una máquina virtual con Ubuntu, una red privada y el provisioning con Ansible. Asegúrate de que el archivo se vea así:

   ```ruby
   Vagrant.configure("2") do |config|
     config.vm.box = "ubuntu/bionic64"

     # Configurar red privada con una IP estática
     config.vm.network "private_network", ip: "192.168.56.10"

     config.vm.provider "virtualbox" do |vb|
       vb.memory = "1024"
       vb.cpus = 2
     end

     # Aprovisionamiento usando Ansible
     config.vm.provision "ansible_local" do |ansible|
       ansible.playbook = "ansible/playbook.yml"
     end
   end

## Ubicación del Archivo Compartido

La carpeta compartida `/vagrant` en la máquina virtual mapea el directorio `project-root` en tu máquina anfitrión. Puedes acceder a `/vagrant` desde la máquina virtual para verificar o modificar archivos.

## Comandos de Vagrant

### Levantar la Máquina Virtual y Ejecutar el Playbook de Ansible

Ejecuta el siguiente comando en el directorio raíz del proyecto:

```bash
vagrant up
```

### Destruir la Máquina Virtual
Si necesitas destruir la máquina virtual y eliminar todos los cambios, ejecuta:
```bash
vagrant destroy
```

### Acceder a la maquina virtual mediante SSH
Si necesitas acceder la máquina virtual mediante SSH y llave privada usando Vagrant, ejecuta:
```bash
vagrant ssh
```

## Uso del Rol `partido`

El rol `partido` está diseñado para realizar las siguientes tareas:

### Leer Variables

- `first_name`, `last_name`, `city` se definen directamente en el playbook de Ansible.
- `id` se define en el archivo `partido_vars.yml` dentro de `roles/partido/vars/`.

### Obtener Datos del Segundo Tenista

- La información se obtiene de la API [https://random-data-api.com/api/users/random_user?size=1](https://random-data-api.com/api/users/random_user?size=1).

### Generar el Archivo `partido.txt`

- Usa la plantilla `partido_template.j2` para generar el archivo con los datos de los tenistas y la fecha actual.

## Resolución de Problemas

### Conflictos de IP en la Red Privada

- Si encuentras errores relacionados con IPs en conflicto, asegúrate de que solo una red "Host-only" en VirtualBox esté configurada con DHCP, o configura una IP estática en el `Vagrantfile`.

### Problemas con Ansible

- Asegúrate de que el archivo `partido_vars.yml` esté correctamente ubicado y que las variables sean accesibles.

Para más información, consulta la [documentación de Vagrant](https://www.vagrantup.com/docs) y la [documentación de Ansible](https://docs.ansible.com/ansible/latest/index.html).

Este README cubre los aspectos necesarios para levantar y entender la solución con Vagrant y Ansible, incluyendo la estructura del proyecto, configuración del `Vagrantfile`, comandos básicos de Vagrant, y el uso del rol `partido`.
