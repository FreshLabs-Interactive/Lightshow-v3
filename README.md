# Lightshow 3

Lightshow 3 is designed by **FreshLabs Interactive**, building on the original Lightshow by Derf.

Lightshow is an Xbox 360 app for the Ring of Light (ROL) and the center power LED. You can test each LED, play shows from a library, build shows on the console in a frame-by-frame Studio, and set any show to play at boot with the included DashLaunch plugin.

Credit to landaire for [smc.h](https://github.com/landaire/LaunchCode/blob/master/LaunchCode/smc.h) and [smc.cpp](https://github.com/landaire/LaunchCode/blob/master/LaunchCode/smc.cpp), to free60.org for the SMC reference, and to Xenomega for the [plugin/symlink code](https://github.com/Xenomega/ArchLoader/tree/master).

<img src="https://consolemods.org/wiki/images/5/58/Lightshow_Picture.png" width="49%" height="auto">

## What's new in v3

* **New UI**: five tabs (LB / RB), a live on-screen Ring of Light that mirrors the console, and context button hints on every screen.
* **Studio**: an on-console editor with a timeline, 15 effect generators, range selection, copy and paste, undo and redo, transforms (reverse, mirror, rotate, retime), show properties and save to `game:\Shows`.
* **Shows library**: every `.conf` on the console plus 12 built-in presets, with an animated preview. Play, loop, change speed, edit, delete, or set as the boot show in one press.
* **Audio sync**: add `@audio song.wav` and the lights follow the XAudio2 sample clock, so they never drift from the music.
* **Drift-free timing**: v2 slept after every LED write, so SMC latency built up over a song. The new engine schedules every step on a show clock, merges late steps into one write and skips writes that change nothing. In tests, a 4.0 s show takes 4.0 s, where v2 took 4.8 s.
* **No more frozen UI**: shows play on a worker thread. You can stop, pause or take over at any time.
* **System tab**: CPU, GPU, memory and board temperatures, SMC version, AV cable, orientation, tray state, an LED self-test, tray eject and "return ring to system".
* **Richer format**: compact frames, `REPEAT` blocks, `WAIT`, `RELEASE`, power LED changes on frames, `@loop`, `@end`, `@speed`, `@delay` and comments. 
* **Better boot plugin**: same engine and format, waits for slow drives, checks HDD, then USB0 and USB1, can hand the ring back when done, and can be switched off with a `boot.disabled` file.
* **Faster MIDI converter**: instant instead of real-time, no rounding drift, polyphony-aware, works with any MIDI type, and has more colour modes.
* **Lightshow Studio for PC** (`tools/LightshowStudio.html`): design shows with a mouse, import MIDI, preview in sync with an audio file, and export a `.conf`.

## Controls

| Tab | Controls |
|---|---|
| **Any screen** | **LB / RB** switch screens. Tap **START** for that screen's action; **hold START** for a second to exit to the dashboard. You can also exit from the last row of Settings. The **When you exit** setting decides whether the ring returns to the system or keeps its lights. |
| **Live** | Left stick aims at a quadrant (centred = whole ring). **A** green, **B** red, **Y** orange, **X** off. D-pad left / right rotates the pattern, up cycles colours, down clears. **Back** cycles the power LED. **LS** randomises. **RS** returns the ring to the system. **Start** plays `lightshow.conf`. |
| **Shows** | **A** play / stop, **X** edit in Studio, **Y** set as boot show, **Back** delete, **Start** loop on / off, **LT / RT** speed, **RS** pause. |
| **Studio** | Stick + **A / B / Y / X** paint the frame or the selection. D-pad left / right picks a frame (hold **RT** to move 10). Up / down changes the length by 10 ms (hold **RT** for 100 ms). The right stick scrubs. **Back** sets the power LED. **LS** starts or clears a selection. **RS** plays from the cursor (**RT + RS** plays from the start). Hold **LT** for edit mode: D-pad moves frames, up inserts, down deletes, **A** copy, **B** paste, **X** undo, **Y** redo. **Start** opens the menu. |
| **System** | **Start** runs the LED self-test, **A** opens or closes the tray, **X** releases the ring, **Y** releases the power LED, **Back** re-sends the LED state. |
| **Settings** | D-pad up / down chooses a setting and left / right changes it. Settings are saved to `game:\Lightshow.ini`. |

If all ROL LEDs are off and a controller is connected, the system turns the player's quadrant green. This is a feature of the system and can't be overridden by the app.

## Install

1. Copy the files/folders (`default.xex`, `Media\`, `lightshow-boot.xex`, and optionally `Shows\` and `lightshow.conf`) to `Hdd1:\Lightshow\`.
2. **Boot shows**: set `lightshow-boot.xex` as a DashLaunch plugin. It plays `Hdd1:\Lightshow\lightshow.conf`, falling back to `USB0:` and `USB1:` at the same path. **Set as boot show** in the app writes this file for you.
3. To pause boot shows without removing the plugin, create an empty `Lightshow\boot.disabled`.

## Aurora artwork

Aurora folder contains all cover art. Merge with existing Aurora folder.

## Making shows

* **On the console**: open the Studio tab, paint frames, and use **Start → Insert effect...** for spins, chases, sparkles and more. Save to `game:\Shows`.
* **On a PC**: open `tools/LightshowStudio.html` in a browser, then use **Export .conf** and copy the file to `game:\Shows`.
* **From MIDI**:
  ```
  pip install mido
  python lightshow_maker.py song.mid -o Shows/song.conf --audio song.wav
  python lightshow_maker.py song.mid --lanes tracks --mode velocity --min-ms 30
  ```
  Up to four channels (or tracks) each drive one quadrant, clockwise from top-left. Export the song as a 16-bit PCM WAV, put it next to the `.conf`, and the app plays the two in sync.

Click either image below to see demos of the original app:

[![PS2 Boot Remix demo](https://img.youtube.com/vi/-H91NT_gmmU/0.jpg)](https://www.youtube.com/watch?v=-H91NT_gmmU "Xbox 360 Lightshow Demo :: PS2 Boot Remix")
[![Sandstorm demo](https://img.youtube.com/vi/3WJwQjNUcpw/0.jpg)](https://www.youtube.com/watch?v=3WJwQjNUcpw "Xbox 360 Lightshow Demo :: Darude - Sandstorm")



