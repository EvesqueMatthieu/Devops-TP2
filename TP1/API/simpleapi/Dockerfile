
    # Build #
  
# On recupère maven et on le tag "maven"
FROM maven:3.8.5-openjdk-17 AS maven
# On créer un working directory
ENV WORKDIR /opt/api
WORKDIR $WORKDIR
# On copy pom.xml dans le repertoir courant du container
COPY pom.xml .
# On copy les source dans le repertoir ./src container
COPY src ./src
# On compile le tout sans faire de test
RUN mvn package -DskipTests

    # Run #
  
# On recupère le runtime (JRE)
FROM openjdk:17-jdk
ENV WORKDIR /opt/api
WORKDIR $WORKDIR

COPY --from=maven $WORKDIR/target/*.jar $WORKDIR/api.jar

#on execute
ENTRYPOINT java -jar api.jar
