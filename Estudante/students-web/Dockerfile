FROM maven:3.9-eclipse-temurin-17 AS build

WORKDIR /students-web

COPY pom.xml .
COPY src ./src

RUN mvn package -DskipTests

FROM tomcat:11.0-jdk17

RUN rm -rf /usr/local/tomcat/webapps/ROOT*

COPY --from=build /students-web/target/students-web.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080