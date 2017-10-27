# JavaScript App Template Style Guide

## What is a NativeScript app template?
The NativeScript app template is a blueprint for a fully functional mobile application developed with NativeScript. Each app template resides in its own git repo (e.g. https://github.com/NativeScript/template-master-detail) and is usually published as an npm package as well (e.g. https://www.npmjs.com/package/tns-template-master-detail). You use the tns create CLI command to create mobile app from app template like this:
```
tns create my-app-name --template tns-template-master-detail 
```
OR 
```
tns create my-app-name --template https://github.com/NativeScript/template-master-detail
```
Progress Software develops and maintains a number of app templates but you can also create templates that suit your business requirements with ease. The purpose of this document is to provide an opinionated guide for app template development that ensures a level of consistency, maintainability, and ease of use for the developer that consumes the app template product (no matter whether it is developed by Progress Software, or not).

## Style vocabulary
Each guideline describes either a good or bad practice, and all have a consistent presentation.  

Do is one that should always be followed. Always might be a bit too strong of a word. Guidelines that literally should always be followed are extremely rare. On the other hand, you need a really unusual case for breaking a Do guideline.

Consider guidelines should generally be followed. If you fully understand the meaning behind the guideline and have a good reason to deviate, then do so. Please strive to be consistent.

Avoid indicates something you should never do.

## Getting started
Consider using the following workflow to facilitate app template development, testing, and debugging:  

Clone the [seed template](https://github.com/NativeScript/template-blank) git repo locally to [work-folder]\template-blank:
```
git clone git@github.com:NativeScript/template-blank.git
```  
Execute `tns create` CLI command to create an app from **the same template** to [work-folder]\blank:
```
tns create blank --template tns-template-blank
```
Save the contents of [work-folder]\blank\app\package.json somewhere in text editor (you will need them in a bit).  

Delete the whole [work-folder]\blank\app folder.  

Using [Link Shell Extension](http://schinagl.priv.at/nt/hardlinkshellext/linkshellextension.html) (for Windows) create symbolic link to [work-folder]\template-blank (the git-controlled folder created above) in [work-folder]\blank and rename [work-folder]\blank\template-blank to [work-folder]\blank\app.  

Replace what is in [work-folder]\blank\app\package.json with the contents you saved aside above (this effectively modifies [work-folder]\template-blank\package.json as well so **make sure you do not commit this change to git** -- this is a local change that allows you to actually `tns run android` or `tns run ios` the app from the [work-folder]\blank folder).

Now you can develop / deploy / debug your app template from [work-folder]\blank, then you can commit changes to git from [work-folder]\template-blank.  

NOTE, however, that you have to be extra careful when commiting changes to git as you don't want to commit the local "dev" changes in [work-folder]\template-blank\package.json (symlinked to [work-folder]\blank\app\package.json) as this will break the `tns create` CLI command for your git-controlled template. 
Also, make sure not to commit changes to [work-folder]\template-blank\App_Resources\Android\app.gradle (or any other App_Resources file modified in the symlinked copy when tns-running the app) either.
Also, if you want to commit actual changes to the [work-folder]\template-blank\package.json file (e.g. new dependencies to the git-controlled template) you will have to make them in two places -- modify [work-folder]\blank\package.json (not symlinked!) to tns-run and test the changes locally and modify [work-folder]\template-blank\package.json to commit them.

## App template structure
Do create folders named for the feature area they represent.  
*Each feature area should be placed in a separate folder in the template's folder structure.*

Do place each page, view model, and service in its own file.  
*Apply the [single responsibility principle (SRP)](https://wikipedia.org/wiki/Single_responsibility_principle) to all pages, view models, services, and other symbols. This helps make the app cleaner, easier to read and maintain, and more testable.*

Consider creating a folder for a page when it has multiple accompanying files (.js, .xml, .scss/css, etc.).

Avoid putting all of your app template's code in a root folder named "app".  
*When the actual app is created from the template, all of the template's code will indeed go inside a root "app" folder but you MUST NOT define this folder in the hierarchy of your template; otherwise the tns create CLI command will not function properly.* 

## Package.json guidelines
Do place a package.json file in the root folder of your app template.  
*Note this is not the actual root package.json of the generated mobile app – it is only used by the `tns create` CLI command upon app creation. Do not expect that everything you place in your package.json will be transferred to the actual package.json file. Notably `scripts` property content is removed, however, if you provide preinstall / postinstall scripts they will be executed before getting removed. You can use this mechanism to generate / move settings files to the root folder of the generated app and/or generate actual "scripts" content for the resulting app package.json – see [copying settings files](https://github.com/NativeScript/template-master-detail/blob/master/tools/preinstall.js) and [generating `scripts` commands on-the-fly](https://github.com/NativeScript/template-master-detail/blob/master/tools/postinstall.js) for concrete examples.*

Do provide a value for the `name` property using the format: **tns-template-[custom-template-name-goes-here]**.  
*Note this property value is NOT transferred to the root package.json file generated by the tns create CLI command but can be found in the app/package.json file of the generated app.*

Do provide a value for the `version` property following semver rules (e.g. 1.0.0).  
*Note this property value is NOT transferred to the root package.json file generated by the tns create CLI command but can be found in the app/package.json file of the generated app.*

Do provide a value for the `main` property specifying the primary entry point to your app (usually **app.js**).  
*Note this property value is NOT transferred to the root package.json file generated by the tns create CLI command but can be found in the app/package.json file of the generated app.*

Do provide a value for the `android` property specifying V8 flags (at a minimum it should be set to `"android": { "v8Flags": "--expose_gc" }`).  
*Note this property value is NOT transferred to the root package.json file generated by the tns create CLI command but can be found in the app/package.json file of the generated app.*

Do provide a value for the `displayName` property (user-friendly template name to be used in a future integration with NativeScript Sidekick).  
*Note this property value is NOT transferred to the root package.json file generated by the tns create CLI command.*

Do provide a value for the `repository` property specifying the place where your code lives.  
*Note this property value is NOT transferred to the root package.json file generated by the tns create CLI command.*
*Note correct `repository` property value is essential for the future integration with NativeScript Marketplace. Check the following section “Marketplace guidelines” for other integration requirements as well.*

Do provide a value for the following additional set of package.json properties: `description`, `license`, `readme`, `dependencies`, `devDependencies`.  
*Note these property values are transferred to the root package.json file generated by the tns create CLI command. For example https://github.com/NativeScript/template-master-detail/blob/master/package.json has the following minimal structure:*
```
{
  "name": "tns-template-master-detail",
  "displayName": "Master-Detail with Firebase",
  "main": "app.js",
  "version": "3.2.1",
  "description": "Master-detail interface to display collection of items from Firebase and inspect and edit selected item properties. ",
  "license": "SEE LICENSE IN <your-license-filename>",
  "readme": "NativeScript Application",
  "repository": {
    "type": "git",
    "url": "https://github.com/NativeScript/template-master-detail"
  },
  "android": {
    "v8Flags": "--expose_gc"
  },
  "dependencies": {
	...
  },
  "devDependencies": {
	...
  }
}
```

## Marketplace guidelines
Do publish your app template to npm (https://www.npmjs.com/) using **tns-template-[custom-template-name-goes-here]** format for the npm package name.

Do provide a screenshot preview to be used in a future NativeScript Marketplace integration under **tools/assets/marketplace.png** in your app template folder structure.  
Check [tools/postinstall.js](https://github.com/NativeScript/template-master-detail/blob/master/tools/postinstall.js) that implements a mechanism for removing the "tools" infrastructure folder from the generated app.

Do provide correct `repository` property value in the root package.json file of your app template (see the "Package.json guidelines" section above for additional package.json requirements).

## Styling
Consider using the [NativeScript core theme](https://github.com/NativeScript/theme) for styling your app template.

Consider using SASS for styling your app template.

Consider using the following infrastructure to enable cross-platform SASS styling for your app template:  
**_app-variables.scss** file in the app template's root folder should import the NativeScript core theme variables and any custom colors or theme variable overrides you might use:
``` scss
// Import the theme's variables. If you're using a color scheme 
// other than "light", switch the path to the alternative scheme, 
// for example '~nativescript-theme-core/scss/dark'. 
@import '~nativescript-theme-core/scss/light'; 

// Custom colors 
$blue-dark: #022734 !default; 
$blue-light: #02556E !default; 
$blue-50: rgba($blue-dark, 0.5) !default; 

// ... 

/** 
* Theme variables overrides 
**/ 

// Colors 
$background: #fff; 
$primary: lighten(#000, 13%); 

// ...
```


**_app-common.scss** file in the app template's root folder should contain any styling rules to be applied both on iOS and Android:
``` scss
// Place any CSS rules you want to apply on both iOS and Android here. 
// This is where the vast majority of your CSS code goes. 
  
// Font icon
.fa {
   font-family: "FontAwesome";
}  

//Action bar
.action-item,
NavigationButton {
    color: $ab-color;
}

// ...
```

**app.android.scss** file in the app template's root folder should import the app variables, the NativeScript core theme main ruleset, and the common styles; also place any styling rules to be applied only on Android here:
``` scss
// Import app variables 
@import 'app-variables'; 

// Import the theme's main ruleset - both index and platform specific. 
@import '~nativescript-theme-core/scss/index'; 
@import '~nativescript-theme-core/scss/platforms/index.android'; 

// Import common styles 
@import 'app-common'; 

// Place any CSS rules you want to apply only on Android here
.action-item {
    padding-right: 10;
    height: 100%;
}

// ...
```

**app.ios.scss** file in the app template's root folder should import the app variables, the NativeScript core theme main ruleset, and the common styles; also place any styling rules to be applied only on iOS here:
``` scss
// Import app variables
@import 'app-variables';

// Import the theme’s main ruleset - both index and platform specific.
@import '~nativescript-theme-core/scss/index';
@import '~nativescript-theme-core/scss/platforms/index.ios';

// Import common styles
@import 'app-common';

// Place any CSS rules you want to apply only on iOS here

// ...
```

Consider using the following infrastructure to enable cross-platform SASS styling on page level:
**_[page-name]-page.scss** in the respective feature folder should contain the style rules to be applied both on iOS and Android for **[page-name]-page.js** (e.g. if styling **cars/car-list-page.js**, the file should be **cars/_car-list-page.scss**):
``` scss
// Start custom common variables
@import '../app-variables';
// End custom common variables

// Custom styles
.list-group {
    .list-group-item {
        padding: 0 0 8 0;
        background-color: $blue-10;

        .list-group-item-content {
            padding: 8 15 4 15;
            background-color: $background-light;
        }

        .fa {
            color: $accent-dark;
        }
    }
}  

// ...
```

**[page-name]-page.android.scss** in the respective feature folder should contain the style rules to be applied only on Android for **[page-name]-page.js** (e.g. if styling **cars/car-list-page.js**, the file should be **cars/car-list-page.android.scss**):
``` scss
@import 'cars-list-page';

// Place any CSS rules you want to apply only on Android here 

// ...
```

**[page-name]-page.ios.scss** in the respective feature folder should contain the style rules to be applied only on iOS for **[page-name]-page.js** (e.g. if styling **cars/car-list-page.js**, the file should be **cars/car-list-page.ios.scss**):
``` scss
@import 'cars-list-page';

// Place any CSS rules you want to apply only on iOS here

// ...
```

## Services
### Delegate complex view model logic to services
Consider limiting logic in a view model to only that required for the view. All other logic should be delegated to services.
*Sample service implementation can be found [here](https://github.com/NativeScript/template-master-detail/blob/master/cars/shared/car-service.js); sample service usage from the view model can be found [here](https://github.com/NativeScript/template-master-detail/blob/master/cars/cars-list-view-model.js).*

Consider moving reusable logic to services and keep pages and view models simple and focused on their intended purpose.

### Single responsibility
Do create services with a single responsibility that is encapsulated by its context.

Do create a new service once the service begins to exceed that singular purpose.

## Data services
### Talk to the server through a service
Consider refactoring logic for making data operations and interacting with data to a service.

Consider making data services responsible for XHR calls, local storage, stashing in memory, or any other data operations.

## Coding conventions
Consider using eslint and the [.eslintrc](https://github.com/NativeScript/template-master-detail/blob/master/tools/.eslintrc)/[.eslintignore](https://github.com/NativeScript/template-master-detail/blob/master/tools/.eslintignore) ruleset provided by Progress Software to ensure maximum consistency with the standard app templates.

### Classes
Do use upper camel case when naming classes.

### Variables and constants
Do declare variables with let instead of var.

Do declare variables with const if their values should not change during the application lifetime.

Consider spelling const variables in lower camel case.

Do tolerate existing const variables that are spelled in UPPER_SNAKE_CASE.

### Properties and methods
Do use lower camelcase to name properties and methods.

### Events
Do name event handler methods with the prefix **on** followed by the event name (e.g. itemTap event -> onItemTap(...) event handler).

Consider specifying the semantic element that is acted upon (e.g. onCarItemTap(), or onDrawerButtonTap()).

Consider declaring event handlers in page code-behind and not directly in the view model (event handler implementation can then call a view model method on its own).

## Naming
### General naming guidelines
Do use consistent names for all symbols.

Do use dashes to separate words in the descriptive name.

Do follow a pattern that describes the symbol's feature then its type. The recommended pattern is **feature-name-type.js|css|scss|xml** (not applicable for custom components).
```
car-list-page.js
car-list-page.xml
car-list-view-model.js
```

Do follow a pattern that describes the custom component's feature. The recommended pattern is **[FeatureName].js|css|scss|xml** as custom components get their selector from the file name (e.g. if you have **shared/my-drawer/MyDrawer.js|scss|xml**, you will use it like:
```xml
<Page
    class="page"
    navigatingTo="onNavigatingTo"  
    xmlns:nsDrawer="nativescript-pro-ui/sidedrawer"
    xmlns:myDrawer="shared/my-drawer"
    xmlns="http://www.nativescript.org/tns.xsd">
    
    <!-- ... -->
	
    <nsDrawer:RadSideDrawer id="sideDrawer" showOverNavigation="true">
        
		<!-- ... -->

        <nsDrawer:RadSideDrawer.drawerContent>
            <StackLayout>
                <myDrawer:MyDrawer selectedPage="Home" />
            </StackLayout>
        </nsDrawer:RadSideDrawer.drawerContent>

		<!-- ... -->
		
    </nsDrawer:RadSideDrawer>
</Page>
```

### Symbols and file names
Do use consistent names for all assets named after what they represent.

Do use upper camel case for class names.

Do match the name of the symbol to the name of the file.

### Service Names
Do use consistent names for all services named after their feature.

Do suffix a service class name with Service. For example, something that gets data or cars should be called a **DataService** or a **CarService**.  
*A few terms are unambiguously services. They typically indicate agency by ending in "-er". You may prefer to name a service that logs messages Logger rather than LoggerService. Decide if this exception is agreeable in your project. As always, strive for consistency.*

### Bootstrapping
Do put bootstrapping and platform logic for the app in a file named **app.js**.

Avoid putting app logic in **app.js**. Instead, consider placing it in a page, view model, or service.