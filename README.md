# trildadevcenter-docker-android
docker source files to build a docker image that is going to be available through docker web

# docker hub
https://hub.docker.com/repository/docker/pplaquette/trildadevcenter-docker-android/general
https://hub.docker.com/r/pplaquette/trildadevcenter-docker-android

# Note from docker hub
this docker image is used for a Gitlab CI/CD to build android applications
(I guess it can be used with other CI/CD pipelines relying on using a docker image, but it is yet untested, if you do it , please let me know)

it currently uses
- Ubuntu 23.04
- JDK 17
- fastlane
- Android SDK 33
- google APIS
- M2 rest

It will be updated regularly to provide improved components.

Until then, the core components will eventually change, and, among them, the versions of JDK and Ubuntu.

I'm not sure yet whether I'll use the LTS version for both, and provide some kind of LTS version for this docker image as other components like ruby, ruby bundler and fastlane are regularly updated.

tags :

- latest
represents the latest API level released for the Android SDK, currently 33
it contains only the latest version of the Android SDK and the build tools for this API level.

- level-YY
corresponds to a specific level which is not the latest available

- levels-XX-YY  (note the plural)
represents a docker image containing everything required for level XX of the api level XX up to api level YY

- levels-24-33
contains the androidSDK and build tool versions for api levels 24 to 33. This image tag can therefore be used to generate an application that covers api levels 24 to 33, and can be used to publish alpha bat applications for testers as well as for users of the Google Play Store or other application stores.
