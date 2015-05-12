# Installing Vivado 2015.1 on VirtualBox

### How to install Xilinx Vivado 2015.1 on Ubuntu 14.04.2 (64 bit), including a functional JTAG cable.

1. Setup Ubuntu using:

 	<pre>
	sudo apt-get update
	sudo apt-get install curl fxload gawk wget git-core diffstat unzip texinfo gcc-multilib build-essential chrpath libsdl1.2-dev xterm vim curl minicom xsltproc cmake u-boot-tools nfs-common nfs-kernel-server
	sudo ln -s /usr/bin/make /usr/bin/gmake
	sudo chmod a+rwx /opt
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

5. Create a script file `vivado.sh` containing the following:

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

6. Source the script using `. vivado.sh`

	then launch vivado by typing `vivado`.

7. Connect JTAG cable and link it to the VM from "Devices->USB devices" VirtualBox menu.  The Digilent cable should appear as "Digilent Adept USB Device" while then Xilinx Platform Cable should appear as "Unknown device 03FD:0013".

8. From Vivado launch "Open Hardware Manager", then "Open Target->Auto Connect".
If you have a Digilent cable the setup is done.
Instead, if you have a Xilinx Platform Cable, its led will light up (its firmware has been loaded) but to use it you need to go back to the "Devices->USB devices" VirtualBox menu and select "XILINX". After that click "Cancel" on Hardware Manager and then once again the "Open Target".  Your cable setup is done.

