# Deploying Django with Docker Compose

https://www.youtube.com/watch?v=mScd-Pc_pX0

Creamos la imagen

```shell
docker compose build
```

Lanzamos servidor de desarrollo

```shell
docker compose up
```

## Para interactuar con el contenedor

Para ver ID del contenedor

```shell
docker ps
```

```shell
docker exec -it e57bb524e558 /bin/sh
```
Tip: sh porque alpine usa sh y no bash

## Una vez dentro de la consola del contenedor puedo ejecutar las migraciones

```shell
python manage.py createsuperuser
```

# Otra forma de interactuar con el contenedor

```shell
docker compose run -rm app sh -c "python manage.py createsuperuser"
```

# Deploy

Para evitar algun conflicto de contenedor

```shell
docker compose -f docker-compose-deploy.yml down --volumes
```

Construimos la imagen para producción

```shell
docker compose -f docker-compose-deploy.yml build
```

Lanzamos servidor de producción

```shell
docker compose -f docker-compose-deploy.yml up
```

Para asegurarnos de que podemos subir imágenes en modo producción

```shell
docker compose -f docker-compose-deploy.yml run --rm app sh -c "python manage.py createsuperuser"
```