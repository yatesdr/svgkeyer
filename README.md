# SVG Keyer
A simple utility class for keying RGBA SVG files into the HDMI output for a raspberry pi.

## What is it used for?
It's used for adding live SVG graphics to video when using a hardware video switcher that supports chroma-key overlays.   This is used in my other application (bugger) which is probably more useful, but if you're building an application for a Raspberry Pi that needs a simple HDMI output of a keyed SVG, this will get you started.

In generaly, you'd write an API or other class to interface with some control surface or software, and use this to command the raspberry pi of what SVG you want to output.   For a scoreboard, you'd load a template, replace what you need, and pass the file in to be displayed.

## Why does it exist?
I wanted a scoreboard overlay and didn't want to subscribe to some service.

## How does it work?
It uses a Raspberry Pi 4 (or other linux computer) to load an SVG file, chroma-key the alpha channel, then display it on the output.   You can then use this output display to feed into a hardware keyer such as an Atem Mini or Roland and use it for real-time streaming video overlays.

## Applications
1. Scoreboards or statistics
2. Lower thirds
3. Video / news clip overlays
4. Anything else you want to overlay that can be rendered in an SVG document.


# Installing
This was tested on a raspberry pi 4 running Raspbian x64 Lite.   It theoretically works on other NUC or whatever as well, but hasn't been really tested.

## Set up your Pi
1. Flash the SD card using Raspberry Pi Imager.
2. Boot, login, apt update and all the normal setup.
3. Create your venv and install dependencies
```
   python3 -mvenv venv
   source venv/bin/activate
   pip3 install cairosvg dispmanx numpy
```

4.  Edit /boot/firmware/config.txt to use a dispmanx compatible framebuffer
```
sudo sed -i 's/^dtoverlay=vc4-kms-v3d/dtoverlay=vc4-fkms-v3d/' /boot/firmware/config.txt
```
5.  reboot, source your venv again, and run the test to see it work.   By default it renders a scoreboard template with a green chromakey.
```
source venv/bin/activate
git clone https://github.com/yatesdr/svgkeyer.git
cd svgkeyer
python3 svgkeyer.py
```


# How to use it
Usage is pretty simple.   Create a class instance, and tell it an SVG file to render.   The SVG should be the same size as your target display, and should be prepared separately.    You can use templates of whatever type you prefer, but template substitution must be done separately as it's not included in this class.

Example usage:
```
from svgkeyer import svgkeyer
myoutput = svgkeyer()
myoutput.show("path/to/svg/file.svg")
```

This will output your SVG on the primary display in 1920x1080p resolution.   If there's an alpha channel in the SVG, it will key it for greenscreen style chroma-keying.

Optionally, you can use blue screen keying (or any other color) by setting the chromakey property:
```
myoutput.chromakey=(255,0,0)  # use a red keyer
myoutput.chromakey=(0,0,255) # use a blue keyer
myoutput.chromakey=(0,255,0)  # use a green keyer
```


# I like this and want a more fully developed application
I'm currently developing a more full fledged application that uses this library as the output, it will be released soon.   Watch for "bugger" in my repo at a future date if it's not already there.




   
   
