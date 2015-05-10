# MSRxxx_track_reader-writer

It should work with other products from Dingword (MSR206, MSR606...).

##Features:

read/write ISO compatible tracks,  
read/write raw tracks,  
configure write density (210 bits/inch or 75 bits/inch),  
configure coercivity (high or low),  
command line interface, to be used from shell,  
python class interface, to be used by other scripts.  

##Dependencies:

pyserial, available at https://pypi.python.org/pypi/serial_device2,  
(USB version) a driver for the PL-2303 serial/USB adaptor embedded in the card reader.  

On Mac OS X, I use the driver from the osx-pl2303 project (here for Intel 64bits support). I've installed pyserial through macport, package py27-serial (for python 2.7). The serial device is called /dev/cu.PL2303-xxx.  

On FreeBSD, the driver (uplcom) is embedded in the default kernel, and serial device is called /dev/cuaU0.  

Should also work on Linux and Windows as they are compatible with pyserial.  

##Command line description:

Read the 3 strips (swipe when the orange LED is on):  

./msr.py --device /dev/cuxxx --read  
1=%............?        or None if strip is empty  
2=;............?        or None if strip is empty  
3=;............?        or None if strip is empty  
Write strips 2 and 3 (swipe when the orange+green LEDs are on, do not include the leading/trailing chars ;/%/?):  

./msr.py --device /dev/cuxxx --strip 23 --write "strip2" "strip3"  
Erase the 3 strips (swipe when the orange+green LEDs are on):  

./msr.py --device /dev/cuxxx --erase  
Online help (to set low-coercivity or density before writting):  

./msr.py --help  

##Class description:

You can "import msr" this file to use the MSR606 class in your own code.

class msr inherits from Serial  
    constructor : (self, dev_path)  
    method reset(self)  
    method read_tracks(self)  
    method write_tracks(self, t1, t2, t3)  
    method erase_tracks(self, t1, t2, t3)  

###Example:

r = msr(/dev/cuaU0)  
t1,t2,t3 = r.read_tracks()  
r.write_tracks(t3=1234567890)  
r.erase_tracks(t1=True)  
