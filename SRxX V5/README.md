# Applying smart receiver firmware

Using either a V4 or V5 controller with recent firmware

- Go to the settings page
- Select the "Update SRv5 Smart Receiver Firmware" - use the all option to send to all receiver chains at once.
- Select the receiver firmware file
- Flashing takes less than 30 seconds

What should happen is the receivers should go into firmware upgrade mode and apply the firmware. You can tell because at the end the fuse lights turn on and then off one at a time.

Use the Visualiser page to validate the receiver version on a V5 controller.

If you are having trouble getting a receiver to take the firmware there is a backup way to force it to take it.

- Power down the receiver
- Hold down the test button
- Power up the receiver
- Re do the instructions above.

I have never not seen this backup way work.

# LED states during firmware update

LED 1 Lit, 3 flashing : Waiting to receive firmware from the controller.
LED 1 Lit, 2 flashing : Controller has signalled about to send firmware.
LED 1 Lit, 2 flashing, 3 flickering : Firmware being received
FUSE LIGHTS 1-4 on ... 4-1 off once - firware ready to flash
LED 2 lit while actually flashing
FUSE LIGHTS 1-4 on ... 4-1 off three times - firware flashed ok

On boot if button is held for > 10 seconds then everything but the
bootloader is erased. Config is also cleared. Once you release the Fuse leds
will light 1-4 and then turn off 4-1 to indicate the deep erase has happened

On boot if button pressed for less than 10 seconds
and FL3 file is there then LED 3 lights while it is deleted.
Config is also cleared

On boot if FL3 is
present and valid the the four fuse lights will light then LED 2 lights while
firmware is applied. On completion the fuse lights will light 5 times.

On boot if FL3 is present on the board and invalid LED 1 lights while FL3 
is deleted ... then it goes to error state.

if boot button is pressed for less than1 1 seconds then
firmware goes into listen mode for 120 seconds for over the wire update If
the bootloader goes into serial listen mode then LED 1 will be solid on and
LED 2 will flash as data is received waiting for a packet once a packet is
seen LED 3 will light while the packet is handled If an error is detected LED
4 will flash

If the bootloader gets into loader mode as a result of the firmware seeing
dat 1 go high for an extended period then it will also go into serial mode
but only for 15 seconds. This is how the controller tells the receiver to get
ready for firmware.

If the uploaded firmware is invalid the 1 and 4 LEDS will flash alternately
until the button is pressed

If the uploaded firmware is fine but it does not load into FLASH then the 2
and 4 LEDS will flash alternately until the button is pressed

Error state is LED 4 flashing

If you end up with a subset of the fuse lights lit it is displaying the build number
and not taking the update. You will likely need to use the backup approach.
To read the build number
FUSE 1 = 1
FUSE 2 = 2
FUSE 3 = 4
FUSE 4 = 8

Sum up the lit fuses ... so FUSE 1 and FUSE 2 would be 1 + 2 = 3.