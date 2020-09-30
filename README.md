# PoltergeistSimulator
Python script to turn a Raspberry Pi into a ghost that haunts your house. Plays creepy 
noises at random times and can flicker lights/other effects by controlling a relay through
the GPIO pins. It only requires the standard Python libraries included with Raspberry Pi OS
or Raspbian, so it should run right away on a fresh Pi setup.

This program assumes you're running it on a Raspberry Pi and will probably break immediately
if you run it on anything else (when it tries to import RPi.GPIO for example). There is a 
stripped-down version for running on a normal computer (platform agnostic, at least 
in theory) that only includes the audio component here: 
https://github.com/davidjouellette/PoltergeistSimulator_Desktop.


Work In Progress, testing not complete. 

RASPBERRY PI REQUIREMENTS (If you've used a Pi before you can skip this section):
-Raspberry Pi:     In theory any model with audio output and GPIO pins should work, this 
                   was tested on a model 3
-Pi necessities:   If you've never used the Raspberry Pi before, this can throw you. You'll
                   need a bare minimum of a USB keyboard (and probably mouse), a monitor or
                   TV with HDMI input, a 5V 2.5A micro-USB power adapter (phone chargers 
                   usually work fine), a micro SD card of about 16GB or larger, and a way to 
                   write files to that card (SD card reader). The SD card reader is the least
                   likely one for you to have hanging around. Check your laptops, sometimes 
                   they're built in. You can also try putting the card in a device like a 
                   camera or phone and connecting it via USB to your computer, but there
                   may be difficulties with that. Alternatively, you can buy an SD card 
                   that'll install the OS for you (these are called NOOBS cards, for New
                   Out Of the Box Software): https://www.adafruit.com/product/4266.

Note: Newer Pi models may have network/USB boot options that get you to the point where 
      you can use the Pi itself as a card reader, but I'm not familiar with that. Google
      around before you buy anything!

HARDWARE REQUIREMENTS:
-Powered Speaker:  The Pi doesn't have enough power to drive a decent-sized speaker through
                   the 3.5mm output so one with external power is likely to give better results.
-Electrical Relay: The Pi can't power much more than an LED from the GPIO, but it can send
                   3.3 volt control triggers to turn a relay on and off. For simplicity 
                   and safety I use a switchable power strip like this one:
                   https://www.amazon.com/gp/product/B00WV7GMA2/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1
                   but with a little electronic know-how the same effect can be achieved for 
                   lower cost.
-Wires/Connectors: You need a way to go from the GPIO pins to the relay at the very least.
                   You may need a few more components based on your project (a small 
                   prototyping "breadboard" would probably be helpful). I use male/female
                   jumper wires to keep things tidy: https://www.adafruit.com/product/826 
-Some appliance:   This plugs into your relay and can be randomly turned on and off. 
                   Make sure it's something that won't be harmed by rapid switching 
                   (or be careful not to include rapid switching patterns in 
                   GPIO_playlist.txt). Sky's the limit! Make one of your household lamps
                   flicker, hook it up to a Halloween decoration, maybe something that 
                   projects spooky images, rig up a servo to shake your bookshelf, connect
                   to a small fan pointed at some spooky wind chimes, go wild!

INITIAL SETUP:
There are plenety of tutorials out there on setting up the Pi itself. Once you get that done,
you'll need to get PoltergeistSimulator.py and the configuration files onto the SD card.
Because the Pi is so barebones, that's easier said than done. You can use an SD card reader to
add the files directly, download them through the Pi itself (note that not all models have wifi
built in), or transfer them from a USB drive. Finally, all you need to do to start the program is
open a terminal, navigate to the location of the program, and enter 'python PoltergeistSimulator.py'
without quotes.

CONFIGURING THE PROGRAM:
There are a lot of options you can set (possibly too many) in the configuration file
"ghost_config.txt". All you need to do is open it in a text editor and go to lines like
'variable = something' and change the 'something'. You can set the time between random events,
the way the relays trigger, all kinds of stuff. Please don't edit anything other than the right
side of those 'variable = something' equations because it will likely break the program when it
tries to read it. Yes, it's not a very robust text parser, this is just a hobby, sue me!

The program will make a playlist that includes all media files in the "sounds" folder that 
OXMPlayer can play. This means audio AND video, although video files are completely untested
at this point (see AUDIO OUTPUT below). The shuffling and repeating (or lack thereof) of the
playlist can be controlled by setting options in ghost_config.txt. More info on the options 
in ghost_config.txt can be found...in ghost_config.txt.

AUDIO OUTPUT:
PoltergeistSimulator on Raspberry Pi uses OMXPlayer, which is included with Raspberry Pi OS 
(formerly Raspbian). PoltergeistSimulator assumes you're using an external speaker through 
the 3.5mm port to play sounds, so it calls OMXPlayer with default options (which in turn 
defaults to playing sounds through the analog port). OMXPlayer will also play video clips,
and will default to playing them through the HDMI port, but the audio for the clip will still
default to the analog output. So if you want to use video instead of audio, you may need to 
modify the call to OMXPlayer in PoltergeistSimulator slightly to play the audio through
HDMI as well. Check the OMXPlayer documentation for how to do that: 
https://www.raspberrypi.org/documentation/raspbian/applications/omxplayer.md

Sometimes I would get a background hum on my speaker. I was using a small battery-powered one
and it would hum while plugged in for charging. I think speaker interference is a common problem
with the Pi so I direct you to Google which can probably provide better answers than I.

SETTING UP THE RELAY:
The GPIO pins can provide a low-current voltage which can power an LED but not much else.
Fortunately there are thousands of projects that use Raspberry Pis (or Arduinos, or many other
similar devices) as controllers for higher-powered devices, all the way up to the industrial
level, so online tutorials abound. For the simple case of a relay, you just need to know which
pin is a GPIO pin and which pin is a ground (GND) pin. Connect a GPIO and a GND from the Pi to
the relay and voila, you can send on/off signals. By default I'm using pin 16 but any of the 
designated GPIO pins should work. You need to tell the program which pin you're using by setting
the board_out value in ghost_config.txt. Be careful! The numbering scheme isn't straightforward, 
see the documentation here: https://www.raspberrypi.org/documentation/usage/gpio/

I've found that a simple circuit with an LED and a small (~50 ohm) resistor (to protect the LED
from drawing too much current) hooked up in series to a GPIO and GND pin is very useful for testing
purposes, especially if you're trying different on/off timings and want to try to synchronize to 
a sound effect. A bright LED can even be a decoration in itself: I have one inserted into a hollow,
translucent glow-in-the-dark plastic skull, which now flashes in time with its creepy laughter.
However you'll want to work out if there's any latency difference between your LED and relay if
you're going to use it for this purpose.
