---
published: false
---
## Multi-room home audio upgrade

I recently moved into a house with a cool, but obsolete (and broken) SystemLine multi-room home audio system.  The system is non-functioning - there is a suspicious high pitched squeel from the power supply - and an upgrade would likely run into 4 figures.  So my plan is to replace all the electronics with something custom and mostly Raspberry Pi powered instead.

### Current system

#### Zones:
- Master bedroom: 2 passive wall mounted speakers, wall mounted touchscreen control unit, wall socket RCA sound input/output unit.
- Bathroom: ceiling  mounted 'active' speaker unit.
- Living room: 4 passive speaker plug sockets ('banana plugs') wired in series for left and right, touchscreen control pad and wall socket RCA.
- Kitchen: same as master bedroom.
- Garden: speaker cables - i've only found one so far, with a small control pad just inside the door.

#### Audio and control hardware:
- 2 Harman Kardon TU-970 DAB digital radio receviers with IR controllers attached.
- 1 Harman Kardon DVD player (I think used for playing CDS) with IR controller attached.
- 1 Old school iPod dock in the Kitchen
- 4 SystemLine 'zone amplifers', one for each passive zone.
- 2 SystemLine control unit things.

### The plan

At present, in each zone, everything is wired to a central hub location via standard audio cable for passive speakers, or cat5 ethernet cable.  This means I can remove all the obsolete hardware, but reuse the existing wiring and speakers to get a fully wired multi-room system.

#### General requirements:
- Each zone must be able to play music from, in order of importance
  1. Spotify.
  1. Internet Radio (e.g. via TuneIn or saved playlist).
  1. Phone via Bluetooth.
  1. FLAC files on network.
  1. Podcasts.
  1. Chromecast for audio
- Must be easy to control
  - For example, for Spotify, it must support [Spotify connect](https://www.spotify.com/us/connect/), and 'just work'.
  - No complex menu hierarchies.
  - Note, I can live with the multi-room aspect being a bit more complex to set up, as this wont be required every day.
- The system must be stable.
- The system must not sound crappy.

