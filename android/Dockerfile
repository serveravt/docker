FROM debian

MAINTAINER Artur Trofymchuk "artur@trofymchuk.net"

# Install Deps
RUN apt-get --quiet update --yes

RUN echo deb http://http.debian.net/debian jessie-backports main >> /etc/apt/sources.list

RUN apt-get update && apt-get --quiet install --yes openjdk-8-jdk

RUN update-alternatives --config java

RUN apt-get --quiet install --yes wget tar git unzip lib32stdc++6 lib32z1
# Install Android SDK
RUN cd /opt && wget --output-document=android-sdk.tgz --quiet http://dl.google.com/android/android-sdk_r24.4.1-linux.tgz && tar xzf android-sdk.tgz && rm -f android-sdk.tgz && chown -R root.root android-sdk-linux

# Setup environment
ENV ANDROID_HOME /opt/android-sdk-linux
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools

# Install sdk elements
ENV PATH ${PATH}:/opt/tools
RUN echo y | android --silent update sdk --no-ui --all --filter platform-tools
#RUN echo y | android --silent update sdk --no-ui --all --filter tools
RUN echo y | android --silent update sdk --no-ui --all --filter extra
RUN echo y | android --silent update sdk --no-ui --all --filter build-tools-23.0.2,build-tools-23.0.3
RUN echo y | android --silent update sdk --no-ui --all --filter android-22,android-23,android-8
RUN echo y | android --silent update sdk --no-ui --all --filter sys-img-armeabi-v7a-android-22

#install gradle
RUN cd /opt && wget --quiet --output-document=gradle.zip https://services.gradle.org/distributions/gradle-2.14.1-bin.zip && unzip -q gradle.zip && rm -f gradle.zip && chown -R root.root /opt/gradle-2.14.1/bin
ENV PATH ${PATH}:/opt/gradle-2.14.1/bin

# Set up and run emulator
RUN echo no | android create avd --force -n test -c 30M -t android-22
ENV HOME /root

ADD wait-for-emulator /usr/local/bin/
ADD start-emulator /usr/local/bin/

RUN which java
RUN which android
RUN which git
RUN which gradle
RUN which adb


# Cleaning
RUN apt-get clean

# GO to workspace
RUN mkdir -p /opt/workspace
VOLUME /root/.gradle
WORKDIR /opt/workspace
