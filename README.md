# Setup

## PYNQ-Z1 Ethernet Connection

Connect the board to a power supply and an Ethernet cable connected to your PC. Make sure the board is powered on, open the terminal and run:

```
arp | grep "<iface-name>"
```

Identify the IPv4 address and login:

```
ssh xilinx@{ip}
```

The password is `xilinx`. Enjoy!

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
