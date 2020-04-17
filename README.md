# Mapuche con Docker
Con los archivos de este repositorio, se puede levantar Mapuche en contenedores y trabajar sobre estos, sin tener que instalar nada en el host. Los contenedores son tres:
* Uno con el código fuente de Mapuche y todas las dependencias del sistema instaladas
* Servidor de Postgres
* Adminer
El contenedor con el código de Mapuche no tiene la instalción realizada, y la BD se encuentra vacía inicialmente. Cada contenedor tiene volúmenes asociados para la persistencia de los datos.

# Instalación
Los valores de configuración se encuentran en el archivo `.env` en la carpeta raíz del repositorio. Los contenedores serán configurados con estos valores.
El código fuente de Mapuche debe descargarse del sitio de SIU. Debe colocarse en la carpeta `mapuche_391`.

Para instalar, levantar todo con compose

```bash
docker-compose up -d
```

Instalar Mapuche y Toba

```bash
docker-compose exec mapuche ./bin/instalador proyecto:instalar --crear-db --no-interaction --sin-mantenimiento
```

Después de instalar, dar los permisos

```bash
docker-compose exec mapuche ./bin/instalador permisos:simple -U root --no-interaction
```

Después, habilitar el sitio en apache

```bash
docker-compose exec mapuche ln -s /toba-data/instalacion/toba.conf /etc/apache2/sites-enabled/mapuche.conf && service apache2 reload
```

En este punto, ya se encuentra Mapuche instalado y listo para personalizar. La instalación y las personalizaciones quedan persitidas en volúmenes.

# Mantenimiento
Para correr comandos en la base,

```bash
docker-compose exec db psql -U postgres -c "<command>"
```

Se puede inspeccionar la base a través de la interfaz web que provee el `adminer`

