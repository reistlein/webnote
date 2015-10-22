
# Jenkins
1. Quelque information a propos du format markdown
  * https://help.github.com/articles/github-flavored-markdown/
2. Docker cheat-sheet
  * https://github.com/wsargent/docker-cheat-sheet
3. Docker Hub
  * https://hub.docker.com/explore/
4. Des informations sur La partie client de Jenkins
  * https://wiki.jenkins-ci.org/display/JENKINS/Jenkins+CLI

Créer un conteneur pour Jenkins avec le repertoire jenkins_home
```
{
  cd /mnt/sda3/docker/2015_10_08_jenkins;
  mkdir jenkins_home;
  DOCKER_HOST=$(ip -4 addr show docker0 | grep -Po 'inet \K[\d.]+')
  docker run -p 8080:8080 -p 50000:50000 --name jenkins   -v $(pwd)/jenkins_home:/var/jenkins_home --add-host host-docker:${DOCKER_HOST} jenkins ;
}
```
Pour se connecter à un container démarrer
```
sudo docker exec -i -t 665b4a1e17b6 bash
```

#### Lister les plugins déja installé
```
{
java -jar jenkins-cli.jar -s http://localhost:8080/ list-plugins  | cut -d " " -f 1 | xargs
}
```

#### Installer les plugins sans click
```
java -jar jenkins-cli.jar -s http://localhost:8080/ install-plugin Parameterized-Remote-Trigger remote-jobs-view-plugin backup show-build-parameters job-import-plugin pegdown-formatter  Parameterized-Remote-Trigger remote-jobs-view-plugin  -deploy
```
Gestion des Jenkins distant: Parameterized-Remote-Trigger remote-jobs-view-plugin

Gestion de la configuration de Jenkins: backup job-import-plugin

Posssibilité de mettre des une description avec des liens HTML vers l'application: job-import-plugin pegdown-formatter

#### Récupéré la configuration d'un job (ici get_log)
java -jar jenkins-cli.jar -s http://localhost:8080/ get-job get_log

### Copier un JOB (via un create job)
java -jar jenkins-cli.jar -s http://localhost:8080/ get-job get_log | java -jar jenkins-cli.jar -s http://localhost:8080/ create-job copy

### Lancer un job avec des paramètres
java -jar jenkins-cli.jar -s http://localhost:8080/ build get_log -p configuration=ALL_LOGS  -s -v 2>/dev/nulljava -jar jenkins-cli.jar -s http://localhost:8080/ build get_log -p configuration=ALL_LOGS  -s -v 2>/dev/null

