Con compose, podemos levantar una base de datos con persistencia y una imagen con todas las dependencias que requiere mapuche instaladas.

El servicio mapuche persitirá la carpeta donde se intala Toba, permitiendo mantener la instalacion

Si nunca se levantó el servicio, primero hay que instalar. Levantar todo con

```bash
docker-compose up -d
```

Instalar Mapuche y Toba

```bash
docker container exec -t mapuche_mapuche_1 ./bin/instalador proyecto:instalar --crear-db --no-interaction
```

Después de instalar, dar los permisos

```bash
docker container exec -t mapuche_mapuche_1 ./bin/instalador permisos:simple -U root --no-interaction
```

Después, habilitar el sitio en apache

```bash
docker container exec -t mapuche_mapuche_1 ln -s /srv/mapuche/toba-data/instalacion/toba.conf /etc/apache2/sites-enabled/mapuche.conf && service apache2 reload
```

