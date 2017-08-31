#### create the container

```
make keygen # to fetch the repository later on via ssh-url
make volume # create the volume under /var/lib/docker/volumes/jenkins-data
make build  # build the containers
make deploy # per default a Test-Build job is configured
make remove # remove all created container and data-volume and ssh-key
```
