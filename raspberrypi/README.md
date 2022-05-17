# raspberrypi

보유중인 rpi1 을 위한 스크립트 모음

## 초기 세팅
오버클럭, wifi 연결 등
```
sudo raspi-config
```

## zsh
```
sudo apt install zsh
curl -L http://install.ohmyz.sh | sh
chsh -s `which zsh`

vi .zshrc
# 원하는 theme으로 수정한다
ZSH_THEME="agnoster"
```

## realtek usb wifi driver
```
sudo lsusb
Bus 001 Device 004: ID 0bda:c811 Realtek Semiconductor Corp. 802.11ac NIC

sudo apt install build-essential -y
mkdir -p ~/build
cd ~/build
sudo apt install git
git clone https://github.com/brektrou/rtl8821CU.git
cd rtl8821CU
make
sudo make install
```
