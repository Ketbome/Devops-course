# Usar una imagen base de eclipse-temurin
FROM eclipse-temurin:17-alpine

# Agregar un grupo devopsc
RUN addgroup devopsc

# Agregar un usuario 'admin' y lo asigna al grupo 'devopsc'
RUN adduser -D -G devopsc admin

# Crear un nuevo punto de montaje en /tmp para el volumen
VOLUME /tmp

# Copiar el archivo jar
ARG JAR_FILE

# Variable de entorno de DB_PASSWORD
ARG DB_PASSWORD

# Agregar la variable de entorno
ENV DB_PASSWORD=$DB_PASSWORD

# Copiar el archivo jar al contenedor
COPY ${JAR_FILE} /tmp/app.jar

# Cambiar el propietario del archivo jar
RUN chown -R admin:devopsc /tmp/app.jar

# Cambiar el usuario al usuario 'admin'
USER admin

# Comando de inicio
CMD ["java", "-jar", "/tmp/app.jar"]