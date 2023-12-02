## 1. 라즈베리파이 업데이트

``` bash
sudo apt update
```
``` bash
sudo apt upgrade
```

## 2. 호스트팟 소프트웨어 설치

``` bash
sudo apt install hostapd dnsmasq
```

## 3. Hostapd 설정
- hostapd는 무선 액세스 포인트를 제공하는 데 사용됩니다.

``` bash
sudo nano /etc/hostapd/hostapd.conf
```
- 다음 설정을 추가하고 저장합니다.

``` plaintext
interface=wlan0
driver=nl80211
ssid=KoreaRB4       # 원하는 네트워크 이름
hw_mode=a
channel=36           # 원하는 무선 채널
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=koreasign00   # 원하는 무선 암호
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

## 4. Hostapd에게 설정 파일 사용하도록 알려주기:
``` bash
sudo nano /etc/default/hostapd
```
- 다음 줄을 수정합니다:

```plaintext 
DAEMON_CONF="/etc/hostapd/hostapd.conf"
```

## 5. Dnsmasq 설정

```bash
sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig
```
```bash
sudo nano /etc/dnsmasq.conf
```
- 다음 내용을 추가하고 저장합니다:

``` plaintext
interface=wlan0
dhcp-range=192.168.10.2,192.168.10.20,255.255.255.0,24h
```

## 6. 무선 인터페이스 설정

``` bash
sudo nano /etc/network/interfaces
```
- 다음을 추가하고 저장합니다:

``` plaintext
allow-hotplug wlan0
iface wlan0 inet static
    address 192.168.10.1
    netmask 255.255.255.0
    network 192.168.10.0
```

## 7. 무선 인터페이스 활성화

``` bash
sudo ifconfig wlan0 192.168.10.1
```

## 8. 서비스 재시작

``` bash
sudo service hostapd start
```
``` bash 
sudo service dnsmasq start
```