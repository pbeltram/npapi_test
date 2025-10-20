## npapi (Network packet API)

Wire speed 119Mbytes/sec (970Mbits/sec) continuous data capture and recording from Zynq-7000 GEM 1Gb Ethernet controller.

Implementation is using UDP data transfer with addition that during data capturing it will request re-transmission of missing UDP packets. Result is continuous data capturing without data lost.

Demo for RedPitaya demonstrates continuous raw ADC 16bit data capture at up to 59.5MSPS for single 16bit ADC channel and up to 29.7MSP for both ADC channels.

Demo for ZyboZ7-20 demonstrates continuous DIO capture at up to 59.5MSPS for 16bit DIO inputs and up to 29.7MSP for 32 bit DIO inputs from 4 PMOD connectors.

**Directories:**

- `./pics/`. Pictures with references from this README.
- `./fpga/`. FPGA block designs. (description in [README.md](./fpga/README.md)).
- `./bin/`. Prebuilt and staged PC binary files to control and capture data blocks for Linux and Windows (description in [README.md](./bin/README.md)).
- `./lnx/`. Files for Zynq 7000 boards. (description in [README.md](./lnx/README.md)).
- `./python/`. Python script to analyze captured data (description in [README.md](./python/README.md)).

#### Zynq 7000 Board configuration

You will have to create SD card to boot your Zynq 7000 board with it. Files and instructions are located in `./lnx/` directory in `./lnx/README.md` file. 
Current IP address/netmask for each board is set in its `/etc/network/interface` file. For RedPitaya it is set to `169.254.50.28/24` and for ZyboZ7 it is `169.254.50.27/24`. 

#### PC configuration

On PC you will need a dedicated 1Gb Eth NIC card for direct Ethernet connection to RedPitaya. It is possible to use also connections via network switch, but it is very dependent on performance and capabilities of network switch (e.g. successful tests were done on MicroTIK 4 port 10Gbeth switch). For this testing it is preferred to use direct Ethernet cable connection. PC NIC card must support jumbo frames with at least 8192 bytes. Tests were done on three different 1Gb Eth PC NIC cards: Intel I219-V on PC Intel NUC (Windows 11), Intel I217-LM on PC HP Z-Book laptop (Ubuntu) and Intel I210 on PC HP-Z420 Workstation (Ubuntu).

PC NIC must be configured to use jumbo frames with MTU=8192 (or larger). NIC IP address must be set on the same IP network address/netmask as RedPitaya, for example `169.254.50.1/24`.

Configurations of PC on Ubuntu and Windows 11 are different.

##### Ubuntu (preferred)

Set IP address/netmask of PC NIC to `169.254.50.1/24` and MTU to at least 8192.

Next you have to change Ubuntu configuration to boost UDP performance with changes in system file `sudo /etc/sysctl.cfg`. Append this lines at the end of file:

```
# UDP performance settings
net.core.rmem_max=134217728
net.core.rmem_default=134217728
net.core.netdev_max_backlog=5000
kernel.shmmni=32768
```

After all this changes it is good to do a reboot of PC and verify all settings are OK with `ping 169.254.50.28` RedPitaya connection and UDP performance settings with:

```
sudo sysctl net.core.rmem_default #134217728
sudo sysctl net.core.wmem_default #134217728
sudo sysctl net.core.netdev_max_backlog #5000
```

See also pictures in `./pics/ubuntu`.

##### Windows

Disable Windows firewall. On Windows 11 tests all firewalls were disabled. There might be some options to tune this, but no experimenting was done with firewall enabled.
NOTE:
Sometimes capturing on my Windows 11 NIC becomes unstable. This shows up as flooded requests for resend and than link connection toggle. This is some Windows 11 problem (or Intel I219-V NIC device driver). Disable network interface and than enable it again brings it back to stable state. Looks like that some Windows system service is intervening on network interface and its traffic. This problem has become more frequent after latest October.2025 Windows 11 update.
Preferred OS for testing is Linux.

Set IP address/netmask of your PC NIC to `169.254.50.1/24` via `ControlPanel\Network and Internet\Network Connections` and properties of 1Gb Eth NIC.

After that you have to change configuration of PC NIC card. This settings depends on capabilities of NIC card but in general what needs to be changed are "Jumbo Packet" to 8192 (or larger), "Receive Buffers"" to its maximum (e.g. 2048 or 4096) and "Interrupt Moderation Rate" to Extreme.
Configure Network driver (NIC Intel(R) Ethernet Connection (6) I219-V)

Example for `1G link (Intel(R) Ethernet Connection (6) I219-V)`:
Control Panel->Network and Sharing Center->Change adapter settings:
Select `1G link (Intel(R) Ethernet Connection (6) I219-V)` and Right Click -> Properties:
Configure->Advanced:

- Flow Control: Rx & Tx Enabled
- Interrupt Moderation: Enabled
- Interrupt Moderation Rate: Extreme (ITR=3600)
- Jumbo Packet: 9014 Bytes
- Receive Buffers 2048 (Max)
- Transmit Buffers: 512 (Default)

See also pictures in `./pics/windows`.

**NOTE:**

You can also use VirtualBox image for Ubuntu 20.04 (uname: `ubuntu`, pswd: `ubuntu..`) with PC binaries and configuration already set. It can be download from this [link here](https://drive.proton.me/urls/RHEGATVK74#BkoI85NGY7KS) (4GBytes md5sum: 697f991eb549f2501633f2732a98922b).
But you will still have to do configurations on your PC NIC card and then bound it to your physical NIC card via VirtualBox `Bridged Adapter` network settings.
Performance by using VirtualBox are slightly worst, because of additional calls in handling network data.
