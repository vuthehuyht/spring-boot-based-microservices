# stage 1
# Start with a base image containing Java runtime
# TODO :: Upgrade to slim jre version for 17 once available
FROM 3.9.3-eclipse-temurin-11-alpine as builder
WORKDIR application
RUN mvn install
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} application.jar
RUN java -Djarmode=layertools -jar application.jar extract

# the second stage of our build will copy the extracted layers
FROM 3.9.3-eclipse-temurin-11-alpinee
WORKDIR application
COPY --from=builder application/dependencies/ ./
COPY --from=builder application/spring-boot-loader/ ./
COPY --from=builder application/snapshot-dependencies/ ./
COPY --from=builder application/application/ ./

EXPOSE 8080

ENTRYPOINT ["java", "org.springframework.boot.loader.JarLauncher"]