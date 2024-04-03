FROM maven:3.9.6-eclipse-temurin-17 as step01
WORKDIR /app
COPY . .
# RUN mvn package
RUN --mount=type=cache,target=/root/.m2,rw mvn package

FROM eclipse-temurin:17.0.10_7-jdk-focal
WORKDIR /myapp
COPY --from=step01 /app/target/*.jar /myapp/app.jar
EXPOSE 8080
CMD ["java", "-jar", "app.jar"]