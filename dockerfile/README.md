# Dockerfile
## Description
Building Jenkins Docker images using Jenkins version 2.204.5, for avoiding "CSRF Protection error".
While using Gitea web hook, enable CSRF Protection will occur error; 2.204.5 is the latest version can disable these feature to avoid error.
You can disbale the feature in global security>CSRF Protection
![](https://i.imgur.com/JdkOWtF.png)

