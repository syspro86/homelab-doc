# raspberrypi

보유중인 rpi1 을 위한 스크립트 모음

## 초기 세팅
오버클럭 등
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
sudo apt update
sudo apt install -y raspberrypi-kernel-headers build-essential bc dkms git

mkdir -p ~/src
cd ~/src
git clone https://github.com/morrownr/8821cu-20210916.git
cd 8821cu-20210916

sudo ./install-driver.sh
```

## wifi 연결
```
sudo nmcli radio wifi on
sudo nmcli dev wifi list
sudo nmcli dev wifi connect "<SSID>" password "<비밀번호>"
sudo nmcli device

sudo nmcli connection modify <CONNECTION_NAME> connection.autoconnect yes
sudo nmcli device set <DEVICE_NAME> autoconnect yes
sudo nmcli -f name,autoconnect connection

# wifi powersave 방지
# 방법1
sudo nmcli connection modify <CONNECTION_NAME> 802-11-wireless.powersave 2
# 방법2 
#sudo echo "[connection]" > /etc/NetworkManager/conf.d/powersave.conf
#sudo echo "wifi.powersave=off" >> /etc/NetworkManager/conf.d/powersave.conf
#sudo chmod 600 /etc/NetworkManager/conf.d/powersave.conf
# 방법3
#sudo iw dev wlan0 set power_save off
#sudo iw dev wlan0 get power_save
```
