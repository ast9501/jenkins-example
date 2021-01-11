# Jenkins-demo

Jenkins token: 6d6f51cce565a450952a2798a7d9e34fc26e9f1

## Running Jenkins in Docker
for running Jenkins to build docker images, the Jenkins container should:
1. running in privilleged mode
2. docker installed inside container

```=cmd
# running jenkins
sudo docker run --name jenkins -d --restart always -p 8080:8080 -p 50000:50000 -v /var/lib/Jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock --privileged alan0415/jenkins-docker:v1
```
### Reference Link
[runnning Jenkins in docker](https://ithelp.ithome.com.tw/articles/10200621)
[Dockerfile recipe for building Jenkins images with docker inside](https://www.edureka.co/community/55640/jenkins-docker-docker-image-jenkins-pipeline-docker-registry)

## Enable Jenkins-docker plugin connect to docker server
1. modify files as follow
```
# /etc/default/docker
DOCKER_OPTS="-H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock"
```
2. modufy files a below:
```
# vim /etc/default/docker
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2375
```
```
# vim /lib/systemd/system/docker.service
ExecStart=/usr/bin/dockerd -H fd://                <--- before
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2375    <--- After
```
3. reload daemon and restart docker service
```=cmd
systemctl daemon-reload
systemctl restart docker
```
### Reference Link
[step 1](https://stackoverflow.com/questions/47709208/how-to-find-docker-host-uri-to-be-used-in-jenkins-docker-plugin)
[step 2](https://github.com/jenkinsci/docker-plugin/issues/637)

## Build docker image
