#### set fqdn /etc/hosts

```
127.0.0.1       jenkins.local.io        localhost
```

#### set build arguments to create the ssl-certificate

```
ARG CERTIFICATE_DIR=
ARG COUNTRY_NAME=
ARG STATE=
ARG CITY=
ARG ORGANIZATION_NAME= 
ARG COMMON_NAME=
ARG CERT_NAME_OUTPUT=

# change it inside the Makefile as followed

```
docker build --build-arg COMMON_NAME=jenkins.local.io -t jenkins-nginx jenkins-nginx/.
```

#### create the container

```
make keygen # to fetch the repository later on via ssh-url
make volume # creates the volume under /var/lib/docker/volumes/jenkins-data
make build  # build the containers
make deploy # per default a Test-Build job is configured
make remove # remove all created container and data-volume and ssh-key
```
