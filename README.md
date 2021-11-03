# Fail-operational
Fail-operational System

> # 1. Enable CAN on Nvidia Jetson Xavier (JetPack 4.5.1)
> https://medium.com/@ramin.nabati/enabling-can-on-nvidia-jetson-xavier-developer-kit-aaaa3c4d99c9
> ```
> sudo apt-get install busybox
> 
> sudo busybox devmem 0x0c303000 32 0x0000C400
> sudo busybox devmem 0x0c303008 32 0x0000C458
> sudo busybox devmem 0x0c303010 32 0x0000C400
> sudo busybox devmem 0x0c303018 32 0x0000C458
> 
> sudo modprobe can
> sudo modprobe can_raw
> sudo modprobe mttcan
> 
> sudo ip link set can0 type can bitrate 500000 dbitrate 2000000 berr-reporting on fd on
> sudo ip link set can1 type can bitrate 500000 dbitrate 2000000 berr-reporting on fd on
> 
> sudo ip link set up can0
> sudo ip link set up can1
> ```
> 
> ```
> touch /enable_CAN.sh
> chmod 755 /enable_CAN.sh
> ```
> ## /enable_CAN.sh
> ```
> #!/bin/bash
> sudo busybox devmem 0x0c303000 32 0x0000C400
> sudo busybox devmem 0x0c303008 32 0x0000C458
> sudo busybox devmem 0x0c303010 32 0x0000C400
> sudo busybox devmem 0x0c303018 32 0x0000C458
> sudo modprobe can
> sudo modprobe can_raw
> sudo modprobe mttcan
> sudo ip link set can0 type can bitrate 500000 dbitrate 2000000 berr-reporting on fd on
> sudo ip link set can1 type can bitrate 500000 dbitrate 2000000 berr-reporting on fd on
> sudo ip link set up can0
> sudo ip link set up can1
> 
> exit 0
> ```
> 
> ```
> printf '%s\n' '#!/bin/bash' 'exit 0' | sudo tee -a /etc/rc.local
> sudo chmod +x /etc/rc.local
> ```
> 
> ## /etc/rc.local
> ```
> #!/bin/bash
> sh /enable_CAN.sh &
> exit 0
> ```
> 
># 2.1 ZeroMQ (zmqpp)
>##Install and Build of ZeroMQ for cpp
>http://github.com/zeromq/zmqpp
>~~~
>git clone git://github.com/jedisct1/libsodium.git
>cd libsodium
>./autogen.sh 
>./configure && make check 
>sudo make install 
>sudo ldconfig
>cd ../
># Build, check, and install the latest version of ZeroMQ
>git clone git://github.com/zeromq/libzmq.git
>cd libzmq
>./autogen.sh 
>./configure --with-libsodium && make
>sudo make install
>sudo ldconfig
>cd ../
># Now install ZMQPP
>git clone git://github.com/zeromq/zmqpp.git
>cd zmqpp
>make
>make check
>sudo make install
>make installcheck
>~~~
>##Environment setup
>-Setup the path
>~~~
>sudo cp -R /usr/local/lib/* /usr/lib
>~~~
