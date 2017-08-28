# Setup

## PYNQ-Z1 Direct Ethernet Connection

### Ubuntu 16.04

1. Install nmap: `sudo apt-get install nmap`.
2. Connect the board to a power supply.
3. Connect an Ethernet cable to your computer.
4. Run `ifconfig` from the terminal and identify the IPv4 address of your Ethernet connection.
5. Run `nmap -sn {ip}/24`.
6. Note the IPv4 address of the PYNQ-Z1 and run `ssh xilinx@{ip}`.
7. Enter password, and you're in!
