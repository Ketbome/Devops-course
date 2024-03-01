# Cosas del curso

## Docker: trabajar con contenedores e imagenes

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

## Docker Compose: Orquestacion simple y buenas practicas

> [!NOTE]
> La version de Docker Compose no puede ser superior a la del Docker engine **[Docker versioning](https://docs.docker.com/compose/compose-file/compose-versioning/)**
