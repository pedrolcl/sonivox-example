# sonivox-example
sonivoxrender utility, and example of sonivox library

[![Build on Linux with Submodule](https://github.com/pedrolcl/sonivox-example/actions/workflows/linux-subm.yml/badge.svg)](https://github.com/pedrolcl/sonivox-example/actions/workflows/linux-subm.yml)  
[![Build on Linux with Artifact](https://github.com/pedrolcl/sonivox-example/actions/workflows/linux-find.yml/badge.svg)](https://github.com/pedrolcl/sonivox-example/actions/workflows/linux-find.yml)  

This is a simple command line utility to render standard MIDI files into raw PCM audio streams. This utility can be compiled after building and installing sonivox in some prefix like `/usr`, `/usr/local`, or `$HOME/Sonivox`.
The CMake script contains three alternatives: using CMake only, using `pkg-config` and using sonivox as a subdirectory.

Once compiled, you can use the program to listen MIDI files or to create MP3 files. These are the available command line options:

~~~
$ ./sonivoxrender -h
Usage: ./sonivoxrender [-h] [-d file.dls] [-r 0..4] [-w 0..32765] file.mid ...
Render standard MIDI files into raw PCM audio.
Options:
    -h              this help message.
    -d file.dls     DLS soundfont.
    -r n            reverb preset: 0=no, 1=large hall, 2=hall, 3=chamber, 4=room.
    -w n            reverb wet: 0..32765.
~~~

Example 1: Render a MIDI file and save the rendered audio as a raw audio file:

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

## License

Copyright (c) 2022-2024 Pedro López-Cabanillas.

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
