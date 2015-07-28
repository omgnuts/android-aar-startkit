### Part 1: Overview of the Android-AAR-StartKit

This vanilla kit is quickly help you get started on the following:

1. Create an Android AAR library.
2. Generate the build and upload the AAR format to the MavenCentral repository (aka Sonatype).
3. Create a sample demo app that utilizes AAR library above.

NOTE: After using bintray and sonatype for a while, I now prefer sonatype because of the ease of handling snapshots during a the development cycle. So, you can use the starter kit for developing an AAR, and then publish to MavenCentral (aka Sonatype). Using repositories is pretty useful for keeping code modularized and things manageable. 

This will upload your AAR to repositories that look like these:

```
// MavenCentral (aka Sonatype) for release builds.
mavenCentral() 
// MavenCentral (aka Sonatype) for snapshot builds.
maven { url "https://oss.sonatype.org/content/repositories/snapshots" }
```

and you can access the library components via a call like this in your build.

```
compile 'com.github.jimcoven:aar-startkit:0.61'
```
The instructions below will be kept short, to avoid over-verbosity. Let me know if I missed out, and I'll try to update it. Thanks!

### Part 2: HOW TO USE THIS KIT

Requirements - Install Android Studio with SDK, and basic knowledge of building android apps.

##### A. Setup a Sonatype account

- Register for your account on https://www.sonatype.org. Once approved, login

- Create a ‘New Project’ JIRA issue on your dashboard. 
![alt text][create]

- For Project, select ‘Community Support – Open Source Project Repository Hosting‘. Fill in the required fields.
![alt text][submit]

- After you submit the request, you have to wait for a few days for that to be approved.
![alt text][notice]

[create]: https://raw.githubusercontent.com/jimcoven/android-aar-startkit/master/art/55-sona-create.jpg "Create Jira Issue"
[submit]: https://raw.githubusercontent.com/jimcoven/android-aar-startkit/master/art/56-sona-submit.jpg "Submit Jira Issue"
[notice]: https://raw.githubusercontent.com/jimcoven/android-aar-startkit/master/art/57-sona-created.jpg "Receive notification"

##### B. Download the project files

Download the source codes kit.

```
git clone https://github.com/jimcoven/android-aar-startkit
```

##### C. Create/Update gradle.properties

- Open the file 'gradle.properties' in your project root and update the information there

```
sonatype.userid=USERNAME
sonatype.passwd=PASSWORD
```

##### D. Configure the project

So lets say you want to create an AAR that people can reference as:

```
compile 'com.playground:my-awesome-widget:1.0'
```

In this part of the universe, we can express the above as 

```
    compile 'group_id : artifact_id : artifact_version '
```

1. Open 'mod_library/build.gradle'. I have sectioned out the parameters as follows. 

- Authentication (Why use gradle.properties instead of leaving them in here? The .gitignore ignores the upload of your auth parameters, in case you leave them inside here and check them in. But the choice is yours) 

- Library info (I have indicated the important ones with '-->'). The rest should be clear.
```
--> package_name = 'some-package-name-that-you-want' 
--> group_id = 'com.playground' 
    artifact_id = 'my-awesome-widget'
--> lib_version = '1.0'
    lib_version_code = 1
    git_url = 'https://github.com/jimcoven/android-aar-startkit.git'
    site_url = 'https://github.com/jimcoven/android-aar-startkit'
    all_licenses = ["Apache-2.0"] // mandatory
    
    pom_library_desc = 'Basic android library template for deploying AARs'
    pom_license_name = 'The Apache Software License, Version 2.0'
    pom_license_url = 'http://www.apache.org/licenses/LICENSE-2.0.txt'
--> pom_developer_id = 'jimcoven' 
    pom_developer_name = 'Jim Coven'
    pom_developer_email = ''
```

3. Go to a command-line / terminal and run to test it out with the command below. 
``` 
gradlew uploadArchives
```
You should see the logs showing that you have uploaded the AAR to maven. Go to the following urls and search for your group ids. Sometimes the network is slow and you have to wait a few minutes for it to refresh. Here is my snapshot url, just replace the last part with your group id 

```
// for snapshots
https://oss.sonatype.org/content/repositories/snapshots/com/github/jimcoven 
```

4. Now lets test your sample app. Go to 'mod_sample/build.gradle'. 

- Make sure you have the repos

```
repositories {
    mavenCentral()
    maven { url "https://oss.sonatype.org/content/repositories/snapshots" }
}
```

- Change the dependencies to

```
compile 'com.playground:my-awesome-widget:1.0'
```

- Note that the sample app does not reference mod_library locally. So it actually pulls your library off your remote repository  and uses that to build your application. From the project root directory, type the following to build the app

```
gradlew build
```

### That's all folks! Hope it helps. Let me know if there are mistakes to be corrected. 


Summary of commands 

```
// To deploy release  / snapshot
// For snapshot: remember to change the version to include the SNAPSHOT eg. 0.6-SNAPSHOT
// Also remember to disable the build of the app because you only want to deploy the library
gradlew uploadArchives

// To build locally for development
gradlew build 

// or 
gradlew clean build
```

### NOTE: Quick steps for obtaining GPG Signing keys

Step 1. You need cygwin or terminal access to do this. Open a terminal and type:

```
gpg --gen-key
```

Step 2. Just press enter when you are asked to choose. The recommendations are good enough for general purpose use. You should see the following output after you have confirmed your name, email. You will also be asked to set a password - this is your **GPG password**.

```
pub   2048R/ABCD1234 2015-07-05
uid                  Jim Coven <jattcode@gmail.com>
sub   2048R/12345678 2015-03-07
```

Step 3. Submit your key to a public like below. You can just submit it, you don't have to open an account or anything. Send the command, where **ABCD1234** are your alphanumeric **KeyId** from Step 2 above. (see the first line that starts with 'pub.. 2048R/ABCD1234 2015-07-05"

```
gpg --keyserver hkp://pool.sks-keyservers.net --send-keys ABCD1234
```

Step 4. Check your secret keys. Type the command below and get the **location** of your key file

```
gpg --list-secret-keys <-- type this
/home/jimcoven/.gnupg/secring.gpg <-- appears on your terminal
```

Add these to your gradle.properties file.

```
signing.keyId=KeyId
signing.password=GPG password
signing.secretKeyRingFile=location
```

Step 5. Export your keys 

```
gpg -a --export jattcode@gmail.com > public-key.asc
gpg -a --export-secret-key jattcode@gmail.com > private-key.asc
```

