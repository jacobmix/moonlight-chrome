# Stuff to know/have before we begin:

[How to see file extensions](https://www.howtohaven.com/system/show-file-extensions-in-windows-explorer.shtml)   

Have an extractor/compresser. Get something like [WinRAR](http://www.rarlab.com/download.htm) or [7Zip](http://www.7-zip.org/download.html). Most likely you want the `64-bit x64` version.   
But [here](https://support.microsoft.com/en-us/help/827218/how-to-determine-whether-a-computer-is-running-a-32-bit-version-or-64)’s how you can check if your  PC is 32-bit or 64-bit.   

Know your Public IP adress. Just Google `My IP` should should show it tho.   
Know your Local IP. You can see it by opening a CMD window. Enter `ipconfig /all` Find your network, and it's IPv4 address.   

# Download & Installing:

## Method 1:

Download the latest `moonlight-chrome.crx` from [realeses](https://github.com/moonlight-stream/moonlight-chrome/releases). Open `chrome://extensions/` Make sure `Developer Mode` is on.   
Drag, and drop the `moonlight-chrome.crx` file you downloaded before. Into the extensions page. It should now install.   

Note: This method might not work for some because of how strict Google is with none-chrome-store extensions.   
So if you can’t download the crx file or drag & drop to install. See [method 2](https://github.com/jacobmix/moonlight-chrome/blob/master/documentation.md#method-2).   

## Method 2:

Download the latest `Source code (zip)` from [realeses](https://github.com/moonlight-stream/moonlight-chrome/releases).   
Extract it, and move the contents somewhere you’ll remember, and _not move around_.   
(Something like `C:\Unpacked Chrome Extensions\Moonlight-chrome` for example)   

Go to `chrome://extensions/` Make sure `Developer Mode` is on, and click `Load unpacked extension...`   
Then find, and select the `Moonlight-chrome` folder you moved.   
(Like the example above `C:\Unpacked Chrome Extensions\Moonlight-chrome`)   

You should now see `Moonlight Game Streaming` Enable it if it isn’t already.   
Now you can run it by clicking the blue `Launch` text under the name, and description.   
You can also make a shortcut by clicking `Details` Then `Create shortcuts...` You can also run it from an [app launcher](https://chrome.google.com/webstore/detail/apps-launcher/ijmgkhchjindcjamnckoiahagecjnkdc).   

Chrome might popup with a warning every time you start it up now. Saying something like:   
`Disable developer mode extensions`   
```Extentions running in a developer mode can harm your computer.
If you're not a developer, you should disable these extensions running in developer mode to stay save.
```
`Disable` `Cancel`   
`Learn more`   
Just press `Cancel` to continue without disabling Moonlght. If you don't want this popup every time you launch Chrome:   
[Here](https://stackoverflow.com/a/30361260)'s a script that will disable it when run as admin while Chrome is closed.   
Note: The popup will return when Chrome updates. So you might wanna keep the file or get it again.   

# Troubleshooting:

## Chrome Troubleshooting:

Make sure Chrome is up to date: `chrome://settings/help`   

Check if Video Decoding, and WebGL are supported or enabled: `chrome://gpu`   

## GeForce Experience Troubleshooting:

Make sure GeForce Experience, and video drivers are up to date.   

Make sure GameStream is on in GeForce Experience:   
GeForce Experience > Settings > wait for left tabs to load > Shield > GameStream > On (Green)  

Check if anything shows up when you go to `YourLocalIPHere:47989/serverinfo` if something does you should be fine.   

If `nvstreamer.exe` is running. Try ending it with taskmanager, and disable. Then re-enable GameStream.   

## Moonlight Troubleshooting:

[Read the Chrome App Logs](https://github.com/moonlight-stream/moonlight-chrome/wiki#reading-the-chrome-app-logs)   

[Delete stored parings](https://github.com/moonlight-stream/moonlight-chrome/wiki#deleting-all-stored-pairings)   

## Potforward & Troubleshooting:

Portforward these ports through your router/modem for your hosting PC.   
TCP: ``35043,47984,47989,47991,47995-47996,48010`` UDP: ``7,9,47989,47992,47998-48000,48010``   

## Firewall & Troubleshooting:

Allow all the ports through your hosting PC's firewall.   
This can be edited manually. But here is a script that will do it for you.   
Just copy it, and paste it into a txt file. Then rename the file extention to a bat file, and run it as admin.   

```
@echo off

cd /d "%~dp0"

echo Running Open GameStream Ports - ALL script.

set LOGFILE=Open_GameStream_Ports-ALL-Batch.log

call :LOG > %LOGFILE%

exit

:LOG

@echo on
set PORT_TCP_IN=7,9,7777,35043,47984,47989,47991,47995-47996,48010
set PORT_TCP_OUT=7,9,7777,35043,47984,47989,47991,47995-47996,48010
set PORT_UDP_IN=7,9,7777,47989,47992,47998-48000,48010
set PORT_UDP_OUT=7,9,7777,47989,47992,47998-48000,48010
set RULE_NAME_TCP_IN="_Open GameStream Ports - TCP in"
set RULE_NAME_TCP_OUT="_Open GameStream Ports - TCP out"
set RULE_NAME_UDP_IN="_Open GameStream Ports - UDP in"
set RULE_NAME_UDP_OUT="_Open GameStream Ports - UDP out"

netsh advfirewall firewall show rule name=%RULE_NAME_TCP_IN% >nul
if not ERRORLEVEL 1 (
    rem Rule %RULE_NAME_TCP_IN% already exists.
    echo Hey, you already got a rule by that name, you cannot put another one in!
) else (
    echo Rule %RULE_NAME_TCP_IN% does not exist. Creating...
    netsh advfirewall firewall add rule name=%RULE_NAME_TCP_IN% dir=in action=ALLOW protocol=TCP localport=%PORT_TCP_IN% description=%RULE_NAME_TCP_IN%_%PORT_TCP_IN%
)

netsh advfirewall firewall show rule name=%RULE_NAME_TCP_OUT% >nul
if not ERRORLEVEL 1 (
    rem Rule %RULE_NAME_TCP_OUT% already exists.
    echo Hey, you already got a rule by that name, you cannot put another one in!
) else (
    echo Rule %RULE_NAME_TCP_OUT% does not exist. Creating...
    netsh advfirewall firewall add rule name=%RULE_NAME_TCP_OUT% dir=out action=ALLOW protocol=TCP localport=%PORT_TCP_OUT% description=%RULE_NAME_TCP_OUT%_%PORT_TCP_OUT%
)

netsh advfirewall firewall show rule name=%RULE_NAME_UDP_IN% >nul
if not ERRORLEVEL 1 (
    rem Rule %RULE_NAME_UDP_IN% already exists.
    echo Hey, you already got a rule by that name, you cannot put another one in!
) else (
    echo Rule %RULE_NAME_UDP_IN% does not exist. Creating...
    netsh advfirewall firewall add rule name=%RULE_NAME_UDP_IN% dir=in action=ALLOW protocol=UDP localport=%PORT_UDP_IN% description=%RULE_NAME_UDP_IN%_%PORT_UDP_IN%
)

netsh advfirewall firewall show rule name=%RULE_NAME_UDP_OUT% >nul
if not ERRORLEVEL 1 (
    rem Rule %RULE_NAME_UDP_OUT% already exists.
    echo Hey, you already got a rule by that name, you cannot put another one in!
) else (
    echo Rule %RULE_NAME_UDP_OUT% does not exist. Creating...
    netsh advfirewall firewall add rule name=%RULE_NAME_UDP_OUT% dir=out action=ALLOW protocol=UDP localport=%PORT_UDP_OUT% description=%RULE_NAME_UDP_OUT%_%PORT_UDP_OUT%
)
exit
```

# Reporting Issues:

If you still got problems after troubleshooting please give us all the info from your troubleshooting, and your PC(s) specs.   
