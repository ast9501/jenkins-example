# Jenkins-example

## Running Jenkins in Docker
for running Jenkins to build docker images, the Jenkins container should:
1. running in privilleged mode
2. docker installed inside container

To build Jenkins server images, reference to dockerfile directory.
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
![](https://i.imgur.com/SRNWwhu.png)

### Reference Link
[step 1](https://stackoverflow.com/questions/47709208/how-to-find-docker-host-uri-to-be-used-in-jenkins-docker-plugin)
[step 2](https://github.com/jenkinsci/docker-plugin/issues/637)

## Jenkins connect
### Gitea
To make Gitea connect to Jenkins server, you need to install Gitea plugin, and configure gitea server in Global configure>Gitea Server
![](https://i.imgur.com/ztU2MgB.png)

#### Pipeline
1. Generate API token at Jenkins server
at user>configuration:
![](https://i.imgur.com/poYFous.png)
or you can just define a token by yourself.
2. start a new project in Jenkins, using pipeline. the figure below shown some configuration.
![](https://i.imgur.com/7FbWXK5.png)
the token in figure can defined by yourself.
3. In gitea repo, fill in the target URL as Jenkins example: 
```
?????????????????? URL ????????????: JENKINS_URL/job/Golang1.13-testing/build?token=TOKEN_NAME ??? /buildWithParameters?token=TOKEN_NAME
??????????????? &cause=??????????????????????????????????????????????????????
```
the token must same as the token filled in Jenkins pipeline setting.
![](https://i.imgur.com/Pf41hOe.png)
4. after set up the configure in both side, you can test configuration in gitea configuration, webhook update.
![](https://i.imgur.com/IjPVzI8.png)

While pushing code to gitea server will trigger Jenkins pipeline, remenber to put Jenkinsfile in the root of repo, or you can define its location in Jenkins pipeline configuration.

## Related study
1. [Several example about Jenkinsfile pipeline script](https://oranwind.org/-devops-jenkins-yu-centos-ubuntu-an-zhuang-jiao-xue/)

## Roadmap
1. learn to build Jenkins server...success
2. Jenkins server to test code, build docker images...success
3. connect Jenkins server to github
3. Known how to use Jenkins agent(Kubernetes, docker)
4. Several defination to define workflow(pipeline, free-style), the syntex about Jenkinsfile.
5. Jenkins Job Builder(JJB), Global JJB  usage, to implement Infrasructure as Code.
