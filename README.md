# audio2image

Convert audio files to image files. The resulting files can be played using `aplay(1)` or similar.

Supported input formats are anything supported by ffmpeg.

Supported output formats are BMP,PPM,PGM,PBM.

Example:

    $ audio2image -r 8000 -c 1 audio.mp3 > image.bmp
    $ aplay image.bmp

It works roughly like so:

* transcode the input audio into raw PCM using ffmpeg to the desired sample rate and number of channels (usually 8000 Hz and 1 channel)
* work out the image dimensions required to contain the PCM
* write the image header
* write the PCM bytes
* write padding 0x80 bytes until image size is filled

## Usage

    usage: audio2image [options] [AUDIOFILE] > [IMAGEFILE]

    This program will decode audio from the given audio file (in any format supported by ffmpeg)
    and write it out as an image file in NetPBM or BMP format, such that it can
    be played using aplay(1).

    Image options:

        -f,--format ppm|pgm|pbm|bmp
            Set the output image format.
            ppm: 24bpp colour
            pgm:  8bpp greyscale
            pbm:  1bpp black and white
            bmp: 24bpp colour
            Default: bmp

        -w,--width PIXELS
            Set the image width in pixels.
            If height is set but not width, then width will be calculated as:
                ceil(total_pixels / height)
            If neither height nor width are set, then both will be calculated as:
                ceil(sqrt(total_pixels))

        -h,--height PIXELS
            Set the image height in pixels.
            If width is set but not height, then height will be calculated as:
                ceil(total_pixels / width)
            If neither height nor width are set, then both will be calculated as:
                ceil(sqrt(total_pixels))

    Audio options:

        -r,--rate HZ
            Set the output sample rate in Hz. If necessary, the input audio will be
            resampled to match this. The resampling is quite primitive, for better
            results use better software.
            Default: 8000

        -c,--channels CHANNELS
            Set the number of output channels. Use 2 for stereo output. If necessary, the
            input channels will be combined or duplicated to match this.
            Default: 1

    Other options:

        -q,--quiet
            Don't warn if the specified width/height truncate the audio.
            Default: off

        -h,--help,--usage
            Show this text.

    Input is taken from a single filename specified on the command line.
    Output is given on stdout.

    Examples:

        $ audio2image -r 44100 -c 2 -w 800 -h 600 -f bmp example.wav > example.bmp

## Contact

audio2image is a program by James Stanley. You can email me at james@incoherency.co.uk or read my blog at https://incoherency.co.uk/
