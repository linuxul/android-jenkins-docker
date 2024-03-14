FROM jenkins/jenkins:lts

LABEL maintainer="jsseo@eflash.co.kr"

# set default build arguments
ARG SDK_VERSION=commandlinetools-linux-9477386_latest.zip
ARG ANDROID_BUILD_VERSION=34
ARG ANDROID_TOOLS_VERSION=34.0.0
ARG NDK_VERSION=26.1.10909125
ARG NODE_VERSION=18
ARG WATCHMAN_VERSION=4.9.0
ARG CMAKE_VERSION=3.22.1

# set default environment variables
ENV ADB_INSTALL_TIMEOUT=10
ENV ANDROID_HOME=/opt/android
ENV ANDROID_SDK_ROOT=${ANDROID_HOME}
ENV ANDROID_NDK_HOME=${ANDROID_HOME}/ndk/$NDK_VERSION
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
ENV CMAKE_BIN_PATH=${ANDROID_HOME}/cmake/$CMAKE_VERSION/bin
ENV PATH=${CMAKE_BIN_PATH}:${ANDROID_HOME}/cmdline-tools/latest/bin:${ANDROID_HOME}/emulator:${ANDROID_HOME}/platform-tools:${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${PATH}

USER root

# Install system dependencies
RUN apt-get update -qq && apt-get install -qq -y --no-install-recommends \
        apt-transport-https \
        curl \
        file \
        gcc \
        git \
        g++ \
        gnupg2 \
        libc++-dev \
        libgl1 \
        libtcmalloc-minimal4 \
        make \
        openjdk-17-jdk-headless \
        openssh-client \
        patch \
        python3 \
        python3-distutils \
        rsync \
        ruby \
        ruby-dev \
        tzdata \
        unzip \
        sudo \
        ninja-build \
        zip \
        libicu-dev \
        jq \
        shellcheck \
    && gem install bundler \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*;

# install nodejs using n
RUN curl -L https://raw.githubusercontent.com/tj/n/master/bin/n -o n \
    && bash n $NODE_VERSION \
    && rm n \
    && npm install -g n \
    && npm install -g yarn

# download and unpack android
RUN curl -sS https://dl.google.com/android/repository/${SDK_VERSION} -o /tmp/sdk.zip \
    && mkdir -p ${ANDROID_HOME}/cmdline-tools \
    && unzip -q -d ${ANDROID_HOME}/cmdline-tools /tmp/sdk.zip \
    && mv ${ANDROID_HOME}/cmdline-tools/cmdline-tools ${ANDROID_HOME}/cmdline-tools/latest \
    && rm /tmp/sdk.zip \
    && yes | sdkmanager --licenses \
    && yes | sdkmanager "platform-tools" \
        "platforms;android-$ANDROID_BUILD_VERSION" \
        "build-tools;$ANDROID_TOOLS_VERSION" \
        "cmake;$CMAKE_VERSION" \
        "ndk;$NDK_VERSION" \
    && chmod 777 -R /opt/android

## Android SDK의 권한 jenkins로 변경
RUN chown -R jenkins:jenkins $ANDROID_SDK_ROOT

## 기본 Jenkins Plugin 설치
USER jenkins
RUN jenkins-plugin-cli --plugins git gradle android-emulator ws-cleanup slack embeddable-build-status blueocean github-coverage-reporter jacoco github-pr-coverage-status locale role-strategy jdk-tool google-login warnings-ng github-pr-comment-build sonar basic-branch-build-strategies github-scm-trait-notification-context list-git-branches-parameter remote-file
