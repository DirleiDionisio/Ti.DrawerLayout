Ti.DrawerLayout
===============

titanium module: android drawer layout


사용법은 example/app.js를 참조바람


#### demo app: https://github.com/ricardoalcocer/Ti.DrawerLayout-Demo-Alloy-App


##Native ActionBar Support (>=1.1.0)

ActionBar homeIcon click event is handled outside of module, because Activity should be sub-classed to override onOptionsItemSelected().

```
	win.activity.actionBar.onHomeIconItemSelected = function() {
		drawer.toggleLeftWindow();
	}
```

Require Titanium Mobile SDK >= 3.3.0


##Right Drawer Support (>=1.0.2)

Set 'rightView' and 'rightDrawerWidth' of drawer view.


```
var drawer = TiDrawerLayout.createDrawer({
    leftView: menuTable,
    centerView: contentView,
	rightView: filterView,
    leftDrawerWidth: "240dp",
	rightDrawerWidth: "120dp",
    width: Ti.UI.FILL,
    height: Ti.UI.FILL
});
```


Assign these properties at any time. Right drawer will be created when it is needed. This behavior has been applied to left drawer too.

```
var drawer = TiDrawerLayout.createDrawer({
    leftView: menuTable,
    centerView: contentView,
    leftDrawerWidth: "240dp",
});

drawer.rightView = filterView;
drawer.rightDrawerWidth = "120dp";
```


Event will be fired with 'drawer' parameter.

```
drawer.addEventListener('draweropen', function(e) {
	if (e.drawer == "left") {
        // left drawer is open
	} else if (e.drawer == "right") {
        // right drawer is open
	}
});
```


##Trouble Shooting: android support library version (only for Ti < 3.3)

Check android support library v4 version in the titanium sdk installation,
and replace android support v4 library jar file to the latest version.

for osx
~/Library/Application Support/Titanium/mobilesdk/osx/3.2.0.GA/android

the current (2014/april/15) file size of android-support-v4.jar is 648kb


support library would be found in your android sdk path:
{install path}/android/extras/android/support/v4/android-support-v4.jar

PS: Titanium SDK >= 3.3 already use the new android-support-v4.jar


##Building module on Titanium Studio

- Follow instructions on [Android Module Development Guide - Installing the Necessary Components](http://docs.appcelerator.com/titanium/latest/#!/guide/Android_Module_Development_Guide-section-29004945_AndroidModuleDevelopmentGuide-InstallingtheNecessaryComponents)
- Clone this repository
- Create .project file
- Create .apt_generated folder
- Create build/.apt_generated folder
- Create .classpath file using .classpath.example as template and:
	- replace {ANDROID_SDK_PATH} by your Android SDK folder
	- replace {APPLICATION_SUPPORT_PATH_WHERE_TITANIUM_LIVES} by appropriated path (ex: /Users/dirlei/Library/Application Support)
	- replace {TITANIUM_SDK_VERSION} by your desired Ti SDK version (ex: 3.3.0.GA)
- Create build.properties using build.properties.example as template and:
	- replace {ANDROID_SDK_PATH} by your Android SDK folder
	- replace {APPLICATION_SUPPORT_PATH_WHERE_TITANIUM_LIVES} by appropriated path (ex: /Users/dirlei/Library/Application Support)
	- replace {TITANIUM_SDK_VERSION} by your desired Ti SDK version (ex: 3.3.0.GA)
- Import project on Titanium Studio (File/Import/Existing Projects into Workspace, select folder where you cloned repository, Finish)
- Build module (select module project, right click, Publish/Package)


## Usage (Alloy)

### index.xml
```
</Alloy>
    <Window id="mainWindow" platform="android" navBarHidden="true">
    	<!-- add header bar here -->
    </Window>
</Alloy>
```

### menu.xml
```
<Alloy>
	<TableView id="menuTable">
		<TableViewRow>Item 1</TableViewRow>
		<TableViewRow>Item 2</TableViewRow>
	</TableView>
</Alloy>
```
### main.xml
```
<Alloy>
	<View>
		<!-- main page content here -->
	</View>
</Alloy>
```

### index.js
```
// Android only
if (OS_ANDROID) {
	// Load module
	var TiDrawerLayout = require('com.tripvi.drawerlayout');
	
	// define menu and main content view
	var menuTable = Alloy.createController('menu').getView();
	var contentView = Alloy.createController('main').getView();
	
	var drawer = TiDrawerLayout.createDrawer({
	        leftView: menuTable,
	        centerView: contentView,
	        leftDrawerWidth: "240dp",
	        width: Ti.UI.FILL,
	        height: Ti.UI.FILL
	});

/* Available methods:
 *  
 *   toggleLeftWindow()
 *   openLeftWindow()
 *   closeLeftWindow()
 *   toggleRightWindow()
 *   openRightWindow()
 *   closeRightWindow()
 *   isLeftWindowOpen()  -> returns boolean
 *   isRightWindowOpen()  -> returns boolean
 * 
 */

/* Available events */

	drawer.addEventListener('draweropen', function(e) {
	        // drawer is open
	});

	drawer.addEventListener('drawerclose', function(e) {
	        // drawer is closed
	});
	
	drawer.addEventListener('drawerslide', function(e) {
			// drawer is sliding
	        // slide offset: e.offset
	});
	
	$.mainWindow.add(drawer);
	$.mainWindow.open();
}
```
