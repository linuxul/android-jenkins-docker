FROM eclipse-temurin:11.0.17_8-jdk AS jre-build

FROM debian:bullseye-20220316

LABEL maintainer = "park97.sm@gmail.com"

ARG user=jenkins
ARG group=jenkins
ARG uid=1000
ARG gid=1000
ARG JENKINS_AGENT_HOME=/home/${user}

ENV JENKINS_AGENT_HOME ${JENKINS_AGENT_HOME}

RUN groupadd -g ${gid} ${group} \
    && useradd -d "${JENKINS_AGENT_HOME}" -u "${uid}" -g "${gid}" -m -s /bin/bash "${user}"

RUN apt-get update \
    && apt-get install --no-install-recommends -y openssh-server git-lfs\
    && rm -rf /var/lib/apt/lists/*

## SDK Manager, Android SDK 설치 경로 및 버전 이름 설정
ENV ANDROID_SDK_ZIP         commandlinetools-linux-7583922_latest.zip
ENV ANDROID_SDK_ZIP_URL     https://dl.google.com/android/repository/$ANDROID_SDK_ZIP
ENV ANDROID_HOME            /opt/android-sdk-linux
ENV ANDROID_SDK_ROOT        /opt/android-sdk-linux

## Gradle 설치 경로 및 버전 이름 설정
ENV GRADLE_ZIP      gradle-6.8-bin.zip
ENV GRADLE_ZIP_URL  https://services.gradle.org/distributions/$GRADLE_ZIP

## Android SDK 및 Gradle 환경 변수 설정
ENV PATH $PATH:$ANDROID_SDK_ROOT/platform-tools
ENV PATH $PATH:$ANDROID_SDK_ROOT/cmdline-tools/bin
ENV PATH $PATH:/opt/gradle-6.8/bin

## 필요한 패키지들 설치
RUN dpkg --add-architecture i386
RUN rm -rf /var/lib/apt/list/* && apt-get update && apt-get install ca-certificates curl gnupg2 software-properties-common git unzip file apt-utils lxc apt-transport-https libc6:i386 libncurses5:i386 libstdc++6:i386 zlib1g:i386 -y
RUN apt-get update \
  && apt-get -y install \
    git-lfs \
    fontconfig


## openJDK 11 설치
RUN apt-get update && apt-get install openjdk-11-jdk -y

ENV JAVA_HOME=/opt/java/openjdk
ENV PATH "${JAVA_HOME}/bin:${PATH}"
COPY --from=jre-build /opt/java/openjdk $JAVA_HOME

## Gradle 설치
ADD $GRADLE_ZIP_URL /opt/
RUN unzip /opt/$GRADLE_ZIP -d /opt/ && rm /opt/$GRADLE_ZIP

## SDK Manager 설치
ADD $ANDROID_SDK_ZIP_URL /opt/
RUN unzip -q /opt/$ANDROID_SDK_ZIP -d $ANDROID_SDK_ROOT && rm /opt/$ANDROID_SDK_ZIP

## Android SDK 설치
RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "platform-tools" "cmdline-tools;latest"

## Android SDK v31 설치
RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "platforms;android-31"
RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "build-tools;30.0.3"
RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "build-tools;30.0.2"

## Android SDK v30 설치
RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "platforms;android-30"
RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "build-tools;29.0.2"

## Android SDK v29 설치
RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "platforms;android-29"
RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "build-tools;28.0.3"

## Android SDK v28 설치
RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "platforms;android-28"
RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "build-tools;27.0.3"

## Android SDK v27 설치
RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "platforms;android-27"
RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "build-tools;26.0.3"

## Android SDK의 권한 jenkins로 변경
RUN chown -R jenkins $ANDROID_SDK_ROOT

RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# SSH Server 설정
RUN sed -i /etc/ssh/sshd_config \
        -e 's/#PermitRootLogin.*/PermitRootLogin no/' \
        -e 's/#RSAAuthentication.*/RSAAuthentication yes/'  \
        -e 's/#PasswordAuthentication.*/PasswordAuthentication no/' \
        -e 's/#SyslogFacility.*/SyslogFacility AUTH/' \
        -e 's/#LogLevel.*/LogLevel INFO/' && \
    mkdir /var/run/sshd

VOLUME "${JENKINS_AGENT_HOME}" "/tmp" "/run" "/var/run"
WORKDIR "${JENKINS_AGENT_HOME}"

ENV LANG C.UTF-8

RUN echo "PATH=${PATH}" >> /etc/environment
COPY setup-sshd /usr/local/bin/setup-sshd

EXPOSE 22

ENTRYPOINT ["setup-sshd"]