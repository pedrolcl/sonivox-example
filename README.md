# sonivox-example
sonivoxrender utility, and example of sonivox library

[![Build on Linux with Submodule](https://github.com/pedrolcl/sonivox-example/actions/workflows/linux-subm.yml/badge.svg)](https://github.com/pedrolcl/sonivox-example/actions/workflows/linux-subm.yml)  
[![Build on Linux with Artifact](https://github.com/pedrolcl/sonivox-example/actions/workflows/linux-find.yml/badge.svg)](https://github.com/pedrolcl/sonivox-example/actions/workflows/linux-find.yml)  

This is a simple command line utility to render standard MIDI files into raw PCM audio streams. This utility can be compiled after building and installing sonivox in some prefix like `/usr`, `/usr/local`, or `$HOME/Sonivox`.
The CMake script contains three alternatives: using CMake only, using `pkg-config` and using sonivox as a subdirectory.

Once compiled, you can use the program to listen MIDI files or to create MP3 files. These are the available command line options:

~~~
$ ./sonivoxrender -h
Usage: ./sonivoxrender [-h|--help] [-v|--version] [-d|--dls soundfont] [-r|--reverb 0..4] [-w|--wet 0..32767] [-n|--dry 0..32767] [-c|--chorus 0..4] [-l|--level 0..32767] [-g|--gain 0..196] [-V|--Verbosity 0..5] [-R|--reverb-post-mix] [-C|--chorus-post-mix] file.mid ...
Render standard MIDI files into raw PCM audio.
Options:
        -h, --help              this help message.
        -v, --version           sonivox version.
        -d, --dls soundfont     DLS or SF2 soundfont.
        -r, --reverb n          reverb preset: 0=no, 1=large hall, 2=hall, 3=chamber, 4=room.
        -w, --wet n             reverb wet: 0..32767.
        -n, --dry n             reverb dry: 0..32767.
        -c, --chorus n          chorus preset: 0=no, 1..4=presets.
        -l, --level n           chorus level: 0..32767.
        -g, --gain n            master gain: 0..196. 100 = +0dB.
        -V, --Verbosity n       Verbosity: 0=no, 1=fatals, 2=errors, 3=warnings, 4=infos, 5=details
        -R, --reverb-post-mix   ignore CC91 reverb send.
        -C, --chorus-post-mix   ignore CC93 chorus send.
~~~

The following examples assume the default option USE_44KHZ=ON:

Example 1: Render a MIDI file and save the rendered audio as a raw audio file (PCM format: little endian signed 16 bits samples, 2 channels, sample rate = 44100 Hz)

    $ sonivoxrender ants.mid > ants.pcm

Example 2: pipe the rendered audio thru the Linux ALSA 'aplay' utility:

    $ sonivoxrender ants.mid | aplay -c 2 -f S16_LE -r 44100

equivalent to:

    $ sonivoxrender ants.mid | aplay -f cd

Example 3: pipe the rendered audio thru the ['lame'](https://lame.sourceforge.io) utility creating a MP3 file:

    $ sonivoxrender ants.mid | lame -r -s 44100 - ants.mp3
    
Example 4: pipe the rendered audio thru the ['sox'](https://sourceforge.net/projects/sox/) utility creating a WAV file:

    $ sonivoxrender ants.mid | sox -t s16 -c 2 -r 44100 - ants.wav

Example 5: pipe the rendered audio thru the PulseAudio's 'pacat' utility:

    $ sonivoxrender ants.mid | pacat

Example 6: pipe the rendered audio thru the PipeWire's 'pw-play' utility:

    $ sonivoxrender ants.mid | pw-play --rate 44100 -

Example 7: pipe the rendered audio thru the [FFmpeg](https://ffmpeg.org/)'s 'ffplay' utility:

    $ sonivoxrender ants.mid | ffplay -i - -f s16le -ar 44.1k -ac 2 -nodisp -autoexit -loglevel quiet

This has the advantage of being multiplatform. Depending on the FFmpeg installed version, you may need instead:

    $ sonivoxrender ants.mid | ffplay -i - -f s16le -ar 44.1k -ch_layout stereo -nodisp -autoexit -loglevel quiet

Example 8: pipe the rendered audio thru the ['mpv' media player](https://mpv.io/):

    $ sonivoxrender ants.mid | mpv --demuxer=rawaudio -demuxer-rawaudio-format=s16le --demuxer-rawaudio-rate=44100 --demuxer-rawaudio-channels=2 --no-video -

Besides being multiplatform, this supports progress view and better navigation (backed by in-memory cache).

You may replace "ants.mid" by another MIDI or XMF file, like "test/res/testmxmf.mxmf"

## License

Copyright (c) 2022-2024 Pedro López-Cabanillas.

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
