#### Different Versions

```
jenkins_docker_alpine_v1 - build conf file to volume
jenkins_docker_alpine_v2 - build conf and ssl to volume
jenkins_docker_alpine_v3 - build conf file to volume and ssl inside container
jenkins_docker_alpine_v4 - build conf file to volume and ssl inside container + configuration changes 
```

#### set fqdn /etc/hosts
```
127.0.0.1       jenkins.local.io        localhost

```

#### backup the container

```
docker rm -f jenkins.local.io
tar czf jenkins-data.tar /var/lib/docker/volumes/jenkins-data

```

#### recreate it on another machine

```
tar xzf jenkins-data.tar /var/lib/docker/volumes/jenkins-data
```

#### update the container

```
docker rm -f jenkins.local.io
docker build -t jenkins.local.io .
docker run -ti -p 8080:8080 -p 50000:50000 --name="jenkins.local.io" -v jenkins-data:/var/jenkins_home -d jenkins2
```

#### list all current jobs (per default no auth is enabled)

```
http://localhost:8080/jnlpJars/jenkins-cli.jar
java -jar jenkins-cli.jar -s http://localhost:8080 list-jobs
```

#### list installed plugins

```
java -jar jenkins-cli.jar -s http://localhost:8080 list-plugins | awk '{ print $1":"$5}'
```
#### change the default theme
- Manage Jenkins
  - Configure System
     - URL of thme CSS https://cdn.rawgit.com/afonsof/jenkins-material-theme/gh-pages/dist/material-indigo.css
