---
published: false
---
## Raspberry Pi home audio system - the kitchen and bedroom

In this first part of this series, I detailed my existing install, and my requirements for the upgraded system.

In this part we will set up our first two rooms with support for both Spotify and TuneIn web radio, in single room, and multi-room modes.


### Initial audio setup

Both these rooms already have a pair of passive speakers, with audio cabling to my central hub, so all i really need to do is provide an amplified audio signal to each set of speakers.

Both rooms also have touchscreen control units, but initally I am fine with using a mobile phone or tablet to control the audio.  I will look into adding a custom control unit in a future part.

To enable multi-room audio, along with individual room customization, we will need one Pi to act as the 'server' for the multi-room system, and one Pi for each set of audio outputs i.e. one per room we want music in.

### Raspberry Pi Hardware
- Raspberry Pi 4b - central server.
  - I have also attached a full set of heatsinks, and have housed this in the offical Raspberry Pi case, plus fan.
  - However, this doesnt need any Pi-specific hardware, so will likely be replaced by a docker container running an a NAS, once I get around to upgrading my NAS.
- 2 x Raspberry Pi 4b and HiFiBerry Amp2 to power the speakers.
  - The key hardware here is the HiFiBerry Amp2, which despite its diminutaive size is a 60W class D stereo  amplifier coupled with a HiFiBerry DAC+ Digital-to-analog audio Converter
  - This requires quite a bit more juice than the Raspberry Pi can output, so I am a 60W 18V 3.33A Meanwell power brick (which is the upgraded model to the one HiFiBerry recomend themseleves).
  - I have also attached a full set of heatsinks, and housed in a HighPi case, with a fan stuck on the outside for extra cooling.







#### OS
I initially tried Volumio which is an OS for Raspberry Pi decidated audio playback, however I couldn't get the Spotify support to work, so i setted on Raspberry PI OS Lite, with extra software installed on top:

- Raspotify - A Spotify Connect client for the Raspberry Pi that Just Works (And it does!).  This wrapps the librespot library for Raspberry PI OS.
- Mopody - General purpose audio player.  I will use this for any audio source apart from Spotify Connect.
  - Iris Web GUI
  - TuneIn plugin
- Snapcast - We will use to enable multi-room audio.
- Pulseaudio - This is used to enable more than one process to use the soundcard at once, and later down the line, may be useful for bluetooth connectivity.


Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.
