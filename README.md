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
compile 'com.jattcode:android-aar-startkit:0.6'
```
The instructions below will be kept short, to avoid over-verbosity. Let me know if I missed out, and I'll try to update it. Thanks!

### Part 2: HOW TO USE THIS KIT

Requirements - Install Android Studio with SDK, and basic knowledge of building android apps.

##### A. Setup a Sonatype account

- Register for your account on https://www.sonatype.org. Once approved, login
- Create a ‘New Project’ JIRA issue on your dashboard. For Project, select ‘Community Support – Open Source Project Repository Hosting‘. Fill in the required fields.
- After you submit the request, you have to wait for a few days for that to be approved.

##### B. Download the project files

Download the source codes kit.

```
git clone https://github.com/jimcoven/android-aar-startkit
```

##### C. Create/Update local.properties

- Open the file 'local.properties' in your project root and update the information there

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

- Authentication (Why use local.properties instead of leaving them in here? The .gitignore ignores the upload of your auth parameters, in case you leave them inside here and check them in. But the choice is yours) 
```
    sonatype_userid = properties.getProperty("sonatype.userid")
    sonatype_passwd = properties.getProperty("sonatype.passwd")
```

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
You should see the logs showing that you have uploaded the AAR to maven. Go to the following urls and search for your group ids. Sometimes the network is slow and you have to wait a few minutes for it to refresh. Here are my urls, just replace the last parts with your group id 

```
// for release builds
https://oss.sonatype.org/service/local/staging/deploy/maven2/com/jattcode/
// for snapshots
https://oss.sonatype.org/content/repositories/snapshots/com/jattcode/ 
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

