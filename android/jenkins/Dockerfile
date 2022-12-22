# jenkins LTS Changelog: https://www.jenkins.io/changelog-stable/
# Jenkins Dockerfile : https://github.com/jenkinsci/docker/blob/master/11/debian/bullseye/hotspot/Dockerfile
FROM jenkins/jenkins:lts-jdk11

LABEL maintinaer="xxx@xxx.com"

ENV SSH_C Lz1Jenkins@example.com

# Command-line tools :https://developer.android.com/studio/command-line
ENV ANDROID_SDK_ZIP commandlinetools-linux-9123335_latest.zip
ENV ANDROID_SDK_ZIP_URL https://dl.google.com/android/repository/$ANDROID_SDK_ZIP
ENV ANDROID_HOME /opt/android-sdk-linux
ENV ANDROID_SDK_ROOT /opt/android-sdk-linux

ENV GRADLE_ZIP gradle-7.0.2-bin.zip
ENV GRADLE_ZIP_URL https://services.gradle.org/distributions/$GRADLE_ZIP

ENV PATH $PATH:$ANDROID_SDK_ROOT/tools/bin
ENV PATH $PATH:$ANDROID_SDK_ROOT/platform-tools
ENV PATH $PATH:/opt/gradle-7.0.2/bin

USER root

RUN dpkg --add-architecture i386
RUN apt-get update && apt-get install -y \
    # software-properties-common provide utilities for managing software repositories and packages on Debian and Ubuntu systems
    # This package is often necessary when installing software from third-party repositories or when building software from source.
    software-properties-common \
    # build-essential is a package that contains the essential tools required for building software on Debian and Ubuntu systems. These tools include the C compiler (gcc), C++ compiler (g++), linker (ld) and other commonly used compilation tools.
    build-essential \
    # Install 32-bit compatibility for 64-bit environments
    libc6:i386 \
    libstdc++6:i386 || true

# Install gradle
ADD $GRADLE_ZIP_URL /opt/
RUN unzip /opt/$GRADLE_ZIP -d /opt/ && \
	rm /opt/$GRADLE_ZIP


# Install Android SDK
ADD $ANDROID_SDK_ZIP_URL /opt/
RUN unzip -q /opt/$ANDROID_SDK_ZIP -d ${ANDROID_SDK_ROOT} && \
	rm /opt/$ANDROID_SDK_ZIP

ENV PATH ${PATH}:${ANDROID_HOME}/cmdline-tools/bin

RUN sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "platform-tools" "build-tools;30.0.1" "platforms;android-30" "build-tools;21.1.2" "platforms;android-21" "ndk-bundle"

RUN chown -R jenkins ${ANDROID_SDK_ROOT}

# Cleanup
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

USER jenkins
