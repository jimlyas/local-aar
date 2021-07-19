# local-aar
Localize your android library module using AAR and define which module to use during your app development.

## Why Localize library module
After android projects grew bigger and implement modularization, there's certain issue that rises with it. By implementing localization, you can *improve gradle build time* as you're not really using the library module but the published version of it as aar.<br/>

That's basically like telling gradle to leave your library module as-is and don't run anything on it when building apk, or even just trying to run your app.

## Requirement
Before using this gradle script, your library modules **need to**:
 - Doesn't have any maven publication configuration
 - Have a debug build variant
 
## Implementation
Follow this steps to start using this gradle scripts:
 - [Adding scripts to all library modules](#adding-scripts-to-all-library-modules)
 - [Configuration consumer modules](#configuration-consumer-modules)
 - [`settings.gradle` configuration](#settingsgradle-configuration)
 - [Publish library modules](#publish-library-modules)
 - [`local.properties` configuration](#localproperties-configuration)


### Adding scripts to all library modules
You may have more than one library modules in your project, add this line in all your library modules's `build.gradle`:
```groovy
apply from: 'https://raw.githubusercontent.com/jimlyas/local-aar/main/publish.gradle'
```
<br/>

### Configuration consumer modules
Your app module or even your modules now might use the localized AAR, add this line to all your aar consumer's `build.gradle`:
```groovy
apply from: 'https://raw.githubusercontent.com/jimlyas/local-aar/main/local-aar.gradle'
```
Then change your library module declaration from:
```groovy
implementation project(':library-a')
```
To this:
```groovy
implementation modulePath(':library-a')
```
<br/>

### `settings.gradle` configuration
In your project's `settings.gradle`, we will define ability to swap certain module during development. Configure it like this:
```groovy
// Add gradle script
apply from: 'https://raw.githubusercontent.com/jimlyas/local-aar/main/local-aar.gradle'

rootProject.name = 'SampleApp'
include ':app'

// For your library modules, use includeIfEnabled
includeIfEnabled(':library-a')
includeIfEnabled(':library-b')
includeIfEnabled(':feature-a')
```
<br/>

### Publish library modules
Before start developing, you'll have to publish all your library modules first as aar. Do that by running this command:
```shell
gradlew publish
```
If build successful, you should see `mavenLocal` folder in your root directory.
> You might want to add `mavenLocal` folder inside your `.gitignore` file.
<br/>

### `local.properties` configuration
We'll be using `local.properties` to define does you want to run your app with library module as AAR and what module you want to work with right now. Add this variable:
```groovy
// Build with AAR?
useAAR=true

// Modules you're working with right now, separated by space
modules=:app :some-modules-a :some-modules-b
```
Gradle sync, and that's it!

<br/>

## Reference
[How We Improved Performance and Build Times in Android Studio](https://medium.com/gojekengineering/how-we-improved-performance-and-build-times-in-android-studio-306028166b79)
