## WSL2(Windows Subsystem for Linux) Settings

WSL2를 사용하기전 확인해야할 것들
- 가상화 여부
- Windows Version 2004
- Windows 설정에 개발자 모드로 변경 및 리눅스 서브시스템과 Hyper-V를 사용하도록 설정

글을 읽기 전에 알아야 할 사항.
- 이 글은 WSL2를 설치 및 개발 환경을 셋팅하는데 있어서 막혔던 부분이나, 에러를 만난 부분에 대해서만 해결 방법을 기술하고 있기 때문에 설치 과정은 생략한다.

### 커맨드라인으로 WSL2 설치하기 [파트](https://github.com/wslhub/wsl-firststep/blob/master/firststep/install.md)

```
wsl --set-default-version 2
```
위의 명령어를 실행할 경우 오류가 뜬다면, https://docs.microsoft.com/ko-kr/windows/wsl/wsl2-kernel 이 링크에서 커널 업데이트를 진행한 후 다시 해당 명령어를 실행해보자.

### Windows Terminal 설정하기 [파트](https://github.com/wslhub/wsl-firststep/blob/master/firststep/winterm.md)에서 zsh의 테마를 agnoster로 할 경우 폰트 깨지는 문제

- powerline 폰트를 https://medium.com/@slmeng/how-to-install-powerline-fonts-in-windows-b2eedecace58 해당 링크를 참고하여 설정한다.
  - 나의 경우 https://github.com/powerline/fonts 여기서 그냥 powerline 폰트를 다운받아서 압축을 풀고 폰트를 Windows의 fonts 폴더 안에 넣고 VSCode로 Windows Terminal의 fonts 설정을 했다.

### OpenJDK 설치하기 [파트](https://github.com/wslhub/wsl-firststep/blob/master/devsetup/openjdk.md)

```
$ sudo apt -y install openjdk-11-jdk
```
위의 명령어를 wsl2 ubuntu shell에서 실행한다.

자바 환경 변수 설정을 할 때 아래와 같이 하되, jvm 뒤에 폴더명을 실제 우분투 쉘에서 /usr/lib/jvm으로 접근하여 확인 후 확인한 폴더명으로 해야 뒤에 Gradle 설정을 할 때 JAVA_HOME을 찾을 수 없다는 에러가 발생하지 않는다.
```
export JAVA_HOME=/usr/lib/jvm/[실제 openjdk 폴더명]
export PATH=$PATH:$JAVA_HOME/bin
```

### gradle 설정 [파트](https://github.com/wslhub/wsl-firststep/blob/master/devsetup/openjdk.md)

- 위의 링크를 통해 gradle까지 깔았다면, source /etc/profile.d/gradle.sh 이 명령어를 ~/.zshrc에 맨 아랫 줄에 기입하고 이후 source ~/.zshrc를 실행한다.
- source명령어가 잘 처리되었다면 gradle -v로 확인한다.

node.js 설정은 [공식문서](https://docs.microsoft.com/ko-kr/windows/nodejs/setup-on-wsl2)를 참고한다.

### XServer를 사용하여 Linux GUI를 통한 IDE 개발 환경 구축

XServer를 사용하기 전 해야할 일.

일부 프로그램에서 /etc/machine-id 파일이 필요하다. 그리고 dbus-uuidgen을 사용하여 uuid를 생성해주어야 한다.

```
$ sudo systemd-machine-id-setup
$ sudo dbus-uuidgen -- ensure
$ cat /etc/machine-id
```

XServer를 사용할 때, 한글 입력 셋팅은 다음 [링크](https://sigmafelix.wordpress.com/2020/08/17/wsl2%ec%97%90%ec%84%9c-%ed%95%9c%ea%b8%80-%ec%9e%85%eb%a0%a5-%ec%82%ac%ec%9a%a9%ed%95%98%ea%b8%b0/comment-page-1/#comment-84)를 참고한다.
- 나의 경우 VcXsrv를 사용하여 xserver를 설치했다.

아래의 명령어는 zsh를 사용할 경우 ~/.zshrc에 넣어주자.
```
export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2; exit;}'):0.0
```

한글 입력 시 vi 편집기로 sh파일을 만들어주자.
```
$ sudo vi /etc/profile.d/fcitx.sh
```

아래는 만들어진 sh파일에 넣을 설정들이다.
```
#!/bin/zsh
export QT_IM_MODULE=fcitx
export GTK_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
export DefaultIMModule=fcitx
 
#optional
fcitx-autostart &>/dev/null
```

추가로 zsh를 사용할 경우 위에서 언급한 작업을 마친 후에도 적용이 안된다면, 터미널 상에서 fcitx를 한번 입력해보기 바란다.

### XServer를 사용하여 깔끔하게 GUI 실행하기

- [링크](https://medium.com/beyond-the-windows-korean-edition/wsl-2-x11-%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98%EC%9D%84-%EB%8D%94-%EA%B9%94%EB%81%94%ED%95%98%EA%B2%8C-%EC%8B%A4%ED%96%89%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-5a270835801c)를 참고하여 VcXsrv를 통한 GUI앱을 사용해보자.

### Intellij IDE를 설치할 경우

- wsl2의 경우 jetbrains-toolbox를 설치하거나, /opt 디렉토리에 버전 별로 설치하는 것을 추천한다.
- jetbrains-toolbox를 설치할 경우 wget이나 curl로 tar.gz파일로 toolbox를 받아오자.
  - 받아온 gz파일을 압축해제하고, jetbrains-toolbox 디렉토리로 이동하여 ./jetbrains-toolbox 명령어를 실행하자.
  - toolbox가 실행되면 원하는 IDE를 다운받고, IDE를 실행하여 JetBrains 계정 로그인을 진행하자. toolbox 로그인은 안타깝게도 되지 않는 것 같다.
- Intellij를 설치후 원활하게 terminal에서 명령어만으로 실행하고 싶을 때 [링크](https://hy.ne.kr/5)를 참고한다.

나의 경우는 toolbox로 설치하였기 때문에 아래와 같이 설정하였다.
```
echo 'alias idea="nohup /home/rebwon/.local/share/JetBrains/Toolbox/apps/IDEA-U/ch-0/202.6397.94/bin/idea.sh > /dev/null 2>&1 &"' >> ~/.zshrc
```

WSL2 디렉토리를 Windows 파일 관리자에서 편하게 접근하고 싶을 경우 해당 [링크](https://www.lesstif.com/software-architect/wsl-2-windows-subsystem-for-linux-2-89555812.html)를 참고한다.

WSL2를 사용하면서 메모리 점유율이 너무 높을 경우 Windows의 사용자 디렉토리에 .wslconfig 파일을 만들어서 아래와 같이 옵션을 설정한다.

나의 경우 윈도우가 아닌 WSL2를 메인 자바 개발 환경으로 사용할 것이기 때문에, 실제 내 놋북 리소스의 절반을 설정해주었다.
```
[wsl2]
memory=8GB
swap=0
localhostForwarding=true
```

메모리 점유 이외에 WSL2를 사용하면서 캐시 메모리가 증가하여 intellij나 다른 작업 시 너무 느려졌을 경우 아래의 명령어를 사용하자. (매번 이 명령어를 치기 귀찮다면, alias 설정을 해주면 된다.)
```
sudo sh -c "/usr/bin/echo 3 > /proc/sys/vm/drop_caches"
```

### VcXsrv의 흐릿한 그래픽 선명하게 하기

해당 [링크](https://blog.nadekon.net/115)를 참고하였다.

VcXsrv 설치 폴더로 들어가, vcxsrv.exe와 xlaunch.exe 두개 모두 속성 설정을 변경한다.

파일 우클릭 -> 속성 -> 호환성 -> 고 DPI 설정 변경 -> 고 DPI 스케일링 덮어쓰기 -> 응용프로그램 설정

위와 같이 설정 후 wsl2의 bashrc 혹은 zshrc를 수정한다.
```
export GDK_SCALE=화면배율
```

1920x1080 해상도인 나의 경우 배율을 2로 할 경우 Intellij IDE가 너무 크게 나왔기 때문에, 1 혹은 1.5로 설정할 것을 권장한다.