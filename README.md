# RRM
The exploit gains code execution on the Wi-Fi firmware on the iPhone 7.

The exploit has been tested against the Wi-Fi firmware as present on iOS 10.2 (14C92), but should work on all versions of iOS up to 10.3.3 (included). However, some symbols might need to be adjusted for different versions of iOS, see "exploit/symbols.py" for more information.

Upon successful execution of the exploit, a backdoor is inserted into the firmware, allowing remote read/write commands to be issued to the firmware via crafted action frames (thus allowing easy remote control over the Wi-Fi chip). 

The attached archive contains the following directories:
  - hostapd-2.6 
    - A modified version of hostapd utilised in the exploit. This version of hostapd is configured to
    support 802.11k RRM, and in particular Neighbor Reports. Moreover, this version of hostapd is
    instrumented to add various commands, allowing injection and reception of crafted action frames
    used throughout the exploit.
  - exploit     
    - The exploit itself.

To run the exploit, you must execute the following steps:
  - Connect (and enable) a SoftMAC Wi-Fi dongle to your machine (such as the TL-WN722N)
  - Compile the provided version of hostapd
  - Modify the "interface" setting under "hostapd-2.6/hostapd/hostapd.conf" to match your interface's name
  - Configure the following settings under "exploit/conf.py":
    - HOSTAPD_DIR - The directory of the hostapd binary compiled above
    - TARGET_MAC  - The MAC address of the device being exploited
    - AP_MAC      - The MAC address of your wireless dongle
    - INTERFACE   - The name of the wireless dongle's interface
  - Assemble the backdoor shellcode by running "exploit/assemble_backdoor.sh"
  - Run hostapd with the configuration file provided above, broadcasting a Wi-Fi network ("test80211k")
  - Connect the target device to the network
  - Run "exploit/attack.py"

Following the steps above should result in installation of a simple backdoor allowing read/write access to the firmware. You can interact with the backdoor to gain R/W access to the firmware by calling the "read_dword" and "write_dword" functions, respectively.
