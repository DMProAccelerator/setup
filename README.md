NOTE: Always turn off the PYNQ-boards after use, as the SD-card will wear out over time due to heavy logwriting on the PYNQ.

# System Setup
In order to run a fully functional version of the system, including development on-board, it is essential to follow these steps.
(or just download the latest QBART Build from http://folk.ntnu.no/kristovm/ , which is basically what you get from following these steps).

1. Download the newest, clean image from https://github.com/Xilinx/PYNQ .
2. Use an image burner (such as Win32DiskImager for windows, or Brasero for Ubuntu) to burn the image to a microSD.
3. Plug the SD-card into the PYNQ-board.
4. Connect the PYNQ to a network (see examples in the connection setup section), preferably one connected to the internet.
5. Turn on the power. The button LEDs should blink after 30-60 seconds. If not, something probably went wrong when writing the image (did you remember to unmount before removing the SD-card?)
6. Connect to the device. (Again, see connection setup). Default user and pw is "xilinx" - we'll change this shortly.
7. Currently, PYNQ uses a custom built variant of Ubuntu 15.10, which has been deprecated for quite some time. This has recently led to issues when using apt, as the repos has been moved to archive. One possible solution is:
```
sudo sed -i s/wily/vivid/g /etc/apt/sources.list.d/multistrap-wily.list
```
NB! If any weird issues arise from any tools, this hack should be the first thing to look at. We use the repos of Ubuntu 17, maybe one has to use one for an older version? So far it works just fine, though.

UPDATE: After some weird broken pipe errors and apt returning error codes, the repos have now been changed to the vivid ones, which is an earlier version of Ubuntu 15 than the one on the system, but is still officially supported. If this also breaks, then one has to look into adding the archive repos for the actual ubuntu 15 on the system (wily)

8. ```sudo apt-get update && sudo apt-get upgrade``` is also nice at this time to update potentially old packages.
9. The image already comes with Jupyter Notebook, but it doesn't have the python2 kernel installed by default. This will cause issues if you run QNN tutorial notebooks on the board (as it is written in python 2), and if you plan to reuse some of the code in your own Jupyter Notebook. So we'll install the python2 ipython kernel.

First we must upgrade pip, as the version included is quite old:
```
sudo pip install -U pip
```

Then we use pip to install the python2 kernel, and register the python 2 kernelspec. Credit: https://github.com/jupyter/jupyter/issues/71
```
sudo python2 -m pip install --upgrade ipykernel && sudo python2 -m ipykernel install
```

10. Next, we'd like for security reasons to not use default passwords, as it is always an awful practice not to change them. If on the QBART-team, there is a default password, ask a member. We need to update both sudo and user, so we run:
```
sudo passwd && passwd
```

There is also an additional default password we need to get rid of:
We also need to change the password of the Jupyter server, reachable when the PYNQ is running at  <PYNQip>:9090, as it is already running out of the box. From the PYNQ-docs:
 hashed password is saved in the Jupyter Notebook configuration file.

```/root/.jupyter/jupyter_notebook_config.py```

You can create a hashed password using the function IPython.lib.passwd(), run these lines either in a python shell, or your preferred IDE, to create a hash:

```
from IPython.lib import passwd
password = passwd("secret") # Secret should be replaced by team password.
print(password)
```
Should give you the hash. Copy and paste the hash.

You can then add or modify the line in the ```jupyter_notebook_config.py``` file

```
c.NotebookApp.password =u'sha1:<Your hash here>'
```

11. Lastly, we would most likely want to give each PYNQ a unique hostname, as the default "pynq" can be uninformative if one is to use several in a parallell OpenMPI approach. Use the provided PYNQ-script:
```
pynq_hostname.sh <NEW HOSTNAME>
```
Remember to reboot the board afterwards in order for changes to take effect.

12. In addition, several other packages must be installed in order for the system to work:
* TODO: <To be added continously>

13. Finally, development files must be installed at the following locations:
TODO: Add these when files become finished

13. It is also a lot of work to repeat this process for every single board.
In order to reduce the amount of work, one would ideally set up one board in the above manner, then we clone the SD-card to an image file, and then burn the image file to other PYNQ SD-cards. The image file should be provided on http://folk.ntnu.no/kristovm/ for easy reuse (placing it in a repo makes pulls a bit more of a strain). It is important to change hostname after flashing to other boards. Kinda pointless if all boards are named the same.

# Connection setup
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
