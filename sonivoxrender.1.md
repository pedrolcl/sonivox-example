% SONIVOXRENDER(1) sonivoxrender 0.0.0 | Sonivox MIDI File Renderer
% Pedro López-Cabanillas <plcl@users.sf.net>

# NAME

**sonivoxrender** — Render standard MIDI files into raw PCM audio

# SYNOPSIS

| **sonivoxrender** [**-h|-\-help**] [**-v|-\-version**] [**-d|-\-dls** _file.dls_] [**-r|-\-reverb** _0..4_] [**-w|-\-wet** _0..32767_] [**-n|-\-dry** _0..32767_] [**-c|-\-chorus** _0..4_] [**-l|-\-level** _0..32767_] [**-g|-\-gain** _0..100_]  _midi_file_

# DESCRIPTION

This program is a MIDI file renderer based on the sonivox synthesizer library.
It reads .MID (Standard MIDI Files) file format, and writes an audio stream to the standard output as raw 16 bit stereo PCM samples.

## Options

-h, -\-help

:   Prints brief usage information.

-v, -\-version

:   Prints the version numbers.

-d, -\-dls  _file.dls_

:   Optional DLS soundfont file name. If not provided, it uses an internal embedded soundfont.

-r, -\-reverb  _reverb_preset_

:   Reverb preset between 0 and 4: 0=no, 1=large hall, 2=hall, 3=chamber, 4=room.

-w, -\-wet  _reverb_wet_

:   Reverb wet level between 0 and 32767.

-n, -\-dry  _reverb_dry_

:   Reverb dry level between 0 and 32767.

-c, -\-chorus  _chorus_preset_

:   Chorus preset between 0 and 4: 0=no, 1..4=presets.

-l, -\-level _chorus_level_

:   Chorus level between 0 and 32767.

-g, -\-gain _master_gain_

:   Master gain between 0 and 100, default is 90 (10 dB below maximum).

## Arguments

_midi_file_

:   Input MID file name.

# EXAMPLES

The following examples assume the default option USE_44KHZ=ON, which means an output sample rate = 44100 Hz.

Example 1: Render a MIDI file and save the rendered audio as a raw audio file:

    $ sonivoxrender ants.mid > ants.pcm

Example 2: pipe the rendered audio thru the Linux ALSA **aplay** utility:

    $ sonivoxrender ants.mid | aplay -c 2 -f S16_LE -r 44100

is equivalent to:

    $ sonivoxrender ants.mid | aplay -f cd

Example 3: pipe the rendered audio thru the **lame** utility creating a MP3 file:

    $ sonivoxrender ants.mid | lame -r -s 44100 - ants.mp3

Example 4: pipe the rendered audio thru the **sox** utility creating a WAV file:

    $ sonivoxrender ants.mid | sox -t s16 -c 2 -r 44100 - ants.wav

Example 5: pipe the rendered audio thru the PulseAudio's **pacat** utility:

    $ sonivoxrender ants.mid | pacat

# BUGS

See Tickets at GitHub <https://github.com/pedrolcl/sonivox/issues/>

# LICENSE AND COPYRIGHT

Licensed under the Apache License, Version 2.0

Copyright (c) 2022-2024 Pedro López-Cabanillas and contributors
