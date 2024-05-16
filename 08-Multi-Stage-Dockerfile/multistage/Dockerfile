# Étape de build
FROM openjdk:8 AS build

# Mettre à jour les paquets et installer Maven
RUN apt-get update && apt-get install -y maven git && apt-get clean

# Cloner le dépôt
RUN git clone -b vp-docker https://github.com/imranvisualpath/vprofile-repo.git

# Définir le répertoire de travail
WORKDIR /vprofile-repo

# Construire le projet Maven
RUN mvn install

# Étape de production
FROM tomcat:8-jre11

# Supprimer les applications web par défaut de Tomcat
RUN rm -rf /usr/local/tomcat/webapps/*

# Copier le fichier WAR généré depuis l'étape de build
COPY --from=build /vprofile-repo/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

# Exposer le port 8080
EXPOSE 8080

# Commande pour démarrer Tomcat
CMD ["catalina.sh", "run"]
