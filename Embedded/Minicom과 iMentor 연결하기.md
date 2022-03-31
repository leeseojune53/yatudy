# Minicom과 iMentor 연결하기

### 🎊 시작하기 전에...

이 글은 방화벽을 모두 비활성화하고 작성한 글입니다.

### ⚙ Minicom 설정

`sudo minicom -s`

위 명령어를 통해 Minicom 설정에 들어간다.

Serial port setup 탭에서

| Serial Device         | /dev/ttyUSB0 |
| --------------------- | ------------ |
| Bps/Par/Bits          | 115200 8N1   |
| Hardware Flow Control | No (F 클릭)  |

`Save setup as dfl`을 누르면 기본 설정으로 저장된다.

### 🔗 Minicom 연결

iMentor과 컴퓨터를 시리얼 연결한다. LAN도 연결해준다.

`sudo minicom`

위 명령어를 실행하고 iMentor의 전원을 켜준다.

### ⚙ Network 설정

VMWare의 Network Adapter를 Bridged로 변경해준다.

![VMWare Network Setting.png](https://github.com/leeseojune53/yatudy/blob/main/images/embedded/VMWare%20Network%20Setting.png?raw=true)

VMWare 내부에서 돌아가고있는 Ubuntu의 네트워크를 아래와 같이 설정해준다.

![Ubuntu Network Setting.png](https://github.com/leeseojune53/yatudy/blob/main/images/embedded/Ubuntu%20Network%20Setting.png?raw=true)

minicom에서 `ifconfig eth0 192.168.0.2`를 하고, Ubuntu의 Terminal에서 `ping 192.168.0.2`를 실행한다.