.PHONY:  keygen build volume deploy remove

keygen:
	@ssh-keygen -t ed25519 -b 4096 -C "jenkins@local.io" -f jenkins-master/config/ssh_keys/jenkins

volume:
	@docker volume create --name jenkins-data
	@docker volume create --name jenkins-ssl

ssl:
	@docker run --rm \
	-e COUNTRY_NAME=AT \
	-e STATE=Austria \
	-e CITY=Vienna \
	-e ORGANIZATION_NAME=local \
	-e COMMON_NAME=jenkins.local.io \
	-e CERT_NAME_OUTPUT=jenkins.local.io \
	-v jenkins-ssl:/ssl \
	dojomi/openssl
	
build:
	@docker build -t jenkins-master jenkins-master/.
	@docker build -t jenkins-nginx jenkins-nginx/.

deploy:
	@docker run -ti -p 8080:8080 -p 50000:50000 --name="jenkins.local.io" -v jenkins-data:/var/jenkins_home -d --restart=always jenkins-master
	@docker run -ti -p 443:443 --name="jenkins-nginx" -v jenkins-ssl:/etc/nginx/ssl --link jenkins.local.io:jenkins-master -d jenkins-nginx
	
remove:
	@docker rm -f jenkins.local.io jenkins-nginx
	@docker volume rm jenkins-data jenkins-ssl
	@rm jenkins-master/config/ssh_keys/*

