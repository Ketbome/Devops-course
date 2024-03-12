# Cosas del curso

## Docker: trabajar con contenedores e imagenes

Con `docker info` se puede ver toda la informacion de docker

### Cargar una imagen directamente ya hecha

Se trabaja con una imagen ya creada para el entendimiento de esta seccion

```bash
docker run -p 8080:80 -p 7080:7080 --name conBilling sotobotero/billingapp
```

### Buildear el docker file de billingApp

```bash
docker build -t billingapp --no-cache --build-arg JAR_FILE=target/*.jar .

# Run image
docker run -d -p 80:80 -p 7080:7080 --name localbillingApp billingapp
```

### Subir imagen a un repositorio de docker

```bash
docker tag billingapp ketbome/image-billingapp

# Tambien se puede agregar una version
docker tag billingapp ketbome/image-billingapp:v1

# Push al repositorio (Loguearse anteriormente)
docker push ketbome/image-billingapp
```

### Ver los datos de un contenedor

```bash
docker inspect localbillingApp
```

### Acceder al terminal de un contenedor

```bash
docker exec -it {nombre_contenedor} sh
```

## Docker Compose: Orquestacion simple y buenas practicas

> [!NOTE]
> La version de Docker Compose no puede ser superior a la del Docker engine **[Docker versioning](https://docs.docker.com/compose/compose-file/compose-versioning/)**

### Para inicializar la billingapp con Compose

```bash
docker compose -f {nombre_archivo} up -d
```

### Pausar y eliminar contenedor creado

```bash
# Pausar
docker compose -f .\stack-billing.yml stop

# Remover
docker compose -f .\stack-billing.yml rm
```

## Docker Swarm: Orquestador Avanzado para escalar contenedores y limitar recursos

Para ver las estadisticas de los recursos de forma basica se puede hacer:

```bash
docker stats
```

Inicializar dicker swarm

```bash
docker swarm init
```

### Desplegar y probar stack

Desplegar

```bash
docker stack deploy -c .\stack-billing.yml billing

docker stack ps billing
```

Para poder acceder a la aplicacion hay que acceder por la IP de swarm del nodo que se puede ver en `docker info`

### Desactivar cluster 

```bash
docker stack rm billing
```

## Kubernetes 

