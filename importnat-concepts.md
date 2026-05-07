# this is the raghu 
1. new feature added : for testing purpose deploy or not ok

login:
https://trial8oqwds.jfrog.io/ui/login

mail: raghukadali@zohomail.in
password: <stored in jenkins credentials>

https://trial8oqwds.jfrog.io/ui/get_started

repository: https://trial8oqwds.jfrog.io/artifactory/api/docker/private1-docker

docker login -u raghukadali@zohomail.in trial8oqwds.jfrog.io
password token: <stored in jenkins credentials - JFROG_CREDS>

trail: docker pull trial8oqwds.jfrog.io/private1-docker/hello-world:latest
tag the image: docker tag trial8oqwds.jfrog.io/private1-docker/hello-world trial8oqwds.jfrog.io/private1-docker/hello-world:1.0.0
push: docker push trial8oqwds.jfrog.io/private1-docker/hello-world:1.0.0

secretes:
kubectl create secret docker-registry jfrog-secret \
    --docker-server=trial8oqwds.jfrog.io \
    --docker-username=raghukadali@zohomail.in \
    --docker-password=<use token from jenkins credentials>