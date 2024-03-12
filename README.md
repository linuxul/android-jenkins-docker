# android-jenkins-docker
Android CI를 위한 Jenkins DockerFile입니다.
<br><br>
## Specs
- **Master:** `Jenkins v2.361.2` `OpenJDK 11` `Gradle v6.8` `Android SDK 27~31`
- **Agent:** `eclipse-temurin:11.0.17_8-jdk` `debian:bullseye-20220316` `OpenJDK 11` `Gradle v6.8` `Android SDK 27~31`
<br>

## How to use
1. Build the Dockerfile.
```
  $ sudo ./docker-image-build.sh
```
<br>

2. Edit the docker-compose.yml file.
> for Master
```
  $ sudo vi ./docker-compose.yml
    ...
      volumes:
        - {enter your host volume}:/var/jenkins_home
        - /var/run/docker.sock:/var/run/docker.sock
      environment:
        - TZ=Asia/Seoul
    ...
```
> for Agent
```
  $ sudo vi ./docker-compose.yml
    ...
      volumes:
        - {Enter your host volume}:/var/jenkins_home
      environment:
        - TZ=Asia/Seoul
        - JENKINS_SLAVE_SSH_PUBKEY={Enter your SSH publicKey in master jenkins docker container}
    ...
```
<br>

3. Start the generated docker file.
```
  $ sudo ./docker-image-start.sh
```

