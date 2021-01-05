# audio2image

Convert audio files to image files. The resulting files can be played using `aplay(1)` or similar.

Supported input formats are anything supported by ffmpeg.

Supported output formats are PPM,PGM,PBM.

Example:

    $ audio2image -r 8000 -c 1 audio.mp3 > audio.ppm
    $ aplay audio.ppm

It works roughly like so:

* transcode the input audio into raw PCM using ffmpeg to the desired sample rate and number of channels (usually 8000 Hz and 1 channel)
* work out the image dimensions required to contain the PCM
* write the image header
* write the PCM bytes
* write padding 0x80 bytes until image size is filled

James Stanley <james@incoherency.co.uk>
