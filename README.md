# Cómo instalar Apache Superset en una instancia de GCP usando la imagen de pública de Docker

<img src="img/s.png" />

Cambiando el usuario a root y actualizando los repositorios. Luego, se instalan las dependencias para los pasos siguientes.
``` bash
sudo su
apt-get update
apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg2 \
    software-properties-common
```

Agregando la llave GPC oficial de Docker y verificando la adición de la key
``` bash
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -

apt-key fingerprint 0EBFCD88
```

Agregando el repositorio con la versión estable. Se puede instalar las versiones de prueba modificando ligeramente esta linea. Luego, actualizando el indice del paquete apt.
``` bash
add-apt-repository \
  "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"

apt-get update
```

Ahora, instalando docker-CE y docker-ce-cli con las versiones para Debian
``` bash
apt-get install docker-ce=5:18.09.1~3-0~debian-stretch docker-ce-cli=5:18.09.2~3-0~debian-stretch containerd.io
```

Instalando docker-compose
``` bash
curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
```

Una vez se tiene instalado docker-compose se clona el repositorio de Github de Superset y se ingresa a la ruta donde están los archivos de Docker (Dockerfile, docker-compose.yml, etc). Desde esa ubicación se hace un docker-compose para construir la imagen y los demás servicios. Finalmente, se sube el servicio.
``` bash
git clone https://github.com/apache/incubator-superset/
cd incubator-superset/contrib/docker
docker-compose run --rm superset ./docker-init.sh
docker-compose up
```

Hay que hacer una modificación en el Dockerfile: Hay que eliminar la línea en donde se define el usuario (USER superset). También hay que modificar el archivo docker-entrypoint.sh y agregar una línea antes de la definición de APP de Flash.
