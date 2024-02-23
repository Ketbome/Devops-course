FROM nginx:stable-alpine

VOLUME /tmp

#Install web app on nginx server
RUN rm -rf /usr/share/nginx/html/*

COPY nginx.conf /etc/nginx/nginx.conf
COPY dist/billingApp /usr/share/nginx/html
COPY mime.types /etc/nginx/mime.types

#Install OpenJDK 17
RUN apk --no-cache add openjdk17-jre

#Define the environment variable
ENV JAVA_HOME /usr/lib/jvm/java-17-openjdk
ENV PATH $JAVA_HOME/bin:$PATH

#Verify the installation
RUN java -version

#Install java microservice
ENV JAVA_OPTS=""
ARG JAR_FILE
ADD ${JAR_FILE} app.jar

#Copy the init script
COPY appshell.sh appshell.sh

#Expose the port
EXPOSE 80 7080

#Run the jar file
ENTRYPOINT ["sh", "appshell.sh"]