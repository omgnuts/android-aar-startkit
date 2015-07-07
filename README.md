A basic android template for building libraries using gradle and which is configured for publishing to jcenter.

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
http://inthecheesefactory.com/blog/how-to-upload-library-to-jcenter-maven-central-as-dependency/en


### Create "local.properties" file in the root project folder
* bintray.user=[eg. user123] 
* bintray.apikey=[eg. 3ff1763398311f0d73ab7e42f3e46h3457132b42]
* bintray.gpg.password=[eg. password123]

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


### Gradle ext properties

```
ext {
    property = 'value'
}
println project.property
println project.getExtensions().getExtraProperties().get('property')
```