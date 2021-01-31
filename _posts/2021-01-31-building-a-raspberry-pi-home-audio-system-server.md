---
published: false
categories:
  - Raspberry Pi
tags:
  - Raspberry Pi
  - HiFiBerry
  - multiroom
  - Mopidy
  - Snapcast
  - Pulseaudio
---
## Raspberry Pi home audio system - the kitchen and bedroom

In this first part of this series, I detailed my existing install, and my requirements for the upgraded system.

In this part we will set up our first two rooms with support for both Spotify and TuneIn web radio, in single room, and multiroom modes.


### Initial audio setup

Both the rooms already have a pair of passive speakers, with audio cabling to my central hub, so all i really need to do is provide an amplified audio signal to each set of speakers.

Additionally rooms also have touchscreen control units, but initially I am fine with using a mobile phone or tablet to control the audio.  I will look into adding a custom control unit in a future part.

#### Raspberry Pi Hardware

To enable multiroom audio, along with individual room customization, we will need one Pi to act as the 'server' for the multiroom system, and one Pi for each set of audio outputs i.e. one per room we want music in.


- [Raspberry Pi 4b 2gb](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/) - central server.
  - I have also attached a [full](https://thepihut.com/products/xl-raspberry-pi-4-heatsink) [set](https://thepihut.com/products/4-piece-raspberry-pi-4-heatsink-set) of heatsinks, and have housed this in the official [Raspberry Pi case](https://thepihut.com/products/raspberry-pi-4-case), plus fan.
  - [Raspberry Pi power supply](https://thepihut.com/products/raspberry-pi-psu-uk).
- 2 x Raspberry Pi 4b 2gb and [HiFiBerry Amp2](https://www.hifiberry.com/shop/boards/hifiberry-amp2/) to power the speakers.
  - The key hardware here is the Amp2, which despite its i size is a 60W class D stereo amplifier coupled with a [HiFiBerry DAC+](https://www.hifiberry.com/shop/boards/hifiberry-dacplus-rca-version/) Digital-to-analog audio Converter
  - This requires quite a bit more juice than the Raspberry Pi can output, so I am a 60W 18V 3.33A Meanwell, model [GST60A18-P1J](https://www.powersuppliesonline.co.uk/power-adapters/gst60a18-p1j-60w-18v-3-33a-power-adapter.html) (This is the latest model of the one HiFiBerry recommend)
  - I have also attached the same set heatsinks, and housed in a [HighPi case](https://www.highpi.net/), with a [fan](https://thepihut.com/products/miniature-5v-cooling-fan-for-raspberry-pi-and-other-computers) stuck on the outside for extra cooling.
- You will also need a suitable Micro SD card for each Pi.  I picked up this 32gb one from [Amazon](https://smile.amazon.co.uk/gp/product/B07RMXNLF4/).

{% include figure image_path="/assets/images/part-2/pi_hardware.jpg" alt="Raspberry Pi Hardware" caption="The Raspberry Pis." %}


## Setup the server

### OS setup
1. Write a copy of Raspberry PI OS Lite to your SD card.  Using the [Raspberry Pi Imager](https://www.raspberrypi.org/blog/raspberry-pi-imager-imaging-utility/) too makes this very easy.
1. Don't forget to enable SSH login by creating a file called `ssh` in the boot folder of the SD card.  See the [Raspberry Pi Documentation](https://www.raspberrypi.org/documentation/remote-access/ssh/) for more details on this.
1. (Optional) if you plan to connect to your Pi via WiFi, you must create a file called `wpa_supplicant.conf` in the boot folder with your WiFi connection details.  Again, see the [Raspberry Pi Documentation](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md) for more details on this.  Note, I am using the ethernet connection for my build.
1. Insert the SD card into the Pi, and power it up, and connect to it via SSH using [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).  The Pi should show up as `raspberrypi` on your network, and you can connect as username `pi` with password `raspberry`.
1. (Optional) Update and upgrade the software
```bash
sudo apt update
sudo apt upgrade -y
```
1. Use the raspi-config to update the hostname, default password and make a small performance tweak
```bash
sudo raspi-config
```
  - Password - Option 1 (System Options), then option S3 (Password).
  - Hostname - Option 1 (System Options), then option S4 (Hostname).  I called changed mine to 'home-server'.
  - Set GPU memory to 16mb; we wont be using the GPU - Option 4 (Performance Options), then option P2 (GPU memory), change to 16.
1. Restart the Raspberry Pi, and check you can connect with your new hostname and password
```bash
sudo shutdown -r now
```

### Snapcast
Snapcast is a multiroom client-server audio player, where all clients are time synchronized with the server to play perfectly synced audio.  It doesnt play music itself however.  
1. Install and configure the [Snapcast](https://github.com/badaix/snapcast) server by following the instructions for [Debian online](https://github.com/badaix/snapcast/blob/master/doc/install.md#debian).  Note, check for the latest package version on github.
```bash
wget https://github.com/badaix/snapcast/releases/download/v0.23.0/snapclient_0.23.0-1_armhf.deb
sudo dpkg -i snapclient_0.23.0-1_armhf.deb
sudo apt -f install
```
1. To use Spotify Connect, we need [librespot](https://github.com/librespot-org/librespot), and the easiest way to do this on Raspberry Pi OS is install [Raspotify](https://github.com/dtcooper/raspotify), and then disable it.
```bash
curl -sL https://dtcooper.github.io/raspotify/install.sh | sh
sudo systemctl disable raspotify
```
1. Configure the snapserver to enable Spotify Connect, and setup for Mopidy
```bash
sudo nano /etc/snapserver.conf
```
```editor-config
# rename the default fifi pipe from 'default' to 'Mopidy'
source = pipe:///tmp/snapfifo?name=Mopidy
# enable Spotify Connect, at high bitrate
source = spotify://librespot?name=Spotify&bitrate=320&devicename=Multi%20Room
```
1.  Restart the snapserver service to pick up the new configuration
```bash
sudo service snapserver restart
# check it worked
sudo service snapserver status
```

### Mopidy
[Mopidy](https://mopidy.com/) is an extensible music server, which can play music from local disk, Spotify, SoundCloud, TuneIn, and more.
1. Install Mopidy by following the [online instructions for Debian](https://docs.mopidy.com/en/latest/installation/debian/)
```bash
wget -q -O - https://apt.mopidy.com/mopidy.gpg | sudo apt-key add -
sudo wget -q -O /etc/apt/sources.list.d/mopidy.list https://apt.mopidy.com/buster.list
sudo apt update
sudo apt install mopidy -y
```
1. Enable to Mopidy service so it will run automatically on boot.
```bash
sudo systemctl enable mopidy.service
```
1. Install python pip package manager, and the gstreamer 'bad' playback plugins.
```bash
sudo apt install python3-pip gstreamer1.0-plugins-bad -y
```
1. Install the [Iris](https://github.com/jaedb/iris) Mopidy web GUI.  See also the [online instructions](https://github.com/jaedb/Iris/wiki/Getting-started#installing).  Iris can also be used to configure snapcast, and can connect to Iris instances on other machines. 
```bash
sudo python3 -m pip install Mopidy-Iris
sudo sh -c 'echo "mopidy ALL=NOPASSWD: /usr/local/lib/python3.7/dist-packages/mopidy_iris/system.sh" >> /etc/sudoers'
```
1. Install the [TuneIn](https://tunein.com/) Mopidy plugin and additional playback libraries.  Alternatively, you can manually add the stream URLs via the Iris GUI.
```bash
sudo apt install mopidy-tunein -y
```
1. Configure Mopidy so its output is sent to snapserver
```bash
sudo nano /etc/mopidy/mopidy.conf
```
```editor-config
# turn on the http server, and ensure we can connect to it from other devices on our network.
[http]
enabled = true
hostname = 0.0.0.0
port = 6680
zeroconf = Mopidy HTTP server on $hostname
# ensure we are taken to Iris automatically
default_app = iris
# Ensure Mopidy audio output is sent to snapserver
[audio]
output = audioresample ! audioconvert ! audio/x-raw,rate=48000,channels=2,format=S16LE ! filesink location=/tmp/snapfifo
# Optional (but recommended) - filter TuneIn streams so you only see stations.
[tunein]
filter = station
```
1. Restart the Mopidy service to pick up the updated configuration
```bash
sudo service mopidy restart
# check it worked
sudo service mopidy status
```
1. Test the Iris interface in the browser on your PC/Mac by navigating to [http://home-server.local:6680/iris/](http://home-server.local:6680/iris/)





Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.
