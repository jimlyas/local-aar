# local-aar
Localize your android library module using AAR and define which module to use during your app development.

## Requirement
Before using this gradle script, your library modules **need to**:
 - Doesn't have any maven publication configuration
 - Have a debug build variant
 
## Implementation
Follow this steps to start using this gradle scripts:
 - [Adding scripts to all library modules](#adding-scripts-to-all-library-modules)
 - [Adding scripts to consumer modules](#adding-scripts-to-consumer-modules)
 - [`settings.gradle` configuration](#settingsgradle-configuration)

<br/><br/>
### Adding scripts to all library modules
You may have more than one library modules in your project, add this line in all your library modules's `build.gradle`:
```groovy
apply from: 'https://raw.githubusercontent.com/jimlyas/local-aar/main/publish.gradle'
```
<br/>

### Adding scripts to consumer modules
Your app module or even your modules now might use the localized AAR, add this line to all your aar consumer's `build.gradle`:
```groovy
apply from: 'https://raw.githubusercontent.com/jimlyas/local-aar/main/local-aar.gradle'
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

## Reference
[How We Improved Performance and Build Times in Android Studio](https://medium.com/gojekengineering/how-we-improved-performance-and-build-times-in-android-studio-306028166b79)
