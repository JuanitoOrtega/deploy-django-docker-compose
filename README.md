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

Ver logs

```shell
docker compose -f docker-compose-deploy.yml logs
```

Si actualizamos nuestro código

1. Desde nuestro ambiente de desarrollo hacemos un push
2. Desde la terminal de producción hacemos un pull para traernos los cambios
3. Reconstruimos la imagen de nuestra app

```shell
docker compose -f docker-compose-deploy.yml build app
```

4. Lanzamos nuestro servidor en segundo plano sin afectar las dependencias, base de datos o el proxy

```shell
docker compose -f docker-compose-deploy.yml up --no-deps -d app
```