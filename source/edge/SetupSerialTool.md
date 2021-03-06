title: Setup Serial Debugging Tool
---

### Preparation
- [x] A Serial Debugging Tool. In this guide, we will use a USB to TTL Converter. Ensure that it supports the `1500000` baudrate.
- [x] Edge needs the Edge-IO breakout-board to support serial debugging.

### Connections
Follow these steps to make the correct connections:

1) Connect Edge-IO board to Edge via the FPC connector.
![Edge IO FPC](/images/edge/edge_io.gif)

2) Connect all the GPIOs, and check that the Tx / Rx Pins are connected correctly:

  * Tool Pin `GND`: <---> Edge IO `GND`
  * Tool Pin `TXD`: <---> Edge IO `RXD`
  * Tool Pin `RXD`: <---> Edge IO `TXD`

The connections should look like this:

![Image of SerialConnections](/images/edge/SerialConnections_3Pin.png)

3) Insert the USB-end into your Host-PC.

### Setup Kermit Protocol(C-Kermit)
**Install c-kermit:**
```sh
$ sudo apt-get install ckermit
```

**Add Access Permission**
```sh
$ sudo usermod -a -G dialout $(whoami)
```

**Add the following contents into ~/.kermrc to finish the setup:**
```
set line /dev/ttyUSB0
set speed 115200
set carrier-watch off
set handshake none
set flow-control none
robust
set file type bin
set file name lit
set rec pack 1000
set send pack 1000
set window 5
c
```

**Run the command `kermit` to launch C-Kermit**

Ensure that you have made the right connections, and if everything went fine, terminal will print this out:
```sh
$ kermit
Connecting to /dev/ttyUSB0, speed 115200
 Escape character: Ctrl-\ (ASCII 28, FS): enabled
Type the escape character followed by C to get back,
or followed by ? to see other options.
----------------------------------------------------
GXL:BL1:9ac50e:a1974b;FEAT:ADFC318C;POC:3;RCY:0;EMMC:0;READ:0;0.0;CHK:0;
TE: 116640

...

```
*Tips: If the print-out contains the following line, you might need to check the step `Add Access Permission` above.*
```
/dev/ttyUSB0: Permission denied
```

### Enable 1500000 baudrate
To enable `1500000` baudrate, you need to replace the `kermit` binary. See [Khadas Kermit](https://dl.khadas.com/Tools/kermit) to download, and execute the following commands:
```sh
$ chmod +x kermit
$ sudo cp kermit /usr/bin/kermit
```

If you want to use the `1500000` baudrate, you need to modify the `~/.kermrc` speed to `1500000`.

### See Also
* [C-Kermit Offical website](http://www.columbia.edu/kermit/index.html)
