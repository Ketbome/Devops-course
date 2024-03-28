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

- Pods: Unidades básicas de despliegue y gestión, que agrupan uno o más contenedores con almacenamiento y red compartidos.
- Deployments: Describen el estado deseado de una implementación, gestionando las réplicas de la aplicación.
- Services: Definen cómo exponer una aplicación ejecutada en un conjunto de pods, actuando como balanceador de carga.
- ConfigMaps: Permiten desacoplar la configuración de las imágenes, almacenando variables de entorno, argumentos de línea de comandos y configuración de volúmenes.
- Labels: Pares clave-valor utilizados para organizar, seleccionar, consultar y monitorear objetos de manera eficiente.
- Selectores: Mecanismos para realizar consultas basadas en etiquetas y filtrar objetos según criterios específicos.

### Uso de minikube para simular

Los comandos basicos:

```bash
minikube start

minikube pause

minikube unpause

minikube stop

minikube config set memory 16384

minikube addons list

minikube start -p aged --kubernetes-version=v1.16.1

minikube delete --all
```

Tambien se tiene un dashboard:

```bash
minikube dashboard --url
```

### Kubetcl

#### Iniciar una imagen ya creada en un pod

```bash
kubectl run kbillingapp --image=sotobotero/udemy-devops:0.0.1 --port=80 80
```

Ver los pod

```bash
kubectl get pod
```

Ver la descripcion del pod

```bash
kubectl describe pod kbillingapp
```

Exponer el puerto para que lo vea el cliente

```bash
kubectl expose pod kbillingapp --type=LoadBalancer --port=8080 --target-port=80
```

Eliminar el pod

```bash
kubectl delete -n default pod kbillingapp
```

#### Crear los objetos de un cluster

Para hacerlo poner todos los archivos uno por uno despues de la -f, Ejemplo:

```bash
kubectl apply -f .\secret-dev.yaml
```

Idealmente en este orden

- Variables de entorno
- ConfigMap
- Almacenamiento/Volumes
- Deployments
- Services

Para eliminar:

```bash
kubectl delete -f .\
```

## Entrega continua(CI) y Despliegue continuo

### Jenkins

Crear la imagen con jenkins y maven

```bash
docker build -t jenkins-cicd --no-cache .
```

Correr la imagen con sus respectivos puertos

```bash
docker run -d -p 8080:8080 -p 50000:50000 --name jenkins jenkins-cicd
```
