FROM jenkins/jenkins

USER root

# Install Java
# Update package lists and install JDK
RUN apt-get update && \
    apt-get install -y openjdk-17-jdk

# Set Java environment variables
ENV JAVA_HOME /usr/lib/jvm/java-17-openjdk-amd64
ENV PATH $PATH:$JAVA_HOME/bin

# Install Maven
RUN apt-get install -y maven

# Set Maven environment variables
ENV MAVEN_HOME /usr/share/maven
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"



# Install Node.js and npm
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-get install -y nodejs

# Install Appium
RUN npm install -g appium@1.22.3

# Install Android SDK Command Line Tools
RUN apt-get install -y curl unzip
ARG ANDROID_SDK_VERSION=commandlinetools-linux-10406996_latest
ENV ANDROID_HOME /opt/android/sdk
ENV ANDROID_SDK_ROOT /opt/android/sdk
ENV PATH $PATH:$ANDROID_HOME/cmdline-tools/tools/bin:$ANDROID_HOME/emulator:$ANDROID_HOME/platform-tools
ARG ANDROID_SDK_VERSION=10406996
ENV ANDROID_SDK_ROOT /opt/android-sdk
RUN mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools && \
    curl -fsSLO https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_SDK_VERSION}_latest.zip && \
    unzip *tools*linux*.zip -d ${ANDROID_SDK_ROOT}/cmdline-tools && \
    mv ${ANDROID_SDK_ROOT}/cmdline-tools/cmdline-tools ${ANDROID_SDK_ROOT}/cmdline-tools/tools && \
    rm *tools*linux*.zip

# Install ADB
RUN apt-get install -y android-tools-adb

USER jenkins

RUN jenkins-plugin-cli --plugins \
    git \
    gradle \
    android-emulator \
    ws-cleanup \
    slack \
    embeddable-build-status \
    blueocean \
    github-coverage-reporter \
    jacoco \
    github-pr-coverage-status \
    locale \
    role-strategy \
    jdk-tool \
    google-login \
    warnings-ng \
    github-pr-comment-build \
    sonar \
    basic-branch-build-strategies \
    github-scm-trait-notification-context \
    list-git-branches-parameter \
    remote-file \
    gitlab-plugin
