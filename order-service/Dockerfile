# ---------- Stage 1 : Build ----------
FROM maven:3.9.9-eclipse-temurin-17 AS build
WORKDIR /app
# Copy only pom first (for caching)
COPY order-service/pom.xml .
RUN mvn -B -q -e -DskipTests dependency:go-offline
# Copy source
COPY order-service/src ./src
# Build jar
RUN mvn clean package -DskipTests

# ---------- Stage 2 : Runtime ----------
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","app.jar"]
