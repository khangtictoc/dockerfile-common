FROM docker.io/library/maven:3.9.6-eclipse-temurin-21 AS builder

WORKDIR /app

COPY pom.xml .
RUN mvn dependency:go-offline -B

COPY src ./src

ARG RUN_TESTS=false
RUN if [ "$RUN_TESTS" = "true" ]; then \
        mvn clean package; \
    else \
        mvn clean package -DskipTests; \
    fi

# Final image
FROM eclipse-temurin:21-jre-jammy

WORKDIR /app

RUN addgroup --system appgroup && adduser --system --ingroup appgroup appuser

COPY --from=builder /app/target/*.jar app.jar

RUN chown appuser:appgroup app.jar

USER appuser

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]