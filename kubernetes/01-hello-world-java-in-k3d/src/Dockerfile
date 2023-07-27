# Stage 1: Build stage
FROM openjdk:11-slim-bullseye AS builder

WORKDIR /app

COPY ./HelloWorld.java ./

RUN echo "Main-Class: HelloWorld" > manifest.txt 

RUN javac HelloWorld.java && jar cvfm HelloWorld.jar manifest.txt HelloWorld.class

# Stage 2: Final stage
FROM openjdk:11-jre-slim-bullseye

WORKDIR /app

COPY --from=builder /app/HelloWorld.jar ./

CMD ["java", "-jar" , "/app/HelloWorld.jar" ]