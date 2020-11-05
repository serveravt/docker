# docker build --rm --tag andbuild .
# docker run --rm -v "{PROJ_PATH}":/proj/ --workdir="/proj/" andbuild /bin/sh -c "./gradlew clean assembleDebug"
# docker run -it --rm -v {GRADLE_CACHE}:/root/.gradle/  -v "{PROJ_PATH}":/proj/ --workdir="/proj/" --entrypoint /bin/bash andbuild

FROM debian:stretch

MAINTAINER Artur Trofymchuk "artur@trofymchuk.net"

ENV VERSION_BUILD_TOOLS "29.0.3"
ENV VERSION_TARGET_SDK "29"

# Install Deps
RUN apt-get --quiet update --yes
RUN apt-get --quiet install --yes apt-utils wget tar git unzip lib32stdc++6 lib32z1 rsync nano openjdk-8-jdk

# Install Android SDK
RUN cd /opt && \
    wget --output-document=android-sdk.zip --quiet https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip && \
    unzip android-sdk.zip -d android-sdk-linux && \
    rm -f android-sdk.zip && chown -R root.root android-sdk-linux

# Setup environment
ENV ANDROID_HOME /opt/android-sdk-linux
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools
RUN mkdir -p $ANDROID_HOME/licenses/ \
  && echo "8933bad161af4178b1185d1a37fbf41ea5269c55" > $ANDROID_HOME/licenses/android-sdk-license \
  && echo "84831b9409646a918e30573bab4c9c91346d8abd" > $ANDROID_HOME/licenses/android-sdk-preview-license

RUN mkdir /root/.android
RUN touch /root/.android/repositories.cfg

# Install sdk elements
RUN (while [ 1 ]; do sleep 5; echo y; done) | ${ANDROID_HOME}/tools/bin/sdkmanager \
  "platform-tools" \
  "platforms;android-${VERSION_TARGET_SDK}" \
  "platforms;android-28" \
  "platforms;android-27" \
  "platforms;android-26" \
  "build-tools;${VERSION_BUILD_TOOLS}" \
  "extras;google;m2repository" "extras;google;google_play_services" "patcher;v4" --verbose

#install gradle
RUN cd /opt \
	&& wget --quiet --output-document=gradle.zip https://services.gradle.org/distributions/gradle-6.5-all.zip \
	&& unzip -q gradle.zip && rm -f gradle.zip && chown -R root.root /opt/gradle-6.5/bin
ENV PATH ${PATH}:/opt/gradle-6.5/bin
ENV HOME /root

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
COPY .bashrc /root/.bashrc
