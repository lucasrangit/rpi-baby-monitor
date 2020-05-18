# Hardware

* INMP441 MEMS I2S Mono Microphone
   * https://invensense.tdk.com/wp-content/uploads/2015/02/INMP441.pdf
* Raspberry Pi 2B, Zero W
* Raspberry Pi CSI Camera 1.3
* Raspberry Pi IR Cut Filter 5MP OV5647 CSI Camera
   * https://www.arducam.com/product/arducam-m12-night-vision-ir-cut-raspberry-pi-camera/

## Setup

* pinout: https://github.com/nejohnson2/rpi-i2s/blob/master/rpi-pins.png

# Software

## Setup Camera

1. `sudo raspi-config`
1. Enable Camera

## Setup UV4L

Reference: https://www.linux-projects.org/uv4l/webrtc-extension/

1. `curl http://www.linux-projects.org/listing/uv4l_repo/lpkey.asc | sudo apt-key add -`
1. `sudo apt-get install uv4l uv4l-raspicam uv4l-server uv4l-webrtc uv4l-raspicam-extras`

## Setup Audio Controller

1. `sudo vim /boot/config.txt`
1. Uncomment `dtoverlay=i2s=on`
1. `sudo reboot`

## Setup Audio Microphone

Reference: https://learn.adafruit.com/adafruit-i2s-mems-microphone-breakout?view=all#raspberry-pi-wiring-test

1. `git clone https://github.com/adafruit/Raspberry-Pi-Installer-Scripts.git`
1. `sudo apt install git build-essential dkms raspberrypi-kernel-headers`
1. `cd Raspberry-Pi-Installer-Scripts/i2s_mic_module/`
1. `sudo cp -R . /usr/src/snd-i2s_rpi-0.1.0`
1. `sudo dkms add -m snd-i2s_rpi -v 0.1.0`
1. `sudo dkms build -m snd-i2s_rpi -v 0.1.0`
1. `sudo dkms install -m snd-i2s_rpi -v 0.1.0`
1. `sudo vim /etc/modprobe.d/snd-i2smic-rpi.conf`
1. Append `snd-i2smic-rpi rpi_platform_generation=1`
1. `echo "snd-i2smic-rpi" | sudo tee /etc/modules-load.d/snd-i2smic-rpi.conf`
1. `vim ~/.asoundrc`
1. See guide for contents
1. `sudo reboot`

## Test Audio Recording

1. `arecord -D dmic_sv -c2 -r 44100 -f S32_LE -t wav -V stereo -v file.wav`

# References

1. https://www.raspberrypi.org/forums/viewtopic.php?t=173640
1. https://www.alsa-project.org/pipermail/alsa-devel/2015-August/096402.html
1. https://github.com/nejohnson2/rpi-i2s
1. https://github.com/opencardev/snd-i2s_rpi
1. https://forums.adafruit.com/viewtopic.php?f=19&t=117687&p=604270#p604270
1. https://github.com/adafruit/Raspberry-Pi-Installer-Scripts/tree/master/i2s_mic_module

