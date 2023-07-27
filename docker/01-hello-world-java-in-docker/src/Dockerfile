FROM ubuntu:latest

WORKDIR /app

COPY HelloWorld.java ./

RUN apt update 

RUN apt install -y default-jdk

RUN javac HelloWorld.java

RUN echo "Main-Class: HelloWorld" > manifest.txt 

RUN jar cvfm HelloWorld.jar manifest.txt HelloWorld.class

CMD ["java", "-jar", "/app/HelloWorld.jar"]