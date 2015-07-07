### Part 1: Overview of the Android-Bintray-Kit -- ABK

This vanilla kit is quickly help you get started on the following:

1. Create an Android AAR library.
2. Generate the build and upload the AAR format a Bintray repository.
3. Create a sample demo app that utilizes AAR ilbrary above.

This will upload your AAR to a repository that looks like 

```
maven { url 'https://dl.bintray.com/jimcoven/maven/' }
```

and you can access the library components via a call like this in your build.

```
compile 'com.jattcode:android-bintray-kit:0.5'
```

Bintray allows you to keep track of your builds in your OWN maven repositories. At the same time you can push those builds to Jcenter or MavenCentral (two popular repositories). Using repositories is pretty useful for keeping code modularized and things manageable. 

*Note: For more information about AARs, repositories and detailed tutorials, please visit this webpage <LINK>. This quick guide surmises the important steps to get things up and running.*

### Part 2: HOW TO USE THIS KIT

Requirements - Install Android Studio with SDK, and basic knowledge of building android apps.

##### A. Setup a Bintray account

- Go to https://bintray.com and create 
- Setup your GPG keys (Theres a quick tutorial on this at the bottom of the page


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

##### NOTE I. GPG Signing keys

Step 1. You need cygwin or terminal access to do this. Open a terminal and type: 

```
gpg --gen-key
```

Step 2. Just press enter when you are asked to choose. The recommendations are good enough for general purpose use. You should see the following output after you have confirmed your name, email.

```
pub   2048R/ABCD1234 2015-07-05
uid                  Jim Coven <jattcode@gmail.com>
sub   2048R/12345678 2015-03-07
```

Step 3. Submit your key to a public like below. You can just submit it, you don't have to open an account or anything. Send the command, where "ABCD1234" are the eight alphanumeric chars from Step 2 above. (see the first line that starts with 'pub.. 2048R/ABCD1234 2015-07-05"

```
gpg --keyserver hkp://pool.sks-keyservers.net --send-keys ABCD1234
```

Step 4. Export your keys

```
gpg -a --export jattcode@gmail.com > public-key.asc
gpg -a --export-secret-key jattcode@gmail.com > private-key.asc
```

For those who are familiar with the keystore method for signing android apps, this is a bit different, but it does a similar thing. 
