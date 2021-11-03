# Step 1 create project

    react-native init POCSettings

# Step 2 init pods and npm

    cd POCSettings
    npm install
    cd ios
    pod install

# Step 3 check if your project runs

    react-native run-ios

This takes some building-time but should open an IOS-simulator with a running application.

# Step 4 open your project in xcode

Open xcode and click on 'open a project or file', Then navigate to your ios folder in your project and open the file 
POCSettings.xcworkspace

    e.g. .../POCSettings/ios/POCSettings.xcworkspace

# Step 5 create a Root.plist file

From your top-bar select file->new->file (or command+N). Scroll down to the resource section and select 
'Settings Bundle' and click next. You are presented with a save menu. Use the settings below:

    Save As:    Settings
    Tags: 
    Where:      ios
    Group:      POCSettings
    Targets:    select at least 'POCSettings'

And click create. In the project navigator on the left-hand side of your window a new Folder and file will appear.

# Step 6 configure the Root.plist file

Either configure the Root.plist file from xcode or open the file in your favorite (xml) editor. For this example we'll
create a boolean switch and a text field parameter. In this file you can add Settings parameters. You can revere to 
these parameters in your React-Native code via the 'identifier' in xcode or the key/string pair in xml.

    <key>Key</key>
    <string>backendServer</string>  <- backendServer is the parameter name used in React-Native!!!

Here's a full example file for your convenience (parameter names are 'development' and 'backendServer':

    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
	    <key>StringsTable</key>
	    <string>Root</string>
	    <key>PreferenceSpecifiers</key>
	    <array>
    		<dict>
    			<key>Type</key>
    			<string>PSGroupSpecifier</string>
    			<key>Title</key>
    			<string>Development backend-server</string>
    		</dict>
    		<dict>
	    		<key>Type</key>
    			<string>PSToggleSwitchSpecifier</string>
	    		<key>Title</key>
		    	<string>Enable</string>
			    <key>Key</key>
    			<string>development</string>
	    		<key>DefaultValue</key>
		    	<false/>
    		</dict>
	    	<dict>
		    	<key>Type</key>
			    <string>PSTextFieldSpecifier</string>
    			<key>Key</key>
	    		<string>backendServer</string>
		    	<key>KeyboardType</key>
			    <string>Alphabet</string>
    			<key>Title</key>
	    		<string>Host:Port</string>
		    	<key>DefaultValue</key>
			    <string>10.0.0.1:8000</string>
    		</dict>
	    </array>
    </dict>
    </plist>

When you're done configuring this file, you can close xcode if you so please. Rebuild your app and head to you Settings
page in the IOS-simulator. Confirm that there is now a settings item for the POCSettings-app with a boolean slider and a
text-field. Congrats!

    note: If you change any of the values, they should remain even if you rebuild the application

# Step 7 add the framework for reading plist files into your project

We're all set for installing the npm module 'react-native-ios-settings-bundle'.

Open a terminal and head to the root of your project directory and install the npm module and run pod install

    cd .../POCSettings
    npm install react-native-ios-settings-bundle --force --save
    npm audit fix --force
    cd ios
    pod install

# Step 8 add requirements to your project

For this proof-of-concept app we're going to edit the 'App.js' file. For your project this might not be the appropriate 
location but the concept is the same.

First we need to import the newly added framework from step 7. Add an import statement to the 'App.js' file.

    import RNIosSettingsBundle from 'react-native-ios-settings-bundle';

Next we're going to create to pop-ups, just to prove that we can actually read from the Root.plist file.

    let development;
    RNIosSettingsBundle.get('backendServer',(err,value)=>{
            if(value && !err) {
              development = value
              alert(development)
            }
            else {
              console.log(err);
            }
          })
    
    let backendServer;
    RNIosSettingsBundle.get('backendServer',(err,value)=>{
            if(value && !err) {
              backendServer = value
              alert(backendServer)
            }
            else {
              console.log(err);
            }
          })

If you build and run the app in the IOS-simulator your should now get two pop-ups showing the values set in the Root.plist
file. Feel free to change the Settings and restart the app to see if the changes are reflected in the pop-ups.

Have fun!
