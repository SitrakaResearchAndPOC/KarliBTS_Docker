# CREATE CONTAINERS DOCKER FOR KarliBTS
## Hardware schematic
[demo1](https://www.youtube.com/watch?v=CebldhIqmDs) </br>
[demos2](https://www.youtube.com/watch?v=_nGVeG_76W8&pp=ygUJa2FybGkgZ3Nt) </br>
Hardware setup 1 : Need battery and not programmable with arduino  

[serial_cable](https://osmocom.org/projects/baseband/wiki/Serial_Cable)    [smartspate](https://www.smartspate.com/how-to-create-2g-network-at-your-own-home/)   [sudonull](https://sudonull.com/post/69473-Launching-a-GSM-network-at-home-Pentestit-Blog)
<p align="center">
  <img src="https://github.com/SitrakaResearchAndPOC/GSM_IMSICATCHER_HALFMITM_SPOOFING-SMS-WITH-PHYSICAL-MS/blob/main/usb_motorola.jpg">
</p>

<p align="center">
  <img width="600" height="400" src="https://github.com/SitrakaResearchAndPOC/GSM_IMSICATCHER_HALFMITM_SPOOFING-SMS-WITH-PHYSICAL-MS/blob/main/usb_cable_final.png">
</p>


Hardware setup 2 : No need battery and programmable with arduino  
<p align="center">
  <img src="https://github.com/SitrakaResearchAndPOC/GSM_IMSICATCHER_HALFMITM_SPOOFING-SMS-WITH-PHYSICAL-MS/blob/main/USB_TTL.jpg">
</p>

# INSTALLATION OF DOCKER KARLIBTS
## preparing docker 
```
docker pull debian:buster
```
```
docker run -tid --privileged -v /dev/bus/usb:/dev/bus/usb -v /dev:/dev -v /tmp/.X11-unix:/tmp/.X11-unix:ro -v $XAUTHORITY:/home/user/.Xauthority:ro --net=host --env="DISPLAY=$DISPLAY" --env="LC_ALL=C.UTF-8" --env="LANG=C.UTF-8" --name karlibts debian:buster
```
```
xhost +
```
```
docker exec -ti karlibts /bin/bash
```
```
apt update
```
```
apt install nano 
```
```
nano /etc/apt/source.list
```
Change apt file
```
#

# deb cdrom:[Debian GNU/Linux 10.13.0 _Buster_ - Official amd64 DVD Binary-1 20220910-18:04]/ buster contrib main

#deb cdrom:[Debian GNU/Linux 10.13.0 _Buster_ - Official amd64 DVD Binary-1 20220910-18:04]/ buster contrib main

# Line commented out by installer because it failed to verify:
#deb http://security.debian.org/debian-security buster/updates main contrib
# Line commented out by installer because it failed to verify:
#deb-src http://security.debian.org/debian-security buster/updates main contrib

# buster-updates, previously known as 'volatile'
# A network mirror was not selected during install.  The following entries
# are provided as examples, but you should amend them as appropriate
# for your mirror of choice.
#
 deb http://deb.debian.org/debian/ buster main contrib
 deb-src http://deb.debian.org/debian/ buster main contrib
```
``` 
apt update
 ```

## installing depencies
```
apt install libtool shtool automake dahdi-source libssl-dev sqlite3 libsqlite3-dev libsctp-dev libfftw3-dev libfftw3-3 autoconf libsctp-dev libgnutls28-dev libcurl4-gnutls-dev git-core pkg-config make gcc gcc-arm-none-eabi doxygen libtalloc-dev libpcsclite-dev libusb-1.0-0-dev
```
``` 
apt install autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool g++ git inetutils-tools libboost-all-dev libncurses5 libncurses5-dev libusb-1.0-0 libusb-1.0-0-dev libusb-dev python3-dev python3-mako python3-numpy python3-requests python3-scipy python3-setuptools python3-ruamel.yaml 
```
```
apt install nano wget
```
## creating and installing osmocom
```
mkdir osmocom
```
```
cd osmocom
```
installing libosomcore
```
git clone https://git.osmocom.org/libosmocore.git
```
```
cd libosmocore/
```
```
git checkout cf70aa0c40c574c32b832454f725cc37459c5d8d
```
```
autoreconf -i
```
```
./configure
```
```
make -j4
```
```
make install
```
```
ldconfig -i
```
```
nano target/firmware/Makefile
```
```
tail -f target/firmware/Makefile
```
```
make -j4 HOST_layer23_CONFARGS=--enable-transceiver -e CROSS_TOOL_PREFIX=arm-none-eabi-
```
```
cd ../..
```
installing dependencies before libosmo-abis
```
apt-get install libortp-dev
```
```
apt-get install libtool shtool automake dahdi-source libssl-dev sqlite3 libsqlite3-dev libsctp-dev libfftw3-dev libfftw3-3 autoconf libsctp-dev libgnutls28-dev libcurl4-gnutls-dev git-core pkg-config make doxygen libtalloc-dev libpcsclite-dev libusb-1.0-0-dev
```
installing libosmo-abis
```
git clone https://git.osmocom.org/libosmo-abis.git
```
```
cd libosmo-abis/
```
```
git checkout 39dffb6c29a8d78ba8527aa4ccc13f34d1c3b319
```
```
autoreconf -i
```
```
./configure
```
```
make -j4
```
```
make install
```
```
ldconfig
```
```
cd ..
```
installing libosmo-netif
```
git clone https://git.osmocom.org/libosmo-netif.git
```
```
cd libosmo-netif/
```
```
git checkout 09c71b04f5a8d82515d0d4d541b8368b585dbd31
```
```
autoreconf -i
```
```
./configure
```
```
make -j4
```
```
make install
```
```
ldconfig
```
```
cd ..
```
installing dependecies for openbsc
```
apt install build-essential gcc g++ make automake autoconf libtool pkg-config libtalloc-dev libpcsclite-dev libortp-dev libsctp-dev libssl-dev libdbi-dev libdbd-sqlite3 libsqlite3-dev libpcap-dev libc-ares-dev libgnutls28-dev libsctp-dev sqlite3 libsofia-sip-ua-glib-dev libusb-1.0-0-dev libfftw3-dev libgsm1-dev
```
```
apt install autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool g++ git inetutils-tools libboost-all-dev libncurses5 libncurses5-dev libusb-1.0-0 libusb-1.0-0-dev libusb-dev python3-dev python3-mako python3-numpy python3-requests python3-scipy python3-setuptools python3-ruamel.yaml
```
```
apt install asterisk telnet python3-pip
```
installing open-bsc
```
git clone https://git.osmocom.org/openbsc.git
```
```
cd openbsc/openbsc/
```
```
git checkout d2550da76f9974bb1957f74c5d3eb75fdae923d9
```
```
autoreconf -i
```
```
./configure
```
```
make -j4
```
```
make install
```
```
ldconfig
```
```
cd ../..
```
installing osmo-bts
```
git clone https://git.osmocom.org/osmo-bts.git
```
```
cd osmo-bts/
```
```
git checkout 59e7773055335a12d749faf84d88a8ed9fa0f201
```
```
autoreconf -i
```
```
./configure --enable-trx
```
```
make -j4
```
```
make install
```
```
ldconfig
```
```
cd ../..
```
Copying osmocon
```
cp osmocom/trx/src/host/osmocon/osmocon /
```
```
chmod +x osmocon
```
Copying trx
```
cp osmocom/trx/src/target/firmware/board/compal_e88/trx.highram.bin /
```
## Before launching preparing all configs
config file of osmo-bts
```
wget https://raw.githubusercontent.com/SitrakaResearchAndPOC/CalypsoBTS_Debian/main/osmo-bts.cfg
```
config file of open-bsc
```
wget https://raw.githubusercontent.com/SitrakaResearchAndPOC/CalypsoBTS_Debian/main/open-bsc.cfg
```
```
config hlr.sqlite3
```
```
touch hlr.sqlite3
```
```
apt install netcat wireshark
```
```
exit
```
## Launching osmocom for one phone (SMS ONLY)
Terminal 1
```
docker exec -ti karlibts ./osmocom/trx/src/host/osmocon/osmocon -m c123xor -p /dev/ttyUSB0 -c osmocom/trx/src/target/firmware/board/compal_e88/trx.highram.bin
```
or Terminal 1
```
docker exec -ti karlibts ./osmocon -m c123xor -p /dev/ttyUSB0 -c trx.highram.bin
```
Tape ctrl+shift+T </br>
Find ARFCN </br>
</br>
Tape `*#*#4636#*#*` and choose GSM only on your Android phone </br>
Installing network signal guru on your android phone </br>
And finding the arfcn that this one is connect </br>
Let's name this arfcn as ARFCN </br>
</br>

Terminal 2
```
docker exec -ti karlibts ./osmocom/trx/src/host/layer23/src/transceiver/transceiver -a ARFCN 
```
Tape ctrl+shift+T </br>
Terminal 3 </br>
```
docker exec -ti karlibts osmo-nitb -c open-bsc.cfg -l hlr.sqlite3 -P -C --debug=DRLL:DCC:DMM:DRR:DRSL:DNM
```
Tape ctrl+shift+T </br>
Terminal 4
```
docker exec -ti karlibts osmo-bts-trx -c osmo-bts.cfg --debug DRSL:DOML:DLAPDM -i 127.0.0.1
```
```
xhost +
```
```
docker exec -ti karlibts wireshark -k -f udp -Y gsmtap -i lo
```
or 
```
nc -u -l -p 4729 > /dev/null & wireshark -k -i lo -f 'port 4729'
```
find gsmtap, gsm_sms , gsm_sms_ud

## Launching osmocom for two phones (SMS AND CALL)
Terminal 1
```
docker exec -ti karlibts ./osmocom/trx/src/host/osmocon/osmocon -m c123xor -p /dev/ttyUSB0 -c osmocom/trx/src/target/firmware/board/compal_e88/trx.highram.bin
```
or Terminal 1
```
docker exec -ti karlibts ./osmocon -m c123xor -p /dev/ttyUSB0 -c trx.highram.bin
```
Tape ctrl+shift+T </br>
Terminal 2
```
docker exec -ti karlibts ./osmocom/trx/src/host/osmocon/osmocon -m c123xor -p /dev/ttyUSB1 -s /tmp/osmocom_l2.2 -c osmocom/trx/src/target/firmware/board/compal_e88/trx.highram.bin 
```
or Terminal 2
```
docker exec -ti karlibts ./osmocon -m c123xor -p /dev/ttyUSB1 -s /tmp/osmocom_l2.2 -c trx.highram.bin 
```
Tape ctrl+shift+T </br>
Find ARFCN </br>
Tape `*#*#4636#*#*` and choose GSM only on your Android phone </br>
Installing network signal guru on your android phone </br>
And finding the arfcn that this one is connect </br>
Let's name this arfcn as ARFCN </br>
Terminal 3 </br>
```
docker exec -ti karlibts ./osmocom/trx/src/host/layer23/src/transceiver/transceiver -a ARFCN -2
```
Tape ctrl+shift+T </br>
Terminal 4
```
docker exec -ti karlibts osmo-nitb -c open-bsc.cfg -l hlr.sqlite3 -P -C --debug=DRLL:DCC:DMM:DRR:DRSL:DNM
```
Tape ctrl+shift+T </br>
Terminal 5
```
docker exec -ti karlibts  osmo-bts-trx -c osmo-bts.cfg --debug DRSL:DOML:DLAPDM -i 127.0.0.1
```
```
xhost +
```
```
docker exec -ti karlibts wireshark -k -f udp -Y gsmtap -i lo
```
## Code managing BTS
```
docker exec -ti karlibts telnet localhost 4242
```
```
docker exec -ti karlibts telnet localhost 4241
```

## Running spoofing extension 
Tape ctrl+shit+T
```
docker  exec -ti  karlibts bash
```
```
wget https://raw.githubusercontent.com/SitrakaResearchAndPOC/nitb-script-all/main/osmo-nitb-scripts.zip
```
```
apt-get install zip
```
```
unzip osmo-nitb-scripts.zip
```
```
rm -rf osmo-nitb-scripts.zip
```
```
mv osmo-nitb-scripts/scripts_spoof1 /
```
```
mv osmo-nitb-scripts/scripts_spoof2 /
```
## Spoof script1 modification
```
cd  scripts_spoof1
```
```
nano finding_imsi_extenstion.sh
```
Change sqlite file as following /hlr.sqlite3
```
bash finding_imsi_extenstion.sh
```
```
nano delete_all.sh
```
Change sqlite file as following /hlr.sqlite3
```
nano set_imsi_extension.sh
```
Change sqlite file as following /hlr.sqlite3
```
nano sending_sms_broadcast.py
```
Change sqlite file as following /hlr.sqlite3
```
exit
```
## Spoof script2 modification
```
docker exec -ti karlibts  bash
```
```
cd  scripts_spoof2
```
```
nano show_subscribers.py
```
Change sqlite file as following /hlr.sqlite3
```
nano sms_broadcast.py
```
Change sqlite file as following /hlr.sqlite3
```
nano sms_send_source_dest_msg.py
```
Change sqlite file as following /hlr.sqlite3
```
exit
```
## Spoof script2 remark
```
docker exec -ti karlibts nano scripts_spoof2/sms_send_source_dest_msg.py 
```
Before launching sms_send_source_dest_msg.py , please corret the help, change : </br>
</br>
usage: ./sms_broadcast.py extension message </br>
This script sends a message from the specified extension (number) to all devices connected to this base station </br>
</br>
```
docker exec -ti karlibts ./scripts_spoof2/sms_broadcast.py 
```
to </br>
</br>
usage: ./sms_send_source_dest_msg.py  extension_source extension_destination  message </br>
This script sends a message from the specified extension source (number) to extension destination connected to this base station </br>
</br>
```
docker exec -ti karlibts ./scripts_spoof2/sms_send_source_dest_msg.py 
```
```
exit
```
## Test spoofing1
```
docker exec -ti karlibts scripts_spoof1/delete_all.sh
```
```
docker exec -ti karlibts scripts_spoof1/finding_imsi_extenstion.sh
```
Change with the following imsi
```
docker exec -ti karlibts scripts_spoof1/set_imsi_extension.sh 646040227957426 0341220590
```
```
docker exec -ti karlibts python2 scripts_spoof1/sending_sms_spoof_byextension.py  
```
```
docker exec -ti karlibts python2 scripts_spoof1/sending_sms_broadcast.py 
```
## Test spoofing2
```
docker exec -ti karlibts python2 scripts_spoof2/show_subscribers.py
```
```
docker exec -ti karlibts python2 scripts_spoof2/sms_broadcast.py 0341220590 'alert corona'
```
run also,
```
docker exec -ti karlibts python2 scripts_spoof2/sms_broadcast.py 0341220599 'alert corona'
```
```
docker exec -ti karlibts python2 scripts_spoof2/sms_send_source_dest_msg.py 0341220590 0341220590 "alert corona v2"
```
```
docker exec -ti karlibts python2 scripts_spoof2/sms_spam.py 0341220590 3 "alert corona 3"
```
## Exporting image
```
docker ps
```
getting the <id>
</br>
```
docker commit <id> karlibts
```
```
docker save karlibts > karlibts.tar.gz
```
# LOAD AN RUN NUMBER 1 (SPOOFING SMS ONLY) 
## Preparing images
```
docker load < karlibts.tar.gz
```
```
docker run -tid --privileged -v /dev/bus/usb:/dev/bus/usb -v /dev:/dev -v /tmp/.X11-unix:/tmp/.X11-unix:ro -v $XAUTHORITY:/home/user/.Xauthority:ro --net=host --env="DISPLAY=$DISPLAY" --env="LC_ALL=C.UTF-8" --env="LANG=C.UTF-8" --name karlibts karlibts
```
## Verify the usb port
dmesg | grep ttyUSB*

## Restart docker
```
docker restart karlibts 
```
## Deleting database
```
docker exec -ti karlibts scripts_spoof1/delete_all.sh
```
## launching osmocom (SMS ONLY)
Terminal 1
```
docker exec -ti karlibts ./osmocom/trx/src/host/osmocon/osmocon -m c123xor -p /dev/ttyUSB0 -c osmocom/trx/src/target/firmware/board/compal_e88/trx.highram.bin
```
or Terminal 1
```
docker exec -ti karlibts ./osmocon -m c123xor -p /dev/ttyUSB0 -c trx.highram.bin
```
Tape ctrl+shift+T </br>
Find ARFCN </br>
</br>
Tape `*#*#4636#*#*` and choose GSM only on your Android phone </br>
Installing network signal guru on your android phone </br>
And finding the arfcn that this one is connect </br>
Let's name this arfcn as ARFCN </br>
</br>
Terminal 2
```
docker exec -ti karlibts ./osmocom/trx/src/host/layer23/src/transceiver/transceiver -a ARFCN 
```
Tape ctrl+shift+T </br>
Terminal 3
```
docker exec -ti karlibts osmo-nitb -c open-bsc.cfg -l hlr.sqlite3 -P -C --debug=DRLL:DCC:DMM:DRR:DRSL:DNM
```
Tape ctrl+shift+T </br>
Terminal 4
```
docker exec -ti karlibts osmo-bts-trx -c osmo-bts.cfg --debug DRSL:DOML:DLAPDM -i 127.0.0.1
```
Terminal 5 </br>
Tape ctrl+shift+T
```
xhost +
```
```
docker exec -ti karlibts wireshark -k -f udp -Y gsmtap -i lo
```
or (not more powerfull for bts, good for ms) 
```
nc -u -l -p 4729 > /dev/null & wireshark -k -i lo -f 'port 4729'
```
find gsmtap, gsm_sms , gsm_sms_ud
## Code managing BTS
```
docker exec -ti karlibts telnet localhost 4242
```
```
docker exec -ti karlibts telnet localhost 4241
```

## Testing spoof1
```
docker exec -ti karlibts scripts_spoof1/finding_imsi_extenstion.sh
```
Take <imsi>  </br>
</br>
Change with the following imsi </br>
```
docker exec -ti karlibts scripts_spoof1/set_imsi_extension.sh <imsi> 0341220590
```
```
docker exec -ti karlibts scripts_spoof1/finding_imsi_extenstion.sh
```
```
docker exec -ti karlibts python2 scripts_spoof1/sending_sms_spoof_byextension.py  
```
```
docker exec -ti karlibts python2 scripts_spoof1/sending_sms_broadcast.py 
```
## Testing spoof2
```
docker exec -ti karlibts python2 scripts_spoof2/show_subscribers.py
```
```
docker exec -ti karlibts python2 scripts_spoof2/sms_broadcast.py 0341220590 'alert corona'
```
run also,
```
docker exec -ti karlibts python2 scripts_spoof2/sms_broadcast.py 0341220599 'alert corona'
```
# Verify the virtual TMSI (none)
```
docker exec -ti karlibts python2 scripts_spoof2/show_subscribers.py
```
```
docker exec -ti karlibts python2 scripts_spoof2/sms_send_source_dest_msg.py 0341220590 0341220590 "alert corona v2"
```
```
docker exec -ti karlibts python2 scripts_spoof2/sms_spam.py 0341220590 3 "alert corona 3"
```
# LOAD AN RUN NUMBER 2 (SPOOFING SMS AND CALL) 
## Preparing docker
```
docker load < karlibts.tar.gz
```
```
docker run -tid --privileged -v /dev/bus/usb:/dev/bus/usb -v /dev:/dev -v /tmp/.X11-unix:/tmp/.X11-unix:ro -v $XAUTHORITY:/home/user/.Xauthority:ro --net=host --env="DISPLAY=$DISPLAY" --env="LC_ALL=C.UTF-8" --env="LANG=C.UTF-8" --name karlibts karlibts
```
## Finding the port of the phone
dmesg | grep ttyUSB*
## Resarting docker
```
docker restart karlibts 
```
## Deleting database
```
docker exec -ti karlibts scripts_spoof1/delete_all.sh
```
## Launching osmocom for two phones (SMS and CALLS)
Terminal 1
```
docker exec -ti karlibts ./osmocom/trx/src/host/osmocon/osmocon -m c123xor -p /dev/ttyUSB0 -c osmocom/trx/src/target/firmware/board/compal_e88/trx.highram.bin
```
or Terminal 1
```
docker exec -ti karlibts ./osmocon -m c123xor -p /dev/ttyUSB0 -c trx.highram.bin
```
Tape ctrl+shift+T </br>
Terminal 2
```
docker exec -ti karlibts ./osmocom/trx/src/host/osmocon/osmocon -m c123xor -p /dev/ttyUSB1 -s /tmp/osmocom_l2.2 -c osmocom/trx/src/target/firmware/board/compal_e88/trx.highram.bin 
```
or Terminal 2
```
docker exec -ti karlibts ./osmocon -m c123xor -p /dev/ttyUSB1 -s /tmp/osmocom_l2.2 -c trx.highram.bin 
```
Tape ctrl+shift+T </br>
Find ARFCN </br>
</br>
Tape `*#*#4636#*#*` and choose GSM only on your Android phone </br>
Installing network signal guru on your android phone </br>
And finding the arfcn that this one is connect </br>
Let's name this arfcn as ARFCN </br
Terminal 3
```
docker exec -ti karlibts ./osmocom/trx/src/host/layer23/src/transceiver/transceiver -a ARFCN -2
```
Tape ctrl+shift+T </br>
Terminal 4
```
docker exec -ti karlibts osmo-nitb -c open-bsc.cfg -l hlr.sqlite3 -P -C --debug=DRLL:DCC:DMM:DRR:DRSL:DNM
```
Tape ctrl+shift+T </br>
Terminal 5
```
docker exec -ti karlibts  osmo-bts-trx -c osmo-bts.cfg --debug DRSL:DOML:DLAPDM -i 127.0.0.1
```
Terminal 5 </br>
Tape ctrl+shift+T
```
xhost +
```
```
docker exec -ti karlibts wireshark -k -f udp -Y gsmtap -i lo
```
# Code managing the BTS
```
docker exec -ti karlibts localhost 4242
```
```
docker exec -ti karlibts telnet localhost 4241
```

# Testing calls
```
docker exec -ti karlibts scripts_spoof1/finding_imsi_extenstion.sh
```
Take <imsi> </br>
</br>
```
docker exec -ti karlibts scripts_spoof1/set_imsi_extension.sh <imsi> 0341220590
```
# Verifying the assocation of IMSI and Extension (MSISDN)
```
docker exec -ti karlibts scripts_spoof1/finding_imsi_extenstion.sh
```
Making the spoof call


## Testing spoof1
```
docker exec -ti karlibts scripts_spoof1/finding_imsi_extenstion.sh
```
Take <imsi> </br>
</br>
Change with the following imsi </br>
</br>
```
docker exec -ti karlibts scripts_spoof1/set_imsi_extension.sh <imsi> 0341220590
```
```
docker exec -ti karlibts scripts_spoof1/finding_imsi_extenstion.sh
```
```
docker exec -ti karlibts python2 scripts_spoof1/sending_sms_spoof_byextension.py  
```
```
docker exec -ti karlibts python2 scripts_spoof1/sending_sms_broadcast.py 
```
## Testing spoff2
```
docker exec -ti karlibts python2 scripts_spoof2/show_subscribers.py
```
```
docker exec -ti karlibts python2 scripts_spoof2/sms_broadcast.py 0341220590 'alert corona'
```
run also,
```
docker exec -ti karlibts python2 scripts_spoof2/sms_broadcast.py 0341220599 'alert corona'
```

# verifying no TMSI for virtual number (none)
```
docker exec -ti karlibts python2 scripts_spoof2/show_subscribers.py
```
```
docker exec -ti karlibts python2 scripts_spoof2/sms_send_source_dest_msg.py 0341220590 0341220590 "alert corona v2"
```
```
docker exec -ti karlibts python2 scripts_spoof2/sms_spam.py 0341220590 3 "alert corona 3"
```
