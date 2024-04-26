# SVG Keyer
A simple utility class for keying RGBA SVG files into the HDMI output for a raspberry pi.

## What is it used for?
It's used for adding live SVG graphics to video when using a hardware video switcher that supports chroma-key overlays.

## Why does it exist?
I wanted a scoreboard overlay and didn't want to subscribe to some service.

## How does it work?
It uses a Raspberry Pi 4 (or other linux computer) to load an SVG file, chroma-key the alpha channel, then display it on the output.   You can then use this output display to feed into a hardware keyer such as an Atem Mini or Roland and use it for real-time streaming video overlays.

## Applications
1. Scoreboards or statistics
2. Lower thirds
3. Video / news clip overlays
4. Anything else you want to overlay


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



   
   