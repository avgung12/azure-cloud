#
# Build stage
#
FROM maven:3.9-eclipse-temurin-8 AS build
WORKDIR /build
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

#
# Package stage(sS)
#
FROM eclipse-temurin:8-jre

WORKDIR /app

RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*
RUN curl -fsSL "https://repo1.maven.org/maven2/com/microsoft/azure/applicationinsights-agent/3.4.12/applicationinsights-agent-3.4.12.jar" -o /app/applicationinsights-agent-3.4.12.jar

COPY --from=build /build/target/*.jar /app/petstoreorderservice.jar
COPY applicationinsights.json /app/applicationinsights.json
EXPOSE 8080
ENTRYPOINT ["java", "-javaagent:applicationinsights-agent-3.4.12.jar", "-jar", "petstoreorderservice.jar"]
