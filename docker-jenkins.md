
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


### Install Nexus (2015_10_10)
docker run -d -p 8081:8081 --name nexus sonatype/nexus:oss
> Je n'ai pas vue comment rajouter des RPM dans la configuration Nexus, il faudrrait se renseigner pour savoir si c'est possible.

### Install YUM repository avec serveur Web (2015_10_10_yumrepo)

Quelque commande de "run" intermédiaire que j'ai utilisé avant la commande final
```
docker run -d -p 80:80 jpalmier/httpd #démarrage d'un container détaché.
docker run --rm --name test-centos -p 80:80 -t -i jpalmier/httpd bash #démarrage du container en mode intéractif.
```

```
cd /mnt/sda3/docker/2015_10_10_yumrepo
docker build --rm -t jpalmier/httpd .
docker run --rm --name test-centos -p 80:80 -v $(pwd)/httpd-conf:/opt/httpd-conf -v $(pwd)/repodata:/opt/repodata -t -i jpalmier/httpd
docker start test-centos
```
> Le problème rencontré était due à une modification des directives de Apache 2.4.3 http://stackoverflow.com/questions/10351167/apache-client-denied-by-server-configuration/13923526#13923526

```
yumdownloader --resolve --downloaddir=. createrepo httpd
yum install createrepo
```


### Install de Sonar (pour voir les resultat de check)
```
docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube:5.1
```

### Creer des doc Word à la volée

http://poi.apache.org/


### Info Wake on Lan

```
http://www.server-world.info/en/note?os=CentOS_6&p=wakeonlan
arp -a #affiche la table ARP
sudo ether-wake -i enp3s0  08:60:6e:e7:3b:fe  # send magick packets
```
### xfreerdp (RDP client)
Cette commande permet de se connecté à une session windows avec l'activation du clipboard, plus le montage automatique des disques du client.
```
xfreerdp -z  -u marine -p xumdoz13 --plugin cliprdr -g 90% --plugin rdpdr --data disk:home-desktop:~ disk:sda3:/mnt/sda3 disk:sda4:/mnt/sda4 -- -a 32 192.168.1.8
```
0000549866709

-----
# Archive
## Docker install
La liste des commandes pour démarrer docker (c'est à faire une fois)
```
sudo systemctl start docker.service; #start du service
sudo systemctl enable docker.service; #eanble auto-start
sudo usermod -aG docker admin;  #add to the user admin the group docker
```

### Install de HP-QC
> Il y a eu un problème sur la configuration de MYSQL pour HP QC (a réasasyer sur windows)

```
sudo ./ALM_installer.bin -f installer.properties
```
####  Install de MYSQL
```
https://devops.profitbricks.com/tutorials/install-mysql-on-centos-7/
sudo rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
sudo yum -y install mysql-community-server
sudo /usr/bin/systemctl enable mysqld
sudo /usr/bin/systemctl start mysqld
cd ~
sudo /usr/bin/mysql_secure_installation #password private

mysql -u root -p mysql #Login to mysql (mysql password)
jdbc:mysql://localhost:3306/dbname
```


#### Information générale:
Installation d'un dictionaire francais
```
yum install hunspell-fr.noarch
```

Note a faire pour plus tard:
Il y a l'idée de se faire une application permettant de prendre des notes de manières structure toute en fourissant une sorte de base de données.

En permettant un export CSV.

jupyter.
