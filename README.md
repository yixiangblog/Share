Cordova Simple Share Plugin for Android
================================

A very simple plugin for Cordova 3.x.x to share a message and an image using Android's native share dialog.

**Important Note:** This plugin can only share files from external cache directory.

## Why Use This Plugin ##
- Been tested to work on Android 5.0 and above.
- Doesn't require Write External Storage permission.
- Smaller apk size (Compared to [social sharing plugin](https://github.com/EddyVerbruggen/SocialSharing-PhoneGap-Plugin)).

## Supported Platforms ##
- **Android** 5.0 and above

## Why Only External Cache Directory ##
**tl;dr:** To support both Android 5.0 and above, and not requiring Write External Cache permission.

Your app should require as few permissions as possible to run, and Write External Storage is a dangerous permission. If you need this permission, you, the app developer, should add it yourself. It doesn't make sense that your app requires a dangerous permission just because you used a certain plugin, especially when such a plugin can do its job without this dangerous permission.

When it comes to sharing, you can write to your app's files/data/cache directory without Write External Storage permission, and supposedly, you should be able to share from these folders too. Yet when I try it out, it seems to not work on 5.0 devices. It's not documented or discussed anywhere, so I don't know if it's just my phone's problem. Anyway, I found a solution by sharing from exteral cache directory, it works both for 5.0 and above, and doesn't require Write External Storage permission, so I decided to share this solution here.

## Adding the Plugin to your project ##
Through the [Command-line Interface](http://cordova.apache.org/docs/en/3.0.0/guide_cli_index.md.html#The%20Command-line%20Interface):
```
cordova plugin add https://github.com/yixiangblog/cordova-plugin-simple-share.git --save
```

## Using the plugin ##
The plugin creates the object ```window.plugins.simpleshare``` with one method:

### show() ###
Takes three parameters: a success callback, a fail callback, and a set of message parameters
```javascript
window.plugins.simpleshare.show(messageParams, success, fail);
```

messageParams should be an object with four properties:
```javascript
{
	subject:	'subject line',  // string, required
	text:		'text here',     // string, required
	imagePath: 	'path/to/image', // string, required
	mimeType:	'type'           // string, required
}
```

imagePath should be a valid path to a local file. Use PhoneGap's File and FileTransfer core plugins if you need to fetch an image over the web.

mimeType should be one of "image/jpeg", "image/gif", "image/png" or "image/bmp", matching the image.

####  Example ####
```javascript
window.plugins.simpleshare.show({
	subject: '',
	text: '',
	imagePath: window.cordova.file.externalCacheDirectory + 'example.jpg',
	mimeType: 'image/jpeg'
}, success, fail);
function success() {}
function fail(error) {console.log('Error: ' + error);}
```

Please note that you'll need the [File plugin](http://cordova.apache.org/docs/en/latest/reference/cordova-plugin-file/index.html) to run this example.

## How to Remove Write External Storage permission from you app ##
Cordova's official File plugin adds a Write External Storage permission by default. To remove it, find `platforms/android/android.json`, and search for the following line:

```Json
"xml": "<uses-permission android:name=\"android.permission.WRITE_EXTERNAL_STORAGE\" />"
```

And remove the block containing this line, which may look like:

```Json
"/*": [
  {
    "xml": "<uses-permission android:name=\"android.permission.WRITE_EXTERNAL_STORAGE\" />",
    "count": 1
  }
]
```

Remember to remove trailing commas too.

## Credit ##
Forked from [Cordova Share Plugin](https://github.com/RonaldPK/Share)
