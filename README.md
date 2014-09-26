Cordova Parse.com Plugin
=========================

Cordova 3.5.0 plugin for Parse.com push service

Using [Parse.com's](http://parse.com) REST API for push requires the installation id, which isn't available in JS

Parse iOS SDK 1.3.0  
Parse Android SDK 1.6.0  

This plugin exposes the four native Android API push services to JS:

* <a href="https://www.parse.com/docs/android/api/com/parse/ParseInstallation.html#getInstallationId()">getInstallationId</a>
* <a href="https://www.parse.com/docs/android/api/com/parse/PushService.html#getSubscriptions(android.content.Context)">getSubscriptions</a>
* <a href="https://www.parse.com/docs/android/api/com/parse/PushService.html#subscribe(android.content.Context, java.lang.String, java.lang.Class, int)">subscribe</a>
* <a href="https://www.parse.com/docs/android/api/com/parse/PushService.html#unsubscribe(android.content.Context, java.lang.String)">unsubscribe</a>

Installation
------------

Pick one of these two commands:

```
phonegap local plugin add https://github.com/Triztian/cordova-parse-plugin
cordova plugin add https://github.com/Triztian/cordova-parse-plugin
```

## Build

### Android

If the application seems to crash after pausing it; try adding the following preference to your cordova `config.xml`
```
<preference name="KeepRunning" value="false"/>
```

 * http://cordova.apache.org/docs/en/3.5.0/guide_platforms_android_config.md.html#Android%20Configuration
 * 

### iOS

If you get the error of "undefined symbols for architecture <architecture>" and the missing symbols appear to be related to 
the Facebook SDK. You will have to modify the build flags for the iOS project, remove the "-ObjC" flag from
the `platforms/ios/<YourProject>.xcodeproj/project.pbxproj` file, somewhere along line 560, the first after the start of the
"XCBuildConfiguration" section.

 * http://stackoverflow.com/questions/23873654/undefined-symbols-for-architecture-arm64-parse
 * https://parse.com/questions/use-parse-with-objc
 * https://www.parse.com/questions/undefined-symbol-for-architecture-armv7-referenced-from-pffacebook


Initial Setup
-------------

Once the device is ready, call ```parsePlugin.initialize()```. This will register the device with Parse, 
you should see this reflected in your Parse control panel. After this runs you probably want to save the 
`installationID` somewhere, and perhaps subscribe the user to a few channels. 

Here is a contrived example.

```
parsePlugin.initialize(appId, clientKey, function() {

	parsePlugin.subscribe('SampleChannel', function() {
		
		parsePlugin.getInstallationId(function(id) {
		
			/**
			 * Now you can construct an object and save it to your own services, or Parse, and corrilate users to parse installations
			 * 
			 var install_data = {
			  	installation_id: id,
			  	channels: ['SampleChannel']
			 }
			 *
			 */

		}, function(e) {
			alert('error');
		});

	}, function(e) {
		alert('error');
	});
	
}, function(e) {
	alert('error');
});

```


Usage
-----
```
<script type="text/javascript>
	parsePlugin.initialize(appId, clientKey, function() {
		alert('success');
	}, function(e) {
		alert('error');
	});
  
	parsePlugin.getInstallationId(function(id) {
		alert(id);
	}, function(e) {
		alert('error');
	});
	
	parsePlugin.getSubscriptions(function(subscriptions) {
		alert(subscriptions);
	}, function(e) {
		alert('error');
	});
	
	parsePlugin.subscribe('SampleChannel', function() {
		alert('OK');
	}, function(e) {
		alert('error');
	});
	
	parsePlugin.unsubscribe('SampleChannel', function(msg) {
		alert('OK');
	}, function(e) {
		alert('error');
	});
</script>
```

Compatibility
-------------
Phonegap > 3.0.0  
Cordova > 3.0.0
