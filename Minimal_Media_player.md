# Minimal Media Player
In the past I used to burn media to DVD for my grandfather to watch. He was acquainted with the DVD player so this made sense, however the process of converting and burning all media to DVD was a hassle, not to mention the large reduction in quality that comes from the outdated DVD format.

The obvious solution was to use the built in media player in the TV, but this was quickly ruled out as this required way to many user inputs to navigate different menus and inputs, not something this user is capable of. Because I could not find any existing media players that would require less user interaction, I decided to make my own.

The starting point was a [Raspberry Pi 2 Model B](https://www.raspberrypi.org/products/raspberry-pi-2-model-b/), but any SBC with an HDMI port, USB port and GPIO for button inputs would do the job. The goal of this media player was minimal interaction needed for playback of media on USB memory sticks, as such there are only a couple things accessible to the user:
* Power on/off button.
* Large LED indicating power state.
* Pause button for (un)pausing during playback.
* Next button for jumping to the next file in the playlist (Usually the next episode).
* Single USB port for inserting memory sticks with media (in custom format)

To implement these features I started with the latest [Raspbian Desktop](https://www.raspberrypi.org/downloads/raspbian/) image. From here I tweaked the user interface to hide all elements and created a script that would run on startup. This script scans for mounted disks containing a `playlist.m3u` file, this file is then opened with [VLC Media Player](https://www.videolan.org/) in fullscreen using some command line parameters. When the playlist is no longer available, all VLC processes are killed to stop playback. The result is that when the Pi boots and a memory stick with a playlist is present or inserted it will automatically start playing according to the playlist file. When the stick is removed playback will stop.

For the buttons a Python script was made to convert the IO into a shutdown command or keypresses for the media controls. These keys were then set as hotkeys in the VLC configuration to control media playback. The LED was connected to the power supply to indicate the power status.

Now, opposed to burning DVDs, I can put the media for my grandfather on a USB memory stick and create a playlist file for the player to read. He can then insert the stick in the player to start playback and skip to the next episode if needed. I did end up running most media files through Handbrake once to lower file size and burn in the relevant subtitles.

## Experiences
This device has been received very well by the end user and !

## Parts used
Hardware:
* Raspberry Pi 2 Model B
* Generic microSD card
* Custom case with front USB port, power LED, and 3 large momentary push buttons
* Generic USB memory sticks for storing media

Software:
* Raspbian Desktop
* VLC Media Player
