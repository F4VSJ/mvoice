# M17 Digital Voice

*M17 Digital Voice* , mvoice, is a fully functional, graphical repeater. It uses David Rowes Codec2 and operates as a complete M17 repeater, only there is no RF component. It can Link to M17 reflectors and it can also do *routing*! It works best with a USB-based headset with microphone. mvoice uses the default pulseaudio/ALSA input and output device, so for most versions of linux, all you need to do is plug your headset in and you should be ready to go.

## Building on a Raspberry Pi

Raspbian is not ready for *qdv* out of the box. While Raspbian has ALSA, it also needs pulseaudio:

```bash
sudo apt install -y pulseaudio pavucontrol paprefs
```

It's proabably a good idea to reboot after installing pulseaudio. After it's installed, take a look at the output of `aplay -L`. Near the beginning is should say:

```bash
default
    Playback/recording through the PulseAudio sound server
```

You might also need to go to the ALSA audio configuration. For Debian Buster, this is found in main menu under **Preferences-->Audio Device Settings**. Select your headset in the drop-down list of devices, and then configure it and set it to be the default device. Set your speaker and microphone gain somewhere near the top. Once you build and configure *qdv*, you can use the Echo feature to help you set these gains adjust the speaker volume of you headset for a comfortable level and adjust the mic gain for the loudest playback without clipping. For my setup, the playback (speaker) was near 100% and the mic gain was at about 50%.

## Building and installing

There are several library requirements before you start:

```bash
sudo apt install -y git build-essential libgtkmm-3.0-dev libasound2-dev libsqlite3-dev

```

Then you can download and build qdv:

```bash
git clone git://github.com/n7tae/mvoice.git
cd DigitalVoice
make
```

If it builds okay, then you can install it:

```bash
make install
```

All the configuration files are located in ~/etc and the mvoice executable is in ~/bin. Please note that symbolic links are installed, so you can do a `make clean` to get rid of the intermediate object files, but don't delete the build directory unless you want to remove *mvoice* from your system.

## Dashboard

DigitalVoice includes a web-based dashboard that uses the php mini-server. To start the mini-server:

```bash
sudo make installdash
```

This will automatically install packages needed for the mini-server. The mini-server can run in the background even if you are not using the *mvoice* program. If it is not serving a browser client, it doesn't use much resources.

Once *mvoice* is running, you can point your broswer to `http://localhost`, or `http://*hostname*.local` where *hostname* is, of course, the hostname where your running *mvoice*.

If you want to turn off the mini-server:

```bash
sudo make uninstalldash
```

## Configuring mvoice

Be sure to plug in your headset before starting *mvoice*. Once your ready, open a shell and type `bin/mvoice`. Most log messages will be displayed within the log window of qdv, but a few messages, especially if there are problems, may also be printed in the shell you launch qdv from.

Once it launches, click the **Settings** button and make sure to set your callsign and the codec setting on the M17 page. You can usually leave the audio settings on "default". Also enable IPv6 if you internet provider supports it. Click the Okay button and your settings will be saved in your ~/etc/ directory.

On the main window, set your destination callsign and IP address. Once you've entered valid values for both, you can save these for future use.

For Linking, you can select a reflector or to automatically link when the linking thread starts. The link button will only be activated after you have entered a valid target in the destination callsign and IP address. This validation **does not** check to see if the module you are requesting is actually configured and operational.

## Operating

*mvoice* can operate in linking or routing mode. Use the check boxes on the Settings dialog to enable or disable these modes. When you disable a mode, the thread operating the that mode will be shut down.

There are three "transmitting" buttons on *mvoice*:

1) **Echo Test** is a toggle switch. Click it to start recording an echo test and your voice will be encoded using the Codec2 mode you selected in the Settings dialog. Note that the button will turn red when you are recording and listening to your echo. Click the button again and *mvoice* will decode the saved AMBE data and play it back to you. This is a great way to check the operation of you setup as well as the volume levels and make sure your headset is working properly. Note that *mvoice* has no volume or gain controls for either TX or RX. You'll use your ALSA interface for this.

2) **PTT** the large PTT button is also toggle switch. Click it and it will turn red also and *qdv* will encode your audio and send it on a route or to your destination. Click the button again to return *mvoice* to receiving.

3) **Quick Key** is a single press button that will send a 200 millisecond transmission. This is useful when trying to get the attention of a Net Control Operator when you are participating in a Net. It could also be useful if you are trying to do a direct callsign route target if you are behind a firewall you can't configure.

de N7TAE (at) tearly (dot) net
