---
published: false
---
## Multi-Room Home Audio Upgrade Part 2

In this first part of this series, I detailed the current system, and my requirements for the upgraded system.

In this part we will set up our first two rooms with support for Spotify and TuneIn web radio


### Initial audio setup

For my initial setup, I want to be able to play Spotify and Internet radio in my Kitchen and Bedroom either using different audio streams, or having them both in sync.

Both these rooms already have a pair of passive speakers, with audio cabling to my central hub.

### Hardware
- Each room already has a set of speakers, with cabling to the central hub.
- Raspberry Pi 4b to act as a central server for the multi-room audio.
  - This doesnt need any specific hardware, so will likely be replaced by a docker container running an a NAS, once I get around to upgrading my NAS.
  - I have also attached a full set of heatsinks, this is housed in the offical Raspberry Pi case, plus fan.
- 2 x Raspberry Pi 4b and HiFiBerry Amp2 to power the speakers.
  - The key hardware here is the HiFiBerry Amp2, which despite its diminutaive size is a 60W class D stereo  amplifier coupled with a HiFiBerry DAC+ Digital/Analge Converter
  - This requires quite a bit more juice than the Raspberry Pi can output, so I am a 60W 18V 3.33A Meanwell power brick (which is the upgraded model to the one HiFiBerry recomemnd themseleves)
  - I have also attached a full set of heatsinks, and housed in a HighPi case, with a fan stuck on the outside for extra cooling.

### Software

#### OS
I initially tried Volumio which is an OS for Raspberry Pi decidated audio playback, however I couldnt get the Spotify support to work, so i setted on Raspberry PI OS Lite.

#### Audio players
- Raspotify - A Spotify Connect client for the Raspberry Pi that Just Works (And it does!).  This wrapps the librespot library for Raspberry PI OS.
- Mopody - General purpose audio player.  I will use this for any audio source apart from Spotify Connect.
  - Iris Web GUI
- Snapcast - Not an audio player, but the software we will use to enable multi-room audio


Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.
