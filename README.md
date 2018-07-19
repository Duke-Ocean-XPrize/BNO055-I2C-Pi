# BNO055-I2C-Pi
This tutorial was made and tested using a Raspberry Pi Model B V1.2 running Raspbian Stretch, but should work with other versions of Raspbian.
Other versions of the Raspberry Pi may need to wire the circuit differently to correspond with the board's I2C pins.
Based on [ghirlekar's implementation](https://github.com/ghirlekar/bno055-python-i2c).

## Wiring

| BNO055 | Raspberry Pi|
| --- | ---: |
| Vin | 5V (pin 4, etc) |
| GND | GND (pin 6, etc) |
| SDA | GPIO2 (pin 3) |
| SCL | GPIO3 (pin 5) |

![Wiring image](https://github.com/Duke-Ocean-XPrize/BNO055-I2C-Pi/raw/master/images/wiring.jpg)

## Setup

Enable I2C by entering `sudo raspi-config`, selecting `5 Interfacing Options`, `P5 I2C`, and then enabling.

Run `sudo nano /etc/modules` and append the following lines to the file:

```
i2c-bcm2708 
i2c-dev```

Install the necessary Python modules by running

```
sudo apt-get install python-smbus
sudo apt-get install i2c-tools```

Reboot using `sudo reboot`.

Test the I2C connection by connecting the BNO055 as shown above and then running `sudo i2cdetect -y 1`. Old Pis might need to instead run `sudo i2cdetect -y 0`.

Because of an issue with clock stretching on the Raspberry Pi over I2C, the default I2C baudrate may result in unusable data or a failure to initialize communication with the BNO055. The issue is better described [here](https://github.com/ghirlekar/bno055-python-i2c/issues/1);

The current baudrate of the I2C bus can be found by running `sudo cat /sys/module/i2c_bcm2708/parameters/baudrate`

The issue can be fixed by changing this baudrate to 50kHz by running

```
sudo modprobe -r i2c_bcm2708
sudo modprobe i2c_bcm2708 baudrate=50000```