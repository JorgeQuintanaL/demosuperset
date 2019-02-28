<p align = “center”><img src=“img/super_gcp.png” /></p>

### Prueba de Concepto de Apache Superset sobre un ambiente en Google Cloud 

Para esta prueba se usa un conjunto de datos obtenidos en [Datos Abiertos](https://www.datos.gov.co/browse?q=Observatorio%20de%20paz&sortBy=relevance). Dicho conjunto de datos se divide en dos partes:

	* Características de las Victimas del Conflicto Armada
	* Información sobre los Auxilios Económicos recibidos

Cada uno de ellos se obtuve usando la API de Datos Abiertos y con base en la documentación sobre los campos se hizo un trabajo de cruce y limpieza de datos en Python. El resultado de dicho proceso se carga a PostgreSQL para usarlos como base para la prueba de **Superset**. Para una descripción detallada de las transformación y el cruce de los datos revisar el Notebook superset_demo.ipynb. A continuación se dan las pautas para el proceso de instalación sobre una instancia de Google Cloud haciendo uso de la imagen pública de Superset en Docker. (Para mayor información sobre el proceso de instalación y las dependencias revisar [Apache Superset Docs]([Apache Superset (incubating) — Apache Superset  documentation](https://superset.incubator.apache.org/)))

### Instalación:

Primero, se debe crear una instancia en GCP. Luego, sobre dicha maquina vamos a instalar Docker, para esto se deben seguir los siguientes pasos:

	* Cambiar el usuario a root y actualizando los repositorios
	* Instalar las dependencias necesarias
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

	* Agregar la llave oficial de Docker y verificar el proceso
``` bash
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -

apt-key fingerprint 0EBFCD88
```

	* Agregar el repositorio con la versión de Docker estable. Se puede instalar las versiones de prueba modificando ligeramente esta linea
	* Actualizar el índice del paquete apt
``` bash
add-apt-repository \
  "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"

apt-get update
```

	* Instalar docker-CE y docker-ce-cli con las versiones para Debian
``` bash
apt-get install docker-ce=5:18.09.1~3-0~debian-stretch docker-ce-cli=5:18.09.2~3-0~debian-stretch containerd.io
```

	* Instalar docker-compose
	* Dar permisos de ejecución a docker-compose
``` bash
curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
```

	* Clonar el repositorio de Github de Superset
	* Ir a la ruta incubator-superset/contrib/docker
	* Hacer docker-compose para construir la imagen y los demás servicios
	* Finalmente, subir el servicio
``` bash
git clone https://github.com/apache/incubator-superset/
cd incubator-superset/contrib/docker
docker-compose run --rm superset ./docker-init.sh
docker-compose up
```

### Problemas con la instalación:

Algunos usuarios han presentado problemas con el proceso de instalación. Para resolver dichos problemas se recomienda leer [Contributing Guidelines]([incubator-superset/CONTRIBUTING.md at master · apache/incubator-superset · GitHub](https://github.com/apache/incubator-superset/blob/master/CONTRIBUTING.md))