# OpenYaleL1
This is a placeholder for my work on reverse engineering the Yale Conexis L1 module comunication protocl.
At the moment this readme will just serve as public notes but I hope to develop this into a matter over thread plug in replacement.

I have so far determined:
The module uses UART communication at 19200 baudrate with no parity bit.

The comminication is a variable lenght, typically no less than 5 bytes.

- The first two bytes determine the command being issued.
- The third byte counts up starting at 0x00, this is reset each time the lock is powered off an on, assuming it also rolls over, but not tested.
- The next bites are a variable length containing the payload for the specific command
- Finally a message always ends with a checksum which is a single byte calculated from the XOR of all the other bytes.

In addition, prior to the module sending a command a pin is pulled down for a moment before the command is sent. The lock does the same, but on two pins. I suspect this serves two purposes:

- To wake either controller from sleep
- To interrupt whatever the respective microcontroller is doing to ensure it is ready to RX the UART message.

The Conexis L1 uses an STM32L microcontroller, whilst the Yale Access Module (the new yellow module to "upgrade" your L1 to work as an L2) contains an NRF52840 SOC.

Currently working on:

- Socumenting the commands, initially as a POS just initialisation and LOCK / UNLOCK
- A POC to demoonstrate interfacing with the lock and proving the concept.
- Researching various SOC's/boards/frameworks for the open source module: NRF52840, Arduino Nano Matter (MGM240SD22VNA), ESP-C6

Future Vision:

The plan is to have a drop in replacement module that implements the full command set of the modules. I plan to use thread over matter, as it is an open standard and is the direction of the industry. This does add some learning curves and challenges due to lock support being reasonable new and some frameworks not having full support yet.

I do not intend to create a replacement app as my interests are in Home Assistant support above all else. Happy to accommodate app support though.


