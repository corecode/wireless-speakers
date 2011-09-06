## Pin assignments (v2):

ROT_BUTTON	PD2	in
ROT_1		PD3	in
ROT_2		PD4	in
IN_SEL		PD5	in
PWRLED		PB1	out
#TUNED		PC0	in
#SCAN		PC1	out
#AMP_SLEEP	PC2	out
#AMP_MUTE	PC3	out
SDA		PC4	TWI
SCL		PC5	TWI


## Plan of operation

### startup

configure defaults for the TDA

trigger SCAN N times to reach programmed channel


### continuous always

debounce buttons

if !IN_SEL: if !TUNED, set AMP_MUTE, else clear AMP_MUTE

if IN_SEL changes, change channel selection on TDA; if IN_SEL, clear AMP_MUTE


### default mode

rotation changes volume setting

short button press toggles AMP_MUTE

long button press transitions to settings mode

if AMP_MUTE: LED pulses, else LED is simply on.


### settings mode

Each settings mode has its unique LED signature that is displayed
continuously.  Every change will make the LED blink very fast for one
second.

If no interaction happens during settings mode for 20 seconds,
transition back to default.

A short button press will accept the changes and cycle forward.
Values are written to EEPROM.

A long button press will discard the changes and return to default
mode.

Rotary control increases/decreases the value.

All changes take immediate (preview) effect (also channel select!).


#### bass control

LED signature: fade from bright to dark


### mids control

LED signature: fade dark-bright-dark (sine?)


### trebble control

LED signature: fade from dark to bright


### pan control

LED signature: fade bright-dark-bright (sine?)


### channel select

LED signature: longer blinks.  if !TUNED, second part of the blink is darker.

AMP_MUTE is cleared.


### input channel amplification

LED signature: short blinks

adjusts the volume of the current IN_SEL input; IN_SEL may be changed
during adjustment.
