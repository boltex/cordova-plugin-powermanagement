PowerManagement
===============
Plugin for Cordova (3.0+)

Added 'isIgnoringBatteryOptimizations' and 'addAppToBatteryWhitelist' functions for use with Android 6.0.0+
=============================================================

The PowerManagement plugin offers access to the devices power-management functionality.
It should be used for applications which keep running for a long time without any user interaction.

For details on power functionality see:

* Android: [PowerManager](http://developer.android.com/reference/android/os/PowerManager.html)
* iOS: [idleTimerDisabled](http://developer.apple.com/library/ios/documentation/UIKit/Reference/UIApplication_Class/Reference/Reference.html#//apple_ref/occ/instp/UIApplication/idleTimerDisabled)
* WindowsPhone: [UserIdleDetectionMode](http://msdn.microsoft.com/en-US/library/windowsphone/develop/microsoft.phone.shell.phoneapplicationservice.useridledetectionmode%28v=vs.105%29.aspx)

Installation
---------
Install the plugin using the cordova command line utility:

`$ cordova plugin add https://github.com/bassena/cordova-plugin-powermanagement.git`

Usage
-----

### window.powerManagement.acquire(successCallback, failureCallback)
Acquire a wakelock by calling this.
```js
window.powerManagement.acquire(function() {
	console.log('Wakelock acquired');
}, function() {
	console.log('Failed to acquire wakelock');
});
```

### window.powerManagement.dim(successCallback, failureCallback)
This acquires a partial wakelock, allowing the screen to be dimmed.
```js
window.powerManagement.dim(function() {
	console.log('Wakelock acquired');
}, function() {
	console.log('Failed to acquire wakelock');
});
```

### window.powerManagement.release(successCallback, failureCallback)
Release the wakelock. It's important to do this when you're finished with the wakelock, to avoid unnecessary battery drain.
```js
window.powerManagement.release(function() {
	console.log('Wakelock released');
}, function() {
	console.log('Failed to release wakelock');
});
```

### [Android Only] window.powerManagement.setReleaseOnPause(enabled, successCallback, failureCallback)
By default, the plugin will automatically release a wakelock when your app is paused (e.g. when the screen is turned off, or the user switches to another app). It will reacquire the wakelock upon app resume. If you would prefer to disable this behaviour, you can use this function.
```js
window.powerManagement.setReleaseOnPause(false, function() {
	console.log('Set successfully');
}, function() {
	console.log('Failed to set');
});
```

### [Android Only, API >= 23] window.powerManagement.isDeviceIdleMode(successCallback, failureCallback)
As of Android 6.0.0+, Android now has an extra power management feature called 'Doze'. This feature disables most device
components (network connection, GPS, etc.) when it is asleep for an extended period of time and is then placed in an 'idle' state.

To check to see if the device is in this state, you can use the following function:
```js
// 'state' is either 1 (in idle) or 0 (not in idle)
window.powerManagement.isDeviceIdleMode(function(state) {
	if (state === 1) {
		console.log('Device IS in idle mode.');
	}
else {
		console.log('Device IS NOT in idle mode.');
	}
	
}, function() {
	console.log('Failed to check the device\'s idle state.');
});
```

### [Android Only, API >= 23] window.powerManagement.addAppToBatteryWhitelist(successCallback, failureCallback)
As of Android 6.0.0+, Android now has an extra power management feature called 'Doze'. This feature disables most device
components (network connection, GPS, etc.) when it is asleep for an extended period of time and is then placed in an 'idle' state. Adding the app to the battery optimization whitelist can circumvent some of these issues.

To add the app that uses this plugin to the device's battery optimization whitelist, use the following function:
```js
window.powerManagement.addAppToBatteryWhitelist(function() {
	console.log('A dialog has popped up asking you to accept adding the app to the whitelist.');
}, function() {
	console.log('Failed to add the app to the device\'s battery optimization whitelist.');
});
```

### [Android Only, API >= 23] window.powerManagement.isIgnoringBatteryOptimizations(successCallback, failureCallback)
As of Android 6.0.0+, Android now has an extra power management feature called 'Doze'. This feature disables most device
components (network connection, GPS, etc.) when it is asleep for an extended period of time and is then placed in an 'idle' state.

To check to see if the app that uses this plugin is on the device's battery optimization whitelist, use the following function:
```js
// 'result' is either 1 (in whitelist), or 0 (not in whitelist)
window.powerManagement.isIgnoringBatteryOptimizations(function(result) {
	if (result === 1) {
		console.log('This app IS on the device\'s battery optimization whitelist.');
	}
	else {
		console.log('This app IS NOT on the device\'s battery optimization whitelist.');
	}
}, function() {
	console.log('Failed to add the app to the device\'s battery optimization whitelist.');
});
```

This function can be use in conjunction with the `window.powerManagement.addAppToBatteryWhitelist` above. Example:
```js
// 'result' is either 1 (in whitelist), or 0 (not in whitelist)
window.powerManagement.isIgnoringBatteryOptimizations(function(result) {
	// If the app isn't on the battery whitelist, open the dialog to add it
	if (result !== 1) {
		window.powerManagement.addAppToBatteryWhitelist(function() {
			console.log('A dialog has popped up asking you to accept adding the app to the whitelist.');
		}, function() {
			console.log('Failed to add the app to the device\'s battery optimization whitelist.');
		});
	}
	else {
		console.log('This app IS NOT on the device\'s battery optimization whitelist.');
	}
}, function() {
	console.log('Failed to add the app to the device\'s battery optimization whitelist.');
});
```
Note that in all the above examples, all callbacks are optional.

License
=======
Copyright 2013 Wolfgang Koller

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
