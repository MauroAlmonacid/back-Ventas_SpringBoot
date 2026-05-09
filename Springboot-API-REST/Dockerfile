FROM maven:3.8.5-openjdk-17-slim AS build
WORKDIR /app

COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

FROM eclipse-temurin:17-jre-jammy
WORKDIR /app

RUN addgroup --system javaspring && adduser --system javaspring --ingroup javaspring

COPY --from=build /app/target/*.jar app.jar
RUN chown javaspring:javaspring app.jar
USER javaspring

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]