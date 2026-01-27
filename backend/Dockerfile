# 1) Build stage
FROM gradle:9.2.1-jdk17 AS builder
WORKDIR /workspace

COPY build.gradle settings.gradle ./
COPY gradle ./gradle
COPY src ./src

RUN gradle --no-daemon clean bootJar -x test

# 2) Run stage
FROM eclipse-temurin:17-jre
WORKDIR /app
COPY --from=builder /workspace/build/libs/*.jar app.jar

EXPOSE 8080
ENV PORT=8080
ENTRYPOINT ["sh","-c","java $JAVA_TOOL_OPTIONS -Dserver.address=0.0.0.0 -Dserver.port=${PORT} -jar /app/app.jar"]
