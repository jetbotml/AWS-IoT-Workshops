# Raspberry Pi SD Card steps for AWS IoT and DHT22 sensor


Download latest Raspberry Pi OS Lite from  https://www.raspberrypi.org/software/operating-systems/
2020-12-02-raspios-buster-armhf-lite.zip 

Use Etcher to flash image to an SD card - https://www.balena.io/etcher/

Once card is flashed, copy your wpa_supplicant.conf file to the boot drive of the new image. 

Txt file name must be wpa_supplicant.conf 

```bash
country=US
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
	
network={
	ssid="yourwifissid"
	psk="yourwifipassword"
	}
```

Boot up your raspberry pi
	Need a monitor, mouse and power
	
Log into your raspberry pi
	Default account
	Login: pi
	Default password: raspberry

Run raspi config to set password, hostname and turn on ssh. You can also set the wifi and password here too
sudo raspi-config 
	System options - set password and hostname
	Interface options - enable SSH
	Finish and reboot

After boot the display will show the IP of the pi.
You can also find it with this command
ifconfig   (ifconfig wlan0 will just show wifi connection info)

Connect to your raspberry pi using ssh to make sure you can. If not go back to sudo raspi-config and make sure ssh was enabled and the wifi is set correctly.

Shutdown the raspberry pi and disconnect the monitor and keyboard. Then reboot the pi
sudo shutdown now

After restarting your pi, ssh to you pi and enter the following commands - one at a time

```bash
export AWS_DEFAULT_REGION=<your preferred region, i.e. us-east-1>
export STACK_NAME=<a unique name for your CloudFormation stack>
./deploy.sh
```
```bash
sudo dpkg-reconfigure tzdata
```

```bash
sudo apt-get update
```

```bash
sudo apt-get upgrade -y
```

```bash
sudo apt-get install python3 git python3-pip libgpiod2 python3-gpiozero -y
```

```bash
sudo pip3 install RPI.GPIO adafruit-blinka adafruit-circuitpython-dht AWSIoTPythonSDK
```

```bash
sudo update-alternatives --install /usr/bin/python python $(which python2) 1
sudo update-alternatives --install /usr/bin/python python $(which python3) 2
sudo update-alternatives --config python
```



Test the setup run 
```bash
sudo python dht_simpletest.py
```
Next - Configure AWS IoT
