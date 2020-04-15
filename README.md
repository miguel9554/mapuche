# Instalación
Con compose, podemos levantar una base de datos con persistencia y una imagen con todas las dependencias que requiere mapuche instaladas.

El servicio mapuche persitirá la carpeta donde se intala Toba, permitiendo mantener la instalacion

Si nunca se levantó el servicio, primero hay que instalar. Levantar todo con

```bash
docker-compose up -d
```

Instalar Mapuche y Toba

```bash
docker-compose exec mapuche ./bin/instalador proyecto:instalar --crear-db --no-interaction
```

Después de instalar, dar los permisos

```bash
docker-compose exec mapuche ./bin/instalador permisos:simple -U root --no-interaction
```

Después, habilitar el sitio en apache

```bash
docker-compose exec mapuche ln -s /toba-data/instalacion/toba.conf /etc/apache2/sites-enabled/mapuche.conf && service apache2 reload
```

# Mantenimiento
Para correr comandos en la base,

```bash
docker-compose exec db psql -U postgres -c "<command>"
```
