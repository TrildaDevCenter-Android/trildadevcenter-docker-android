# This Dockerfile creates a static build image for CI

FROM ubuntu:23.04

RUN echo "apt-get update..."


RUN apt-get update \
    && apt-get install -y --no-install-recommends ca-certificates gnupg2
  #  && rm -rf /var/lib/apt/lists/*


RUN echo "GPG Key set up for SAP machine JDK 17"

RUN export GNUPGHOME="$(mktemp -d)" \
    && gpg --batch --keyserver hkps://keys.openpgp.org --recv-keys CACB9FE09150307D1D22D82962754C3B3ABCFE23 \
    && gpg --batch --export --armor 'CACB 9FE0 9150 307D 1D22 D829 6275 4C3B 3ABC FE23' > /etc/apt/trusted.gpg.d/sapmachine.gpg.asc \
    && gpgconf --kill all && rm -rf "$GNUPGHOME" \
    && echo "deb http://dist.sapmachine.io/debian/$(dpkg --print-architecture)/ ./" > /etc/apt/sources.list.d/sapmachine.list \
    && apt-get update \
    && apt-get -y --no-install-recommends install sapmachine-17-jdk=17.0.7

ENV JAVA_HOME=/usr/lib/jvm/sapmachine-17

ENV LC_ALL=en_US.UTF-8
ENV LANG=en_US.UTF-8

CMD ["jshell"]

RUN echo ${PWD}

# Just matched `app/build.gradle`
# ANDROID_COMPILE_SDK is the version of Android you're compiling with.
# It should match compileSdkVersion.
ENV ANDROID_COMPILE_SDK: "33"

# Just matched `app/build.gradle`
# ANDROID_BUILD_TOOLS is the version of the Android build tools you are using.
# It should match buildToolsVersion.
ENV ANDROID_BUILD_TOOLS "33.0.2"

# It's what version of the command line tools we're going to download from the official site.
# Official Site-> https://developer.android.com/studio/index.html
# There, look down below at the cli tools only, sdk tools package is of format:
#        commandlinetools-os_type-ANDROID_SDK_TOOLS_latest.zip
# when the script was last modified for latest compileSdkVersion, it was which is written down below
ENV ANDROID_SDK_TOOLS "9477386"

ENV ANDROID_HOME /android-home


# install OS packages

RUN echo "tools  installing..."

#RUN apt-get --quiet update -y

RUN echo "wget installing..."
RUN apt-get install -y wget

RUN echo "apt-utils installing..."
RUN apt-get --quiet install -y apt-utils

RUN echo "tar installing..."
RUN apt-get --quiet install -y tar

RUN echo "unzip installing..."
RUN apt-get --quiet install -y unzip

#RUN echo "lib32stdc++6 installing..."
#RUN apt-get --quiet install -y lib32stdc++6

#RUN echo "lib32z1 installing..."
#RUN apt-get --quiet install -y lib32z1

RUN echo "build essential installing..."
RUN apt-get --quiet install -y build-essential


#RUN apt-get --quiet install -y wget apt-utils tar unzip lib32stdc++6 lib32z1 build-essential ruby ruby-dev

RUN echo "ruby installing..."

RUN apt-get --quiet install -y ruby ruby-dev

RUN echo "gem budler installing..."

RUN gem install bundler

RUN echo "gem installfastlane  installing..."
RUN gem install fastlane




# We use this for xxd hex->binary
RUN apt-get --quiet install -y vim-common

# install Android SDK
RUN install -d $ANDROID_HOME
RUN cd ${ANDROID_HOME}
#RUN wget --quiet --output-document=android-sdk.tgz https://dl.google.com/android/repository/sdk-tools-linux/android-sdk_r${ANDROID_SDK_TOOLS}-linux.tgz
RUN wget --quiet --output-document=cmdline-tools.zip https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_SDK_TOOLS}_latest.zip
#RUN tar --extract --gzip --file=android-sdk.tgz
RUN unzip -d $ANDROID_HOME cmdline-tools.zip

RUN echo ${PWD}

RUN ls -alf
RUN ls -alf ${ANDROID_HOME}/cmdline-tools/bin/
RUN ls -alf ${ANDROID_HOME}/
ENV PATH=$PATH:${ANDROID_HOME}/cmdline-tools/bin/
ENV PATH="${PATH}:${ANDROID_HOME}/platform-tools/"

RUN echo ${PATH}
RUN echo ${PWD}

RUN echo "platforms;android-${ANDROID_COMPILE_SDK}"


RUN yes | sdkmanager --sdk_root=${ANDROID_HOME} --licenses || true


RUN sdkmanager --sdk_root=${ANDROID_HOME} --update
RUN sdkmanager --sdk_root=${ANDROID_HOME} "platforms;android-33"
RUN sdkmanager --sdk_root=${ANDROID_HOME} "platforms;android-32"
RUN sdkmanager --sdk_root=${ANDROID_HOME} "platforms;android-31"
RUN sdkmanager --sdk_root=${ANDROID_HOME} "platforms;android-30"
RUN sdkmanager --sdk_root=${ANDROID_HOME} "platforms;android-29"
RUN sdkmanager --sdk_root=${ANDROID_HOME} "platforms;android-28"
RUN sdkmanager --sdk_root=${ANDROID_HOME} "platforms;android-27"
RUN sdkmanager --sdk_root=${ANDROID_HOME} "platforms;android-26"
RUN sdkmanager --sdk_root=${ANDROID_HOME} "platforms;android-25"
RUN sdkmanager --sdk_root=${ANDROID_HOME} "platforms;android-24"
RUN sdkmanager --sdk_root=${ANDROID_HOME} "platform-tools"
RUN sdkmanager --sdk_root=${ANDROID_HOME} "build-tools;33.0.2"
RUN sdkmanager --sdk_root=${ANDROID_HOME} "extras;android;m2repository"
RUN sdkmanager --sdk_root=${ANDROID_HOME} "extras;google;m2repository"
RUN sdkmanager --sdk_root=${ANDROID_HOME} "extras;google;google_play_services"


RUN echo "end docker file"
RUN rm -rf /var/lib/apt/lists/*
