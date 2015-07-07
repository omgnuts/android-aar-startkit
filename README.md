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

##### D. Update the project with your keys

- Open the file 'bintray.auth_example' and update the information there

```
bintray.userid=USERNAME
bintray.apikey=3ff1d537280570753817e42f3e45693816592b41
bintray.gpgkey=PASSWORD
```

- Rename the file to 'bintray.auth'
- Hooray, now your project is ilnked to Bintray with valid authentication setup.

##### E. Configure the project

So lets say you want to create an AAR that people can reference as:

```
compile 'com.playground:my-awesome-widget:1.0'
```

In this part of the universe, we can express the above as 

```
    compile 'group_id : artifact_id : artifact_version '
```

1. Open 'bintray-aar-settings.gradle' and update the project artifact_id to 'my-awesome-widget' but it still logically points to mod_library in the project. You can change this anytime, but do bear it mind it will affect those who have already used your library in their projects. It should look like this

```
    include ':my-awesome-widget'
    project(':my-awesome-widget').projectDir = new File('mod_library')
```

2. Open 'mod_library/build.gradle'. I have sectioned out the parameters as follows. 

- Authentication (Why use bintray.auth instead of leaving them in here? The .gitignore ignores the upload of your auth parameters, in case you leave them inside here and check them in.)
```
    bintray_userid = properties.getProperty("bintray.userid")
    bintray_apikey = properties.getProperty("bintray.apikey")
    bintray_gpgkey = properties.getProperty("bintray.gpgkey")
```

- Library info (I have indicated the important ones with '-->'). The rest should be clear.
```
    bintray_repo = 'maven'
--> package_name = 'some-package-name-that-you-want' 
--> group_id = 'com.playground' 
    artifact_id = project.name // we already set this above
--> artifact_version = '1.0'
    all_licenses = ["Apache-2.0"] // mandatory
    git_url = 'https://github.com/jimcoven/android-bintray-kit.git'
    site_url = 'https://github.com/jimcoven/android-bintray-kit'
    
    pom_library_desc = 'Basic android library template for deploying AARs'
    pom_license_name = 'The Apache Software License, Version 2.0'
    pom_license_url = 'http://www.apache.org/licenses/LICENSE-2.0.txt'
--> pom_developer_id = 'jimcoven' // does not have to be your bintray user_id
    pom_developer_name = 'Jim Coven'
    pom_developer_email = 'jattcode@gmail.com'
```

3. Go to a command-ilne / terminal and run to test it out. If you are on windows, you can run 'bintray-publish.bat'. You should see the logs showing that you have uploaded the AAR to bintray. Go to bintray -> Maven repo, and look for your package. Alternatively, go to https://dl.bintray.com/your_username/maven and you should see things there in a minute or so. 

``` 
gradlew bintrayUpload -c bintray-aar-settings.gradle -Ppublish_mode=release
```

4. Now lets test your sample app. Go to 'mod_sample/build.gradle'. 

- Change the repo to your own username

```
maven { url 'https://dl.bintray.com/your_username/maven/' }
```

- Change the dependencies to

```
compile 'com.playground:my-awesome-widget:1.0'
```

- Note that the sample app does not reference mod_library locally. So it actually pulls your library off your bintray repository and uses that to build your application. From the project root directory, type the following to build the app

```
gradlew build
```

### That's all folks! Hope it helps. Let me know if there are mistakes to be corrected. 


### NOTE I. Quick steps for obtaining GPG Signing keys

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

*For those who are familiar with the keystore method for signing android apps, this is a bit different, but it does a similar thing.*


### NOTE II. What are the important files/scripts in this project?

```
- bintray.auth : For authenticating with bintray
```

```
- bintray-aar-settings.gradle : for separating the release settings.gradle from the settings.gradle used in development
- bintray-publish.bat : for executing a release
- bintray-publish-debug.bat : for executing a dry-run
```

```
- mod_library/build.gradle : finalized parameter container + android AAR build using the android library plugin. 
- bintray-aar-publish.gradle : contains the upload and publishing script.
```

```
- mod_sample/build.gradle : defines the repo + dependency for your AAR library. 
```
