.PHONY:  keygen build volume deploy remove

keygen:
	@ssh-keygen -t ed25519 -b 4096 -C "jenkins@example.com" -f jenkins-master/config/ssh_keys/jenkins

volume:
	@docker volume create --name jenkins-data

build:
	@docker build -t jenkins-master jenkins-master/.
	@docker build -t jenkins-nginx jenkins-nginx/.

deploy: 
	@docker run -ti -p 8080:8080 -p 50000:50000 --name="jenkins-master" -v jenkins-data:/var/jenkins_home -d --restart=always jenkins-master
	@docker run -ti -p 80:80 --name="jenkins-nginx" --link jenkins-master:jenkins-master -d jenkins-nginx

remove:
	@docker rm -f jenkins-master jenkins-nginx
	@docker volume rm jenkins-data
	@rm jenkins-master/config/ssh_keys/*

