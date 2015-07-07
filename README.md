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

The instructions below will be kept short, to avoid over-verbosity. Let me know if I missed out, and I'll try to update it. Thanks!

*Note: For more information about AARs, repositories and detailed tutorials, please visit this webpage <LINK>. This quick guide surmises the important steps to get things up and running.*

### Part 2: HOW TO USE THIS KIT

Requirements - Install Android Studio with SDK, and basic knowledge of building android apps.

##### A. Setup a Bintray account

- Create a user account : Go to https://bintray.com 
- Setup GPG public/private key + GPG password : Under 'GPG signing' in your Bintray profile, update your PUBLIC & PRIVATE keys (See below for a quick tutorial on how to generate the signature keys + signature password)
- Get an API key : Obtain the 'API key' from your Bintray profile

##### B. Enable auto-signing in your Bintray-Maven repo

- Log-in and go to your dashboard (https://bintray.com/). 
- You should see 6 repositories personally owned by you!! (And its free! So nice of Jfrog :D).
- Click on the repo that says 'Maven'. This brings you to the repo dashboard.
- Click on 'Edit' for the repo, scroll to the bottom and enable 'GPG Sign uploaded files automatically'

##### C. Download the project files

Download the android-bintray-kit codes.

```
git clone https://github.com/jimcoven/android-bintray-kit
```

##### C. Configure the project


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

##### NOTE I. Quick steps for obtaining GPG Signing keys

Step 1. You need cygwin or terminal access to do this. Open a terminal and type: 

```
gpg --gen-key
```

Step 2. Just press enter when you are asked to choose. The recommendations are good enough for general purpose use. You should see the following output after you have confirmed your name, email. You will also be asked to set a password - this is your GPG password.

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

So, you should finally have a) GPG public key b) GPG private key c) GPG password. 

* For those who are familiar with the keystore method for signing android apps, this is a bit different, but it does a similar thing. *


