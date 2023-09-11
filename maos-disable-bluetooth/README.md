# Problem
 
https://lapcatsoftware.com/articles/bluetooth.html
 
### July 20 2022 by Jeff Johnson
 
On April 22, I filed a bug (FB9992639) with Apple's [Feedback Assistant](https://feedbackassistant.apple.com/feedback/9992639 "Bluetooth re-enabled after every iOS and macOS update") titled "Bluetooth re-enabled after every iOS and macOS update". I believe this issue started with iOS 14 and macOS 11, but in any case it definitely happens now with every iOS 15 and macOS 12 update, including today's iOS 15.6 and macOS 12.5 updates, on every device I own. (I think Apple stopped re-enabling Bluetooth for Big Sur security updates after Monterey was released.) I finally decided to file a bug with Apple after I upgraded from my old MacBook Pro running Big Sur to a [new MacBook Pro](https://lapcatsoftware.com/articles/impressions-mbp.html "Impressions of the new MacBook Pro") running Monterey, because I was filing several Monterey bugs at the same time. Here are the steps to reproduce from my bug report:
 
1. Disable Bluetooth in iOS Settings or macOS System Preferences.
2. Install an iOS or a macOS update.
3. Check iOS Settings or macOS System Preferences.
 
Expected results: Bluetooth is still disabled.
 
Actual results: Bluetooth is enabled.
 
_Note:_ There are [two ways to disable bluetooth on iOS](https://support.apple.com/HT208086 "Use Bluetooth and Wi-Fi in Control Center"), one temporary and one permanent. The temporary way is with the Bluetooth button in Control Center, which disables Bluetooth until you restart your device or 5am in the morning, whichever comes first. In contrast, my bug report is about the "permanent" way, which involves switching off Bluetooth in the Settings app on iOS or System Preferences on macOS (now System Settings, sigh).
 
Today I got a response from Apple to my bug report:
 
> Thank you for filing this feedback report. We reviewed your report and determined the behavior you experienced is currently functioning as intended.
>
> You can close this feedback by clicking on the "Close Feedback" link. Thank you.
 
Needless to say… wow. And to be clear, the above quote was the _entirety_ of Apple's response to my bug report. No explanation of why, just functioning as intended.
 
This is so incredibly user hostile. If you're wondering what's wrong with re-enabling Bluetooth — aside from the obvious: blatantly disregarding the user's preference — you can [Google](https://www.google.com/search?q=site%3Asupport.apple.com+%22About+the+security+content%22+bluetooth) a list of security vulnerabilities in Apple's Bluetooth stack. I personally don't use Bluetooth for anything and don't ever want it enabled on my devices.
 
## Solution

## Last working on 09/11/2023
## MacOS 13.5.2
 
To disable bluetooth at login:
 
Disable bluetooth command
https://apple.stackexchange.com/questions/365055/disable-bluetooth-or-disconnect-bluetooth-connections-when-lid-is-closed
 
```bash
brew install blueutil
echo "$(which blueutil) -p 0"
```
 
Full script:
 
```bash
#!/bin/bash
 
/usr/local/bin/blueutil -p 0
```
 
Run `chmod a+x` on that script too.
 
https://stackoverflow.com/questions/6442364/running-script-upon-login-in-mac-os-x
 
Create a plist script to run the shell.
 
Save as `~/Library/LaunchAgents/com.user.loginscript.plist`
 
 
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" http://www.apple.com/DTDs/PropertyList-1.0.dtd>
<plist version="1.0">
<dict>
   <key>Label</key>
   <string>com.user.loginscript</string>
   <key>ProgramArguments</key>
   <array><string>/Users/YOUR_USER_ID/Documents/disable_bluetooth.sh</string></array>
   <key>RunAtLoad</key>
   <true/>
</dict>
</plist>
```
 
MacOS auto-adds this to login items.
 
Log out and log in to verify it works.