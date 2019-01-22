# Wear OS Instructions

## Debug over Bluetooth
Bluetooth debugging only works for Android-paired watches. Before you begin, be sure the watch and phone are paired and you've enabled developer options on the watch. You should also be sure that developer options is also enabled on the phone. To check, open the phone's Settings menu, select About phone and click the build number seven times.

### Enable USB debugging on the phone
1. Open the phone's Settings menu.
2. Select Developer Options and enable USB debugging.
3. Enable ADB/Bluetooth debugging on the watch
4. Open the watch's Settings menu.
5. Scroll to Developer Options.
6. Confirm that ADB debugging is enabled.

### Enable Debug over Bluetooth.
1. To enable Bluetooth debugging on the phone, open the Wear companion app.
2. Scroll down to Advanced Settings and tap to view the Advanced Settings options.
3. Enable Debugging over Bluetooth. 

A status message appears under the option. It looks like this:
Host: disconnected
Target: connected

At this point the development machine (the host) is not communicating with with the watch (the target). You need to complete the link.

## Connect the debugger to the watch
In this final step, you'll use everything: the debugger, the phone, and the watch.
<ol>
  <li>Connect the phone to your development machine with a USB cable.</li>
  <li>Run these two commands in Android Studio terminal:</br>
    <code>adb forward tcp:4444 localabstract:/adb-hub</code></br>
    <code>adb connect 127.0.0.1:4444</code></br>
    Note: You must use the IP address 127.0.0.1. You can use any available port on your development machine. Be sure you use the same port in both commands. (In this example the port is 4444.)
  </li>
  <li>After you type the connect command, look at the watch. It will ask you to confirm that you are allowing ADB Debugging.</li>
  <li>Go back to the phone and check the status display in the Wear companion app. It should look like this:</br>
    <code>Host: connected</code></br>
    <code>Target: connected</code></br>
    The watch is now connected to the debugger and you're ready to start debugging.
  </li>
</ol>

When you debug a watch using Bluetooth, <code>adb</code> always uses the IP address <code>127.0.0.1</code> plus the port that you assigned. Therefore, all adb commands use this format (continuing the example, the port is 4444):

## Use screenrecord for Wear
You can record a video from your Wear device using the following steps:

<ol>
<li>Record raw frames on the watch:</br>
<code>adb shell screenrecord --time-limit 30 --output-format raw-frames --verbose /sdcard/video.raw</code></br>
<li>Copy the raw file to your development machine:</br>
<code>adb pull /sdcard/video.raw video.raw</code></br></li>
<li>Use ffmpeg to convert the raw file to MP4:</br>
<code>ffmpeg -f rawvideo -vcodec rawvideo -s 400x400 -pix_fmt rgb24 -r 10 -i video.raw -an -c:v libx264 -pix_fmt yuv420p video.mp4</code></li>
