FROM openjdk:8 AS builder
COPY . /usr/src/app/
#RUN mvn -f /usr/src/app/pom.xml package
#WORKDIR /usr/src/app/target/dependency
WORKDIR /usr/src/app/
RUN ./mvnw package
WORKDIR target/dependency/
RUN jar -xf ../*.jar

FROM openjdk:8-jdk-alpine
RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring
ARG DEPENDENCY=/usr/src/app/target/dependency
COPY --from=builder ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=builder ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=builder ${DEPENDENCY}/BOOT-INF/classes /app
ENTRYPOINT ["java","-cp","app:app/lib/*","hello.Application"]
