# Setup

NB! Always remember to turn off the boards after use to reduce wear on the SD-card.

## PYNQ-Z1 Ethernet Connection

Connect the board to a power supply and an Ethernet cable connected to your PC. Make sure the board is powered on, open the terminal and run:

```
arp | grep "<iface-name>"
```

Identify the IPv4 address and login:

```
ssh xilinx@{ip}
```
Ask a team member if you don't know the password.

## PYNQ-Z1 Serial Connection

Credit: Tutorial materials, the getting started guide for PYNQ-Z1, and various stackoverflow searches.

Sometimes, the board might be IPv6 only, or nmap scans can take alot of time.
An alternative approach to determining if the board is connected to the network at all, and determining the address, is to connect to its serial port.

1. Install minicom: `sudo apt-get install minicom`.
2. With the board powered on, connect cable from computer to the microusb port labeled "PROG / UART".
3. Find the correct serial port for the board, active boards can be found by using `dmesg | grep tty`. It is usually ttyUSB0 or ttyUSB1.
3. Start minicom with `minicom /dev/<device-name-from-step-3>`
4. Configure minicom (<kbd>Ctrl+A</kbd> <kbd>Z</kbd>, then O for cOnfigure Minicom), bps must be set to 115200, 8N1, HW and SW Flow control must be off ("No").
5. Exit setup. Type something in minicom and press enter, you should see a xilinx@pynq command prompt. If not, you are on the wrong serial port.

When you are at the command prompt, use the `ifconfig` command to see the current network configuration. Here you should see the ethernet connection, and whether or not you have assigned an IPv4 or IPv6 address.

## PYNQ-Z1 Wireless Connection
To ease development, especially for those who lack an ethernet adapter on their development machines, we've setup our own router (with access to the NTNU-network) in the software lab, which you can hook up the PYNQ-boards to.

SSID is JARVIS, ask a team member for the pw (remember that this is an open repo).

When on the network, just switch on the board you wish to work on, and it should have a static IP setup from the getgo.

Currently, the set IPs are as follows:
```
192.168.1.2 GUNN
192.168.1.4 BJORG
192.168.1.5 SOLFRID
192.168.1.7 AASE
```

You can use this information to make SSH-connections a bit simpler (instead of searching for the IP every time).
If using Ubuntu, you can edit your /etc/hosts file to include this routing information.

An example of the /etc/hosts file with the information filled in:
```
127.0.0.1       localhost
127.0.1.1       <your computer hostname here>

192.168.1.2     gunn
192.168.1.4     bjorg
192.168.1.5     solfrid
192.168.1.7     aase
```

Now you can simply SSH into, for example gunn, by entering:
`ssh xilinx@gunn`

And if you wish to connect to the Jupyter Notebook, go to the following URL in your preferred browser.
http://gunn:9090
