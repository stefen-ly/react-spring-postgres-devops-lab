# Stage 1: Build Spring Boot
FROM gradle:8.6-jdk17 AS builder

WORKDIR /home/gradle/project

COPY --chown=gradle:gradle build.gradle settings.gradle ./
COPY --chown=gradle:gradle src ./src

RUN gradle --no-daemon --max-workers=1 \
    -Dorg.gradle.jvmargs="-Xmx384m -XX:MaxMetaspaceSize=192m" \
    bootJar -x test

# Stage 2: Run Spring Boot
FROM eclipse-temurin:17-jre-alpine

WORKDIR /app

COPY --from=builder \
    /home/gradle/project/build/libs/*.jar \
    /app/app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "/app/app.jar"]