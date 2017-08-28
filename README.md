# Setup

## PYNQ-Z1 Direct Ethernet Connection

### Ubuntu 16.04

1. Install nmap: `sudo apt-get install nmap`.
2. Connect the board to a power supply.
3. Connect an Ethernet cable to your computer.
4. Locate the Ethernet connection in the Network Manager interface and set the method of the IPv4 settings to "Shared to other computers".
5. Run `ifconfig` from the terminal and identify the IPv4 address of your Ethernet connection.
6. Run `nmap -sn {ip}/24`.
7. Note the IPv4 address of the PYNQ-Z1 and run `ssh xilinx@{ip}`.
8. Enter password, and you're in!


## PYNQ-Z1 Serial Connection

### Ubuntu 17.04 (should work for 16.04 as well)
Credit: Tutorial materials, the getting started guide for PYNQ-Z1, and various stackoverflow searches.

Sometimes, the board might be IPv6 only, or nmap scans can take alot of time.
An alternative approach to determining if the board is connected to the network at all, and determining the address, is to connect to its serial port.

1. Install minicom: 'sudo apt-get install minicom'.
2. With the board powered on, connect cable from computer to the microusb port labeled "PROG / UART"
3. Find the correct serial port for the board, active boards can be found by using "dmesg | grep tty". It is usually ttyUSB0 or ttyUSB1.
3. Start minicom with "minicom /dev/deviceNameInStep3Here"
4. Configure minicom ("Ctrl+A Z", then O for cOnfigure Minicom), Bps must be set to 115200, 8N1, HW and SW Flow control must be off ("No")
5. Exit setup. Type something in minicom and press enter, you should see a xilinx@pynq command prompt. If not, you are on the wrong serial port.

When you are at the command prompt, use the ifconfig command to see the current network configuration. Here you should see the ethernet connection, and whether or not you have assigned an ipv4 or ipv6 address.
