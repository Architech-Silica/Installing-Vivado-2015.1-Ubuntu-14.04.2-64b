# Installing Vivado 2015.1 on VirtualBox

### How to install Xilinx Vivado 2015.1 on Ubuntu 14.04.2 (64 bit), including a functional JTAG cable.

1. Setup Ubuntu using:

 	<pre>
	sudo apt-get update
	sudo apt-get install curl fxload gawk wget git-core diffstat unzip texinfo gcc-multilib build-essential chrpath libsdl1.2-dev xterm vim curl minicom xsltproc cmake u-boot-tools nfs-common nfs-kernel-server
	sudo ln -s /usr/bin/make /usr/bin/gmake
	sudo chmod a+rwx /opt
	sudo dpkg --add-architecture i386
	sudo apt-get update
	sudo apt-get install libc6:i386
	sudo apt-get install libstdc++6:i386
	sudo apt-get install libfontconfig1:i386
	sudo apt-get install libXext6:i386
	sudo apt-get install libXrender1:i386
	sudo apt-get install libgthread1:i386
	sudo apt-get install libglib2:i386
	sudo apt-get install libglib2.0-0:i386
	sudo apt-get install libsm6:i386
	</pre>

2. Create directory:
  
	`mkdir ~/utils`

	then copy the following files into it:
	
	+ Xilinx_Vivado_SDK_2015.1_0428_1.tar.gz
	+ 52-xilinx-pcusb.rules
	+ libusb-1.0.9.tar.bz2
<p>

3. Install Vivado as un-priviledged user:
 
	<pre>
	cd ~/utils  
	tar xzvf Xilinx_Vivado_SDK_2015.1_0428_1.tar.gz  
	cd Xilinx_Vivado_SDK_2015.1_0428_1
	./xsetup
	</pre>

	During installation select "Vivado System Edition", then select "SDK", and **un-select** installation of "cable device drivers".

4. Install Digilent and Xilinx Platform Cable drivers:

	<pre>
	cd /opt/Xilinx/Vivado/2015.1/data/xicom/cable_drivers/lin64/install_script/install_drivers  
	sudo ./install_digilent.sh  
	cd /opt/Xilinx/Vivado/2015.1/data/xicom/cable_drivers/lin64/install_script/install_drivers  
	sudo ./install_drivers
	</pre>

	<pre>
	cd ~/utils  
	tar -xjvf libusb-1.0.9.tar.bz2  
	cd libusb-1.0.9/  
	./configure   
	make  
	sudo make install
	</pre>

	Check installation of "libusb" using:

	`ls /usr/local/lib/`
	
	a file like `libusb-1.0.so.0.1.0` should be listed.

	Copy the rules file:
	
	`sudo cp ~/utils/52-xilinx-pcusb.rules /etc/udev/rules.d/`
	
	Check rule files:

	`ls /etc/udev/rules.d`

	It should be listed something like:
    
	<pre>
	-rw-r--r-- 1 root root 3721 Apr 28 18:35 52-xilinx-digilent-usb.rules
	-rwxr-xr-x 1 root root  435 Apr 29 09:03 52-xilinx-pcusb.rules
    -rw-r--r-- 1 root root  134 Apr 28 17:38 60-vboxadd.rules
    -rw-r--r-- 1 root root 1157 Apr 14  2014 README
	</pre>

5. Generate (webpack) license

	<pre>
	sudo su
	cd /opt/Xilinx/Vivado/2015.1/bin/unwrapped/lnx64.o/ 
	./install_fnp.sh 
	export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/Xilinx/Vivado/2015.1/lib/lnx64.o/
	sudo ./xlicclientmgr -cr clickforlicense.xml
	exit
	firefox clickforlicense.html
	</pre>
	on lower tab "Activation Based Licenses" tick the "Vivado WebPACK License", then click on "Activate Node-locked License" button
	download the Xilinx_License.xml file then launch License manager using
	`vlm`
	then select "Load License" then click on button "Activate License" and point to file Xilinx_License.xml 

6. Create a script file `vivado.sh` containing the following:

    <pre>
	export XIL_IMPACT_USE_LIBUSB=1
    export DISPLAY=":0"
    source /opt/Xilinx/Vivado/2015.1/settings64.sh
    sudo /opt/Xilinx/Vivado/2015.1/data/xicom/cable_drivers/lin64/install_script/install_drivers/setup_pcusb
    export LM_LICENSE_FILE=~/.Xilinx
    export XILINXD_LICENSE_FILE=~/.Xilinx
    export LC_CTYPE="en_US.UTF-8" 
    export LC_NUMERIC="en_US.UTF-8"
    LC_TIME="en_US.UTF-8" 
    LC_COLLATE="en_US.UTF-8" 
    LC_MONETARY="en_US.UTF-8" 
    LC_MESSAGES="en_US.UTF-8" 
    LC_PAPER="en_US.UTF-8" 
    LC_NAME="en_US.UTF-8" 
    LC_ADDRESS="en_US.UTF-8" 
    LC_TELEPHONE="en_US.UTF-8" 
    LC_MEASUREMENT="en_US.UTF-8" 
    LC_IDENTIFICATION="en_US.UTF-8" 
    LC_ALL=en_US.UTF-8
    </pre>

7. Source the script using `. vivado.sh`

	then launch vivado by typing `vivado`.

8. Connect JTAG cable and link it to the VM from "Devices->USB devices" VirtualBox menu.  The Digilent cable should appear as "Digilent Adept USB Device" while then Xilinx Platform Cable should appear as "Unknown device 03FD:0013".

9. From Vivado launch "Open Hardware Manager", then "Open Target->Auto Connect".
If you have a Digilent cable the setup is done.
Instead, if you have a Xilinx Platform Cable, its led will light up (its firmware has been loaded) but to use it you need to go back to the "Devices->USB devices" VirtualBox menu and select "XILINX". After that click "Cancel" on Hardware Manager and then once again the "Open Target".  Your cable setup is done.


Contributions
-------------
Code examples and how-to guides are provided for your use, but please feel free to contribute your own code and wisdom back to this repository via a pull request in the usual fashion.  Please fork from this repo, then create a suitably named branch in your fork before submitting back to this repo.  Please don't submit a pull request from your "master" branch.  Each new addition to the code should belong to its own submitted branch.  Thanks. 
 
