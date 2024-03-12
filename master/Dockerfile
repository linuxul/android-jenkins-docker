FROM jenkins/jenkins:lts

LABEL maintainer = "jsseo@eflash.co.kr"

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

USER root

## openJDK 11 설치
# RUN apt-get update && apt-get install openjdk-8-jdk -y
# RUN apt-get update && \
#     apt-get install -y software-properties-common && \
#     add-apt-repository "deb http://security.debian.org/debian-security stretch/updates main" && \
#     apt-get update && \
#     apt-get install -y openjdk-8-jdk

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
# RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "build-tools;30.0.3"
# RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "build-tools;30.0.2"

## Android SDK v30 설치
RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "platforms;android-30"
# RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "build-tools;29.0.2"

## Android SDK v29 설치
RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "platforms;android-29"
# RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "build-tools;28.0.3"

## Android SDK v28 설치
RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "platforms;android-28"
# RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "build-tools;27.0.3"

## Android SDK v27 설치
RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "platforms;android-27"
# RUN echo yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "build-tools;26.0.3"

## Android SDK의 권한 jenkins로 변경
RUN chown -R jenkins $ANDROID_SDK_ROOT

RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

## 기본 Jenkins Plugin 설치
#USER jenkins
#RUN jenkins-plugin-cli --plugins git gradle android-emulator ws-cleanup slack embeddable-build-status blueocean github-coverage-reporter jacoco github-pr-coverage-status locale role-strategy jdk-tool google-login warnings-ng github-pr-comment-build sonar basic-branch-build-strategies github-scm-trait-notification-context list-git-branches-parameter remote-file