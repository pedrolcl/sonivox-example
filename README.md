# sonivox-example
sonivoxrender utility, and example of sonivox library

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

    $ sonivoxrender ants.mid | aplay -c 2 -f S16_LE -r 22050

Example 3: pipe the rendered audio thru the 'lame' utility creating a MP3 file:

    $ sonivoxrender ants.mid | lame -r -s 22050 - ants.mp3
