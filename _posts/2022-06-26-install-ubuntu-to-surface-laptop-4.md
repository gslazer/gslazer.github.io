---
title: "서피스 랩탑4에 우분투 설치하기"
date: 2022-06-22 06:08:00 -0500
categories: surface-laptop-4 linux-surface ubuntu
---
>linux-surface github : https://github.com/linux-surface
>Installation and Setup page : https://github.com/linux-surface/linux-surface/wiki/Installation-and-Setup#Debian--Ubuntu
>Surface Laptop4 Page https://github.com/linux-surface/linux-surface/wiki/Surface-Laptop-4

처음에도 고생했지만 꺠끗하게 끝내겠다고 한 세번 네번 다시했는데 할때마다 뭔가 석연치가 않았다 ^^;;
언젠가 다시 세팅할일이 있을지 모르곘는데, 혹시라도 그러려면 다 까먹고 다시 고생할것 같아서 남기기로.

대부분 과정은 [linux-surface 깃허브][linux-surface github]에서 잘 정리해 놓은 [linux-surface 설치 페이지][linux-surface Installation and Setup page] 대로 진행하면 되는데, [서피스 랩탑4 페이지][linux-surface Surface Laptop4 Page]을 참고하여 몇가지를 끼워넣으면 된다.

그럼 서피스 랩탐4(라이젠)에 ubuntu 20.04.4 를 설치하는 순서를 정리해보겠다.


# 우분투 설치

1. 설치 준비
    우분투 설치USB를 만든다.
    스페어 usb 키보드와 마우스를 준비한다. 설치 과정에서 키보드와 터치패드가 먹통이 되기때문에 반드시 필요하다.
    (서피스 랩탑4는 'USB A 포트가 단 한개'라서 필연적으로 usb허브 등의 추가장비도 필요하다.)

2. 윈도우 파티션 축소
    멀티부팅으로 세팅할 생각이라면 필요하다.

    >Go to Control Panel -> System and Security -> Administrative Tools -> Computer Management -> Storage -> Disk Management.

    >Right click on the Windows partition and shrink the volume as much as you'd like (a minimum of 50 GB is recommended).

    나는 가장 저렴한 모델을 구매해서 ssd용량에 여유도 없고 리눅스 전용으로 사용할 생각이라 스킵.

3. 보안 부팅 비활성화(disable)
    우분투는 secure boot를 지원하니까 비활성화 할 필요가 없는것처럼 적혀있지만..
    아무튼 비활성화하지않으면 USB부트로 진입할 수 없어서 해야된다. 
    [보안 부팅 비활성화하기][disable secure boot]

4. UEFI에 진입하여 USB 부팅 설정 

5. 우분투 설치하기
    아마도 여기 혹은 이 다음과정부터 서피스랩탑의 키보드와 터치패드가 동작하지 않게 되니 준비한 마우스와 키보드를 사용하면 된다.
    - 서피스 랩탑 4 라이젠 모델에서는 후술할 IOMMU error로 인해 화면이 먿어버리는 현상이 있는데, 이게 설치용 USB로 부팅시에도 발생한다. 이 단계에서는 부팅메뉴에서 디스플레이 옵션을 최소화한 안전부팅을 사용하여 설치를 진행한다.


# 디스플레이 프리징 문제 해결

    [linux-surface 설치 페이지][linux-surface Installation and Setup page]에는 문제시되어있지 않지만(아마 다른 서피스 시리즈에서는 별 탈이 없는 듯 하다), 서피스 랩탑 4 에서는 직전에 적은 디스플레이가 굳어버리는 IOMMU 문제가 여기서 발목을 잡는다. 우분투 설치가 이후 커널 설치까지 완료되어도 해당 문제를 해결하지않으면 모니터는 사용할 수 없기 때문에 IOMMU 오류부터 해결하고 넘어간다. [서피스 랩탑4 페이지][linux-surface Surface Laptop4 Page]에 작성된 해결 방식을 따라간다.

1. Grub진입
    부팅할떄 시프트 키를 누르고 있으면 서피스 로고 후에 Grub으로 진입한다.
    만약 부팅 메뉴에 진입할 수 없다면, [grub 복구][restore-grub]를 참조하여 복구 후에 진행하자.

2. 부팅 옵션 적용
    부팅메뉴에서 e를 눌러 당장 부팅하기 위한 부팅옵션을 추가한다.
    'quiet splash' 뒤에 ` amd_iommu=off iommu=off'를 붙여주고, ctrl+x로 부팅하면 화면이 먿지 않고 부팅 가능할 것이다.
    * nomodeset이나  amd_iommu=force_isolation도 시도해봤지만 몇가지 문제가 발생한다.(밝기조절 불가, 해상도조절 불가) 얌전히 위의 옵션만 적용하자

3. Grub 파일 수정
    당장 부팅에 성공 했지만 2.의 부팅옵션은 지속되지 않는 방법이기떄문에 Grub 파일을 편집해서 향후 적용될 부팅옵션을 수정해준다.

    ```
    $sudo nano /etc/default/grub
    ```

    'GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"' 를 다음과 같이 고쳐준다.

    ```
    GRUB_CMDLINE_LINUX_DEFAULT="quiet splash amd_iommu=off iommu=off acpi_backlight=vendor"
    ```

    'amd_iommu=off iommu=off'는 iommu 에러를 회피하기 위한 것이고
    'acpi_backlight=vendor'는 백라이트 설정이 동작하게 하기 위한 것이다. [서피스랩탑4 페이지][linux-surface Surface Laptop4 Page]

    ```
    $sudo update-grub
    ```
    변경한 사항을 적용하기 위해 grub을 업데이트해준다.


# 리눅스 커널 설치

거의 다 왔다. 나머지는 이제 [linux-surface 설치 페이지][linux-surface Installation and Setup page] 를 죽 따라가기만 하면 된다.

```
$ wget -qO - https://raw.githubusercontent.com/linux-surface/linux-surface/master/pkg/keys/surface.asc \
    | gpg --dearmor | sudo dd of=/etc/apt/trusted.gpg.d/linux-surface.gpg
```

```
$ echo "deb [arch=amd64] https://pkg.surfacelinux.com/debian release main" \
    | sudo tee /etc/apt/sources.list.d/linux-surface.list

$ sudo apt update
```
여기에서 문제가 발생한다면 [여기][error-401]를 참고.

```
$ sudo apt install linux-image-surface linux-headers-surface iptsd libwacom-surface
$ sudo systemctl enable iptsd
```

```
$ sudo apt install linux-surface-secureboot-mok
```

```
$ sudo update-grub
```

이것으로 귀하시고 고마우신 분들이 만든 linux-surface커널 설치가 완료되어 서피스랩탑4로 우분투를 사용할 준비가 완료되었다.
ubuntu를 사용한적이 없어서 여러모로 헤매면서 여기 저기를 뒤졌지만, 결국 [linux-surface 설치 페이지][linux-surface Installation and Setup page]와 [서피스 랩탑4 페이지][linux-surface Surface Laptop4 Page]에 모두 있는 내용이었고 이쪽에서 제시한 방법이 가장 깔끔했다. 저 두 페이지에서 순서 정도만 설치과정에 맞게 끼워맞추고, 우분투 유저라면 다들 알아서인지 자세히 적혀있지 안은 grub 설정 방법 같은것만 추가했다.


# 좀비 마우스커서 문제 해결

마지막으로 100% 발생하는 듯한 문제에 대해서 추가.
처음에는 깔끔하다가도 사용하다보면 로그인스크린에서의 마우스포인터가 화면에 그대로 남게 된다.
[Ubuntu 20.04 and 21.04: second cursor stuck on the screen 150% fractional scaling on a 1440p monitor after login (XOrg)][https://askubuntu.com/questions/1335496/ubuntu-20-04-and-21-04-second-cursor-stuck-on-the-screen-150-fractional-scalin]에서 해결을 찾았다.

'sudo nano /etc/gdm3/custom.conf'

'WaylandEnable=false'의 주석 제거 (#를 지움)

'sudo service gdm restart'

끝~

[linux-surface github]: https://github.com/linux-surface
[linux-surface Installation and Setup page]:   https://github.com/jekyll/jekyll
[restore-grub]: [https://gomsik.tistory.com/m/44]
[linux-surface Surface Laptop4 Page]: https://github.com/linux-surface/linux-surface/wiki/Surface-Laptop-4
[disable secure boot]: [https://surfacetip.com/disable-secure-boot-surface-laptop-4/]
[error-401]: [https://github.com/linux-surface/linux-surface/wiki/Known-Issues-and-FAQ#apt-update-fails-on-ubuntudebian-based-distributions-with-error-401-unauthorized]
