# react-native-app-shortcuts

## Introduction
Library for creating Android [App Shortcuts](https://developer.android.com/guide/topics/ui/shortcuts.html) in React Natvie.

**Google Official intro for App Shortcuts**

If your app targets Android 7.1 (API level 25) or higher, you can define shortcuts to specific actions in your app. These shortcuts can be displayed in a supported launcher. Shortcuts let your users quickly start common or recommended tasks within your app.

<img src="https://developer.android.com/images/guide/topics/ui/shortcuts.png" height="400" />

## Requirements
Android SDK version >= `25`

## Getting started

`$ npm install react-native-app-shortcuts --save`

### Mostly automatic installation

`$ react-native link react-native-app-shortcuts`

### Manual installation

#### Android

1. Open up `android/app/src/main/java/[...]/MainActivity.java`
  - Add `import com.rnas.RNAppShortcutsPackage;` to the imports at the top of the file
  - Add `new RNAppShortcutsPackage()` to the list returned by the `getPackages()` method
2. Append the following lines to `android/settings.gradle`:
  	```
  	include ':react-native-app-shortcuts'
  	project(':react-native-app-shortcuts').projectDir = new File(rootProject.projectDir, 	'../node_modules/react-native-app-shortcuts/android')
  	```
3. Insert the following lines inside the dependencies block in `android/app/build.gradle`:
  	```
      compile project(':react-native-app-shortcuts')
  	```

## Usage
Import the package
```js
import RNAppShortcuts from 'react-native-app-shortcuts';
```

### Add and update App Shortcut

**Add App Shortcut**
```js
RNAppShortcuts.addShortCut({
	id: 'id1',
	shortLabel: 'sample',
	longLabel: 'sample label',
	iconFolderName: 'drawable',
	iconName: 'icon',
	activityName: 'MainActivity'
})
```

**Update App Shortcut**
```js
RNAppShortcuts.updateShortCut({
	id: 'id1',
	shortLabel: 'updated',
	longLabel: 'updated label',
	iconFolderName: 'drawable',
	iconName: 'icon',
	activityName: 'AnotherActivity'
})
```

**Options Meaning**
| Name | Type | Description |
| --- | ---  | --- |
| id | String | Id of the shortcut. |
| shortLabel | String | Short label for the shortcut. |
| longLabel | String | Long label for the shortcut. |
| iconFolderName | String | Folder name of the shortcut's icon. For example, if the icon is in `./android/app/src/res/drawable` folder of your prject, you should use `'drawable'` here.
  |
| iconName | String | Name of the icon, without extension name. |
| activityName | String | **default: 'MainActivity'** Name of the activity which the intent of the shortcut will go to. Since there is only one activity(MainActivity) in most React Naive app. So we set it as the default value. This options is optional. |

### Remove App ShortCut

**Remove specify shortcut**

You can remove a shortcut just by passing the id into `RNAppShortcuts.removeShortCut`.
```js  
RNAppShortcuts.removeShortCut('id')
```

**Remove all shortcuts**
```js  
RNAppShortcuts.removeAllShortCuts()
```

### Check the existence of the Shortcut
Set the id of the parameter of `RNAppShortcuts.exists`.
```js
RNAppShortcuts.exists('id').then(function() {
	// Exists
}).catch(function(err) {
	// Not exists
})
```

### Handle App Shortcut
In Native android, we set an Intent to describe the behavior after trigger the shortcut. But in React Native, since there mostly only one activtity, that is the `MainActivity`. As well you can let the shortcut turn to other Activity you have created. We can just handle the shortcut after it triggered in React Native by JavaScript.

`RNAppShortcuts.handleShortcut` will trigger after you click the shortcut. And you will get the id of the shortcut here.
```js
RNAppShortcuts.handleShortcut(function(id) {
	// Do anything you want. Just like navigate to specify page by the id and so on.
})
```