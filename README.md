### Overview of the Android-Bintray-Kit -- ABK

This vanilla kit is quickly help you get started on the following:

1. Create an Android AAR library.
2. Generate the build and upload the AAR format a Bintray repository.
3. Create a sample demo app that utilizes AAR ilbrary above.

Bintray allows you to keep track of your builds in your OWN maven repositories. At the same time you can push those builds to Jcenter or MavenCentral (two popular repositories). Using repositories is pretty useful for keeping code modularized and things manageable.

*Note: For more information about AARs, repositories and detailed tutorials, please visit this webpage <LINK>. This quick guide surmises the important steps to get things up and running.*

Requirements - Install Android Studio with SDK, and basic knowledge of building android apps.

###

- Explain how to use (SetupBintray, DownloadProject, ConfigureParams.)
- Link to website for detailed tutorial


TODO: Add mavencentral. a bit more on jcenter. setup repo to support snapshots.

### This template uploads in the new AAR format.

- /AndroidManifest.xml (mandatory)
- /classes.jar (mandatory)
- /res/ (mandatory)
- /R.txt (mandatory)
- /assets/ (optional)
- /libs/*.jar (optional)
- /jni/<abi>/*.so (optional)
- /proguard.txt (optional)
- /lint.jar (optional)


### Setup bintray 
Here's a good tutorial
... find those bookmarks

### Create "local.properties" file in the root project folder
* bintray.userid=[eg. user123] 
* bintray.apikey=[eg. 3ff1763398311f0d73ab7e42f3e46h3457132b42]
* bintray.gpgkey=[eg. password123]

### Bintray (either JCenter or MavenORG)
* The libraries will be pushed to bintray. You can specify this in the compile (see sample app).
* This repository does not support SNAPSHOT. To use that register for Sona in Bintray
 
```
 maven {
        url 'https://dl.bintray.com/jattcode/maven/'
    }
```

```
dependencies {
    compile 'com.jattcode.oss:android-library-template:0.1.0'
}
```

