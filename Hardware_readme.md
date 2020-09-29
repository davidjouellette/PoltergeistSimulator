HARDWARE REQUIREMENTS:
-Raspberry Pi:     In theory any model with audio output and GPIO pins should work, this was tested on a model 3)
-Powered Speaker:  The Pi doesn't have enough power to drive a decent-sized speaker)
-Electrical Relay: The Pi can't power much more than an LED from the GPIO, but it send
                   3/5 volt control triggers to turn a relay on and off. For simplicity 
                   and safety I use a switchable power strip like this one:
                   https://www.amazon.com/gp/product/B00WV7GMA2/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1
                   but with a little electronic know-how the same effect can be achieved for 
                   lower cost
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
                   
NOTES ON HARDWARE:
This program assumes you're running it on a Raspberry Pi. There is a stripped-down version
for running on a normal computer (platform agnostic, at least in theory) with only the audio
component here: https://github.com/davidjouellette/PoltergeistSimulator_Desktop.

AUDIO OUTPUT:
PoltergeistSimulator on Raspberry Pi uses OMXPlayer, which is included with Raspberry Pi OS 
(formerly Raspbian). PoltergeistSimulator assumes you're using an external speaker through 
the 3.5mm port to play sounds, so it calls OMXPlayer with default options (which in turn 
defaults to playing sounds through the analog port). OMXPlayer will also play video clips,
and will default to playing them through the HDMI port, but the audio for the clip will still
default to the analog output. So if you want to use video instead of audio, you may need to 
modify the call to OMXPlayer in PoltergeistSimulator very slightly to play the audio through
HDMI as well. Check the OMXPlayer documentation for how to do that: 
https://www.raspberrypi.org/documentation/raspbian/applications/omxplayer.md

Sometimes I would get a background hum on my speaker. I was using a small battery-powered one
and it would hum while plugged in for charging. I think speaker interference is a common problem
with the Pi so I direct you to Google which can probably provide better answers than I.

SETTING UP THE RELAY:
The GPIO pins can provide a low-current voltage which can power an LED but not much else.
Fortunately there are thousands of projects that use Raspberry Pis (or Arduinos, or many other
similar devices) as controllers for higher-powered devices, all the way up to the industrial
level, so online tutorials abound. A quick tutorial for simple cases is provided below.

