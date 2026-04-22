FROM eclipse-temurin:21-jdk-alpine AS build
WORKDIR /app
COPY gradlew .
COPY gradle gradle
RUN chmod +x gradlew
COPY build.gradle settings.gradle ./
RUN ./gradlew dependencies --no-daemon || true
COPY src ./src
RUN ./gradlew clean build -x test --no-daemon

FROM eclipse-temurin:21-jre
WORKDIR /app
COPY wait-for-it.sh wait-for-it.sh
RUN chmod +x wait-for-it.sh
COPY --from=build /app/build/libs/*.jar app.jar
RUN apt-get update && apt-get install -y curl
EXPOSE 8081
ENTRYPOINT ["java", "-jar", "app.jar"]
