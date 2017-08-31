.PHONY:  keygen build volume deploy remove

keygen:
	ssh-keygen -t ed25519 -b 4096 -C "jenkins@local.io" -f jenkins-master/config/ssh_keys/jenkins

volume:
	docker volume create --name jenkins-data

build:
	docker build -t jenkins-master jenkins-master/.
	docker build --build-arg COMMON_NAME=jenkins.local.io -t jenkins-nginx jenkins-nginx/.
	
deploy:
	docker run -ti -p 8080:8080 -p 50000:50000 --name="jenkins.local.io" -v jenkins-data:/var/jenkins_home -d --restart=always jenkins-master
	docker run -ti -p 443:443 --name="jenkins-nginx" --link jenkins.local.io:jenkins-master -d jenkins-nginx
	
remove:
	docker rm -f jenkins.local.io jenkins-nginx
	docker volume rm jenkins-data
	rm jenkins-master/config/ssh_keys/*

