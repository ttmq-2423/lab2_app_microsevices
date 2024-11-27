FROM openjdk:8-alpine

EXPOSE 8083
WORKDIR /usr/src/app

COPY pom.xml mvnw ./
COPY .mvn/ ./.mvn

RUN chmod -R 755 /usr/src/app/mvnw
RUN ./mvnw dependency:resolve

COPY . .
RUN chmod -R 755 /usr/src/app/mvnw
RUN ./mvnw install

CMD ["java", "-jar", "./target/users-api-0.0.1-SNAPSHOT.jar"]
