Grove SerialLCD and Raspberry Pi
================================

See http://www.seeedstudio.com/depot/grove-serial-lcd-p-773.html

By default, most distributions of Linux for the Raspberry Pi use
/dev/ttyAMA0 (the UART) as a console.  You'll have to make a few changes so
that you can use it for other purposes (like attaching your SerialLCD
module).

First, edit /boot/cmdline.txt to remove any references to /dev/ttyAMA0.  My
old one looked like this:

  dwc_otg.lpm_enable=0 console=ttyAMA0,115200 kgdboc=ttyAMA0,115200 console=tty1 root=/dev/mmcblk0p2 rootfstype=ext4 elevator=deadline rootwait

Now it looks like this:

  dwc_otg.lpm_enable=0 console=tty1 root=/dev/mmcblk0p2 rootfstype=ext4 elevator=deadline rootwait

Next, edit /etc/inittab and comment out the line that runs getty on
/dev/ttyAMA0, if any.

The examples included here use GNU awk; so install it:

  sudo apt-get install gawk

Last, add the 'pi' user to the 'dialout' group so it has access to the UART:

  sudo usermod -aG dialout pi

Power off your Pi.  It's time to wire up the display!

Connect Ground and 5V from the Pi's P1 connector to the display.
Connect the Pi's Tx pin to the display's Rx pin.

DO NOT connect the display's Tx pin to the Rx on the Pi.  The display is a
5V device and will damage the Pi's 3v3 UART receive circuitry.

Power the Pi back up and run 'lcd-on'  You should see the backlight
come on.

You should be able to send messages to the display by piping text to
'lcd' like this:

  (date; uname -r) | ./lcd

or 

  echo -e "This is a\ntest..." | ./lcd

You can see all the possible characters on the display with 'chartest' or
turn the display off with 'lcd-off'.

NOTE: Sometimes it's difficult to break out of these programs; they tend to
not be interruptible with Control-C.  Try hitting Control-Z to suspend them,
then use 'kill %' to terminate the process.
 