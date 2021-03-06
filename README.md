#Blackbox - a RFID project

First phase is to build a state machine with various RFID tags<br>
Use a hexeditor to edit the 1K Mifare template file and create a 'door' unlock card

#Start of environment
I read some good and even better blogs, code snippits and alike, still the setup took some time.
I wanted to share how I got to the point that I could start writing Python code for the puzzel. <br>

So I personally started with a Raspbian Wheezy image file (2014 version for Raspberry Pi B model 2.. yes there are newer versions but this was the one I already had).
First :  
```bash
sudo raspi-config 
```
set `GPU ram` to 0, `enable SPI & I2C bus`, `set timezones`, `expanded the filesystem` and `changed the pi password`.
(yes, you should do this also..)

Then on your bright new booted image you would want to update to get the latest and greatest stuff:
```bash
sudo apt-get update
sudo apt-get upgrade
```

I recommend you also watch a nice tutorial that was released in October 2015 by [Andr.oid Eric](https://www.youtube.com/watch?v=LGs048l6kbQ&list=PLP7qPet500dcE-zP_-EVEisi7N1Lh4Ekk). Just as a starter. <br> 

Next step, check for SPI devices:
```bash
   ls /dev/spi* 
```
They should be there.. if not you may have a blacklist issue, check your Raspberry settings with the:
```bash
sudo raspi-config
```
command and enable SPI and I2C in the advanced option.. reboot and check again.. :-) no other way possible.
If those SPI devices are there, then you can continue.
```bash   
   cd ~
   git clone https://github.com/lthiery/SPI-Py
   sudo python setup.py install
   git clone https://github.com/rasplay/MFRC522-python
```
Then.. (the most) Important! Edit the device tree:
```bash
sudo nano /boot/config.txt
```
Add one line at the bottom:  device_tree=on
Then reboot the Pi:
```bash
sudo shutdown now -r
```
This device tree-stuff seems to be crucial, in some blogs it is missed and in some it is present. 
This is where my first try went wrong.
Then, exiting moment, test your RFID-RC522 reader with:
```bash
sudo python read.py
```
in the directory `/MFRC522-python`, it should do something like this:
```bash
Card detected
Card read UID: 102,162,107,48,159
Card detected
Card read UID: 172,79,145,110,28
wrong Card
(etc)
```
Now the fun starts, we need another library.. <br>
Use the [mxgxw](https://github.com/mxgxw/MFRC522-python) examples !Mind the other author name!.
First make another folder (e.g. mxgxw) cd into that new directory and then use the following to clone:
```bash
cd ~
mkdir mxgxw
cd mxgxw
git clone https://github.com/mxgxw/MFRC522-python
```
This is nasty but needed because otherwise we will end up with 2 of those `MFRC522-python` folders with exactly the same name.
Try the following (including the `sudo`):
```bash
sudo python Dump.py
```
For more code visit  the [mxgxw Blog](http://helloraspberrypi.blogspot.nl/2015/10/raspberry-pi-python-mxgxwmfrc522-python.html), the second code is really nice because it dumps a card, asks you to remove the card and then cycles to a read again. Watch the video and enjoy a bit of coding.<br>

If you then fiddle around with the code in another directory like the `\blackbox` directory, do copy the 2 `MFRC522.py*` files, they contain a (compiled?) library that you will need to import in your python code when interacting with the SPI RFID reader device on SPI port 0.

#Next up
-Get the 2.2 TFT display working on SPI port 1 since SPI0 is taken.

#Links
-Mifare 1Kbyte Card info [http://www.nxp.com/documents/data_sheet/MF1S50YYX.pdf] <br>
-Raspberry Pi pinout ![image](http://pingig.com/sites/default/files/raspberrypirev2pinout.jpg)
