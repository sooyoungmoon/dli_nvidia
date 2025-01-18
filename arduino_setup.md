
## Install Java for ARM64  
```bash
chat@chat-desktop:~$sudo apt update
chat@chat-desktop:~$sudo apt install openjdk-8-jdk
chat@chat-desktop:~$ wget https://downloads.arduino.cc/arduino-1.8.19-linuxaarch64.tar.xz
chat@chat-desktop:~$ tar -xf arduino-1.8.19-linuxaarch64.tar.xz
chat@chat-desktop:~$ cd arduino-1.8.19
chat@chat-desktop:~/arduino-1.8.19$ sudo ./install.sh
chat@chat-desktop:~/arduino-1.8.19$ sudo usermod -a -G dialout chat
chat@chat-desktop:~/arduino-1.8.19$ ls /dev/tty*
chat@chat-desktop:~/arduino-1.8.19$ sudo chmod a+rw /dev/ttyACM0  
chat@chat-desktop:~/arduino-1.8.19$ cd 
chat@chat-desktop:~$ arduino
```

## Install SimpleDHT11 library 

## Wiring

## Install Python 3.8 on Jetson Nano






