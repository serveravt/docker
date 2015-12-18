FROM debian

MAINTAINER Artur Trofymchuk "artur@trofymchuk.net"

# Install Deps
RUN apt-get --quiet update --yes
RUN apt-get --quiet install --yes wget tar git unzip openjdk-7-jdk lib32stdc++6 lib32z1
# Install Android SDK
RUN cd /opt && wget --output-document=android-sdk.tgz --quiet http://dl.google.com/android/android-sdk_r24.4.1-linux.tgz && tar xzf android-sdk.tgz && rm -f android-sdk.tgz && chown -R root.root android-sdk-linux

# Setup environment
ENV ANDROID_HOME /opt/android-sdk-linux
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools

# Install sdk elements
ENV PATH ${PATH}:/opt/tools
RUN echo y | android --silent update sdk --no-ui --all --filter platform-tools,tools,extra,build-tools-23.0.2,android-22,android-8

#install gradle
RUN cd /opt && wget --quiet --output-document=gradle.zip https://services.gradle.org/distributions/gradle-2.9-bin.zip && unzip -q gradle.zip && rm -f gradle.zip && chown -R root.root /opt/gradle-2.9/bin
ENV PATH ${PATH}:/opt/gradle-2.9/bin

#install ssh-agent
RUN 'which ssh-agent || ( apt-get update -y && apt-get install openssh-client -y )'

RUN which java
RUN which android
RUN which git
RUN which gradle
#RUN which adb


# Cleaning
RUN apt-get clean

# GO to workspace
RUN mkdir -p /opt/workspace
WORKDIR /opt/workspace