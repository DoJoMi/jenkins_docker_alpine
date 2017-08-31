#### set fqdn /etc/hosts
```
127.0.0.1       jenkins.local.io        localhost

```

#### create the container

```
make keygen # to fetch the repository later on via ssh-url
make volume # creates the volume under /var/lib/docker/volumes/jenkins-data, /var/lib/docker/volumes/jenkins-ssl
make ssl    # creates the ssl-certifacte and save it under /var/lib/docker/volumes/jenkins-ssl
make build  # build the containers
make deploy # per default a Test-Build job is configured
make remove # remove all created container and data-volume and ssh-key
```


