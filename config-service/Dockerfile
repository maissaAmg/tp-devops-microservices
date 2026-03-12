# Étape build
FROM eclipse-temurin:21-jdk AS build
WORKDIR /app
COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .
RUN chmod +x mvnw
RUN ./mvnw dependency:go-offline -B
COPY src src
RUN ./mvnw package -DskipTests -B

# Étape runtime
FROM eclipse-temurin:21-jre
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*
WORKDIR /app

# Copier le JAR
COPY --from=build /app/target/*.jar app.jar

# Copier le dossier de configuration
COPY src/main/resources/configurations /configurations

# Définir le profil et la config location
ENV SPRING_PROFILES_ACTIVE=native
ENV SPRING_CONFIG_LOCATION=file:/configurations/

EXPOSE 8888
ENTRYPOINT ["java", "-jar", "app.jar"]
