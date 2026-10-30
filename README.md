# CamelSpace-Patched
Prepare to be entranced! CamelSpace can take any sound you throw at it, be it
a pad, a synth line or a drum loop, and generate a dizzying array of
dynamically-evolving, gated rhythmic textures.

Camel Audio CamelSpace 1.50.1.609

An enormously powerful 128-step 'trance gate' sequencer controls panning,
filter cut-off and volume, while independent LFOs provide still more
mind-boggling modulation options.

A tempo-synced stereo delay and powerful auto-panner ensure that your sounds
are always on the move, while an enhancer (with 'Xcita' and soft saturation),
a flanger and a high-quality reverb all add 'sparkle'.

A stunning graphical user interface puts all the controls at your fingertips,
while the X/Y pad allows easy real-time manipulation of the most important
parameters.

With the intelligent 'Randomize' button, new sounds and inspiration are just a
click away. Over 500 attention-grabbing presets, organised by category, are
also included. Find out more in the easy-to-read manual.


Features
--------
- Rhythmic multi-effect, to add dynamic interest to pads, synths, drums and more.
- Powerful 128-step 'trance gate' controlling filter cut-off, pan and volume.
- Auto-panner, enhancer, flanger, multi-mode filter, stereo delay, reverb.
- Easy-to-use, with X/Y pad and intelligent Randomize.
- 500+ categorised presets.


New Features in v1.5
--------------------
- 64-bit support
- New filter modes from Alchemy
- Better preset organisation by category with 164+ new presets
- MIDI program change
- Inverse LFO shapes for Sine, Square, Triangle
- Improved pattern and step tie editing
- Randomisation option for step sequencer
- Randomise button affects all active patterns
- Default delay filter cutoff now 100% open


What's in this folder
---------------------
- Install CamelSpace.pkg    Installs everything below in one go
- CamelSpace.component      Audio Unit (Logic, GarageBand and other AU hosts)
- CamelSpace.vst            VST version for VST hosts
- CamelSpaceData            Presets and the graphics for the interface
- CamelSpaceManual.pdf      The manual

Both plug-ins work in 64-bit and 32-bit hosts.


Installing
----------
The easy way: double-click "Install CamelSpace.pkg" and follow the steps.

The first time, macOS may say it can't open the installer. If so, open
System Settings > Privacy & Security, scroll down, click "Open Anyway", and
run it again.

Prefer to do it by hand? Copy these three items into place:

  CamelSpace.component  ->  /Library/Audio/Plug-Ins/Components/
  CamelSpace.vst        ->  /Library/Audio/Plug-Ins/VST/
  CamelSpaceData        ->  /Library/Application Support/Camel Audio/

(Use the main Library folder on your hard drive, not the one in your home
folder. Create the "Camel Audio" folder if it isn't there.)

Then open Terminal and paste these lines so macOS will load the plug-ins:

  sudo xattr -cr /Library/Audio/Plug-Ins/Components/CamelSpace.component /Library/Audio/Plug-Ins/VST/CamelSpace.vst
  sudo codesign --force --deep -s - /Library/Audio/Plug-Ins/Components/CamelSpace.component
  sudo codesign --force --deep -s - /Library/Audio/Plug-Ins/VST/CamelSpace.vst
  killall -9 AudioComponentRegistrar


Using it in Logic Pro
---------------------
Open Logic > Settings > Plug-in Manager, select CamelSpace and click
"Reset & Rescan Selection". It will then appear under Audio Units >
Camel Audio.

On Apple Silicon Macs (M1 and later), Logic runs CamelSpace through Rosetta
automatically. Other DAWs may need "Open using Rosetta" turned on in the
app's Get Info window.

This is a personal-use build of a commercial Camel Audio product. Please
don't share it publicly.


Why this version works on modern macOS
--------------------------------------
On macOS 10.14 Mojave and later, the original CamelSpace loads and makes
sound, but its window is blank.

The cause is in the plug-in's graphics library (VSTGUI 3.6). Whenever the
window needs drawing, its drawing routine first calls "lockFocus" to grab a
drawing surface, draws, then calls "unlockFocus". On older macOS that was
harmless. Since Mojave, plug-in windows are drawn through Core Animation
layers, and calling lockFocus there swaps in a separate, off-screen surface.
Everything gets drawn, but never reaches the screen.

The fix removes those two calls, in both the 64-bit and 32-bit versions,
by replacing each with a "do nothing" instruction (a NOP) of the same size.
The plug-in then draws straight onto the surface macOS already prepared, just
as it did on older systems. Nothing else is changed.

  64-bit:  lockFocus at 0x744bd, unlockFocus at 0x745ab
           ff 15 xx xx xx xx  ->  66 0f 1f 44 00 00
  32-bit:  lockFocus at 0x84fda, unlockFocus at 0x850f3
           e8 xx xx xx xx     ->  0f 1f 44 00 00

Because this changes the plug-in, its original code signature no longer
matches. That's why the installer (or the Terminal steps above) re-signs it.
