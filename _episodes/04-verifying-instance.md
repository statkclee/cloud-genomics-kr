---
title: "클라우드 설정 미세 조정하기"
teaching: 5
exercises: 10
questions:
- "본인 원격 컴퓨터가 제대로 환경이 설정되어 있는가?"
- "내가 자리를 비운 상태에서도 작업을 계속 수행시킬 방법은 무엇인가?"
objectives:
- "원격 컴퓨터의 가용 자원과 파일 시스템을 점검한다."
- "`tmux`를 사용해서 클라우드에서 백그라운드로 작업을 계속해서 돌린다."
keypoints:
- "항상 신규 인스턴스가 제대로 준비되었는지 점검한다."
- "`tmux`같은 프로그램을 사용해서 인터넷 연결이 좋지 않을 때도 작업을 계속해서 돌리게 한다."
---

# 원하는 클라우스 서비스인가요?

신규 생성된 원격 인스턴스에 접속하게 되면, 설정이 원하는 바대로 되었는지,
프로젝트를 시작하기 전에 모든 것이 제대로 동작하는지 두번 검정한다.


이번 워크샵은 시작되기 전에 강사가 모든 확인절차를 거쳐서 별도 점검이 필요없다.
하지만, 본인 스스로 인스턴스를 생성시키는 경우 중요한 기술적 자산이 된다.


## 연결상태 검증하기 

인스턴스에 연결되면, 환영 메시지가 스크린에 띄워진다.
데이터 카펜트리 아마존 인스턴스에 접속되면 다음 메시지가 화면에 보이게 된다:

~~~
Welcome to Ubuntu 14.04.3 LTS (GNU/Linux 3.13.0-48-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Sun Jan 24 21:38:35 UTC 2016

  System load:  0.0                Processes:           151
  Usage of /:   48.4% of 98.30GB   Users logged in:     0
  Memory usage: 6%                 IP address for eth0: 172.31.62.209
  Swap usage:   0%

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

12 packages can be updated.
10 updates are security updates.


Last login: Sun Jan 24 21:38:36 2016 from
~~~
{: .output}

명령을 기다리며 깜빡이는 커서도 보이게 된다.

~~~
$
~~~
{: .bash}

## 환경 점검하기

인스턴스에 연결이 되었기 때문에, 명령어 몇개를 던져 연결된 컴퓨터에 대해 살펴보자:


- `whoami`: 연결된 컴퓨터의 사용자명(`username`)을 보여준다.

  ~~~
  dcuser@ip-172-31-62-209 ~ $ whoami
  dcuser
  ~~~
  {: .output}

- `df -h`: 하드 디스크의 남은 공간을 보여준다.

  ~~~
  dcuser@ip-172-31-62-209 ~ $ df -h
  Filesystem      Size  Used Avail Use% Mounted on
  udev            2.0G   12K  2.0G   1% /dev
  tmpfs           396M  792K  395M   1% /run
  /dev/xvda1       99G   48G   47G  51% /
  none            4.0K     0  4.0K   0% /sys/fs/cgroup
  none            5.0M     0  5.0M   0% /run/lock
  none            2.0G  144K  2.0G   1% /run/shm
  none            100M   36K  100M   1% /run/user
  ~~~
  {: .output}

상기 `'Mounted on` 칼럼 아래 `/`인 행이 있다. 즉, `/dev/xvda1       99G   48G   47G  51% /`.
하드디스크에 대한 값을 저장용량을 나타내고 있다.


- `cat /proc/cpuinfo`: 컴퓨터가 보유하고 있는 중앙처리장치(CPU)에 대한 자세한 정보를 나타내고 있다.

  ~~~
  dcuser@ip-172-31-62-209 ~ $ cat /proc/cpuinfo
  processor  : 0
  vendor_id	: GenuineIntel
  cpu family	: 6
  model		: 62
  model name	: Intel(R) Xeon(R) CPU E5-2670 v2 @ 2.50GHz
  stepping	: 4
  microcode	: 0x415
  cpu MHz		: 2494.060
  cache size	: 25600 KB
  physical id	: 0
  siblings	: 2
  core id		: 0
  cpu cores	: 2
  apicid		: 0
  initial apicid	: 0
  fpu		: yes
  fpu_exception	: yes
  cpuid level	: 13
  wp		: yes
  flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology eagerfpu pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm xsaveopt fsgsbase smep erms
  bogomips	: 4988.12
  clflush size	: 64
  cache_alignment	: 64
  address sizes	: 46 bits physical, 48 bits virtual
  power management:

  processor	: 1
  vendor_id	: GenuineIntel
  cpu family	: 6
  model		: 62
  model name	: Intel(R) Xeon(R) CPU E5-2670 v2 @ 2.50GHz
  stepping	: 4
  microcode	: 0x415
  cpu MHz		: 2494.060
  cache size	: 25600 KB
  physical id	: 0
  siblings	: 2
  core id		: 1
  cpu cores	: 2
  apicid		: 2
  initial apicid	: 2
  fpu		: yes
  fpu_exception	: yes
  cpuid level	: 13
  wp		: yes
  flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology eagerfpu pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm xsaveopt fsgsbase smep erms
  bogomips	: 4988.12
  clflush size	: 64
  cache_alignment	: 64
  address sizes	: 46 bits physical, 48 bits virtual
  power management:
  ~~~
  {: .output}

- `tree -L 1`: 현재 위치(디렉토리)에서 1 단계 아래 파일시스템의 나무구조를 보여주고 있다.

  ~~~
  dcuser@ip-172-31-62-209 ~ $ tree -L 1
  .
  ├── dc_sample_data
  ├── Desktop
  ├── Downloads
  ├── FastQC
  ├── openrefine-2.6-beta.1
  ├── R
  └── Trimmomatic-0.32

  7 directories, 0 files
  ~~~
  {: .output}

## 클라우드에 접속을 유지한 상태로 유지하기.


클라우드 시스템에 연결된 방법에 따라 차이가 날 수 있지만, 한동안 작업을 계속 실행해야 하거나 계속해서 돌고 있는 
프로세스(process)와 작업(job)이 있을 수 있다.
VNC를 경유해서 클라우드를 GUI 데스크톱 형태한 연결된 경우, 시작된 작업은 계속 실행될 것이다.
SSH를 통해 연결된 경우, SSH 접속을 종료(예를 들어, SSH 세션을 끝내거나, 인터넷 연결이 끊어지거나,
노트북을 종료시키는 등등)시키면 접속이 끊어지게 될 때 돌고 있던 작업은 프로세스는 중지(killed)된다.
백그라운드로 클라우드 프로세스를 계속 실행시키는 방법이 몇가지 존재한다.
백그라운드 프로세스를 지칭할 때, 
[Linux: Start Command In Background](http://www.cyberciti.biz/faq/linux-command-line-run-in-background/) 웹사이트에 
기술된 내용(명령어를 실행하고 쉘 프롬프트로 되돌아 감)을 지칭한다.
이번에 기술하는 프로그램은 `tmux`로 연결이 끊어지더라도 쉘 전체를 실행시키면서 프로세스 실행을 유지시켜주는 역할을 수행한다.
현재 시스템에 `tmux`가 없더라도, `screen`을 사용할 수는 있다.
`screen`은 `tmux`와 같은 기능을 갖춘 또다른 프로그램이다.
하지만, 매우 오래되서 사용하기가 더 투박하다; 어떤 클라우드 시스템에서도 이용가능하다는 것은 좋은 점이다.


`tmux`와 `screen` 프로그램 모두 '세션(session)'을 열어 사용한다.
'세션(session)'은 `tmux`와 `screen`에서 윈도우 화면으로 간주하면 좋다.
컴퓨터에서 작업을 하나 수행할 때 터미널을 열고, 또 다른 작업을 수행하고자 할때 
터미널 세션을 열어 작업하게 된다.

세션을 닫을 때까지 활동중 액티브 상태가 유지된다.
설사 컴퓨터 연결이 끊어지더라도, 해당 세션에서 시작된 작업(`job`)은 완료될 때까지 계속 돌아가게 된다.

### 세션 시작하고 세션에 연결하기

세션을 시작하고 나서 세션에 명칭을 부여하는 것도 가능하다:

- `tmux`

  ~~~
  $ tmux new -s session_name
  ~~~
  {: .bash}

- `screen`

  ~~~
  $ screen -S session_name
  ~~~
  {: .bash}

상기 명령어를 실행하게 되면, `session_name`을 갖는 세션이 생성된다.
해당 세션을 닫을 때까지 활동중(active) 상태로 유지된다.

### 세션에서 분리 (프로세스는 백그라운드로 계속 실행됨)

키보드에서 다음을 누르게 되면 세션에서 분리된다:


- `tmux`: `control + b` 다음에 `d` (세션 분리 목적)
- `screen`: `control + a` 다음에 `d` (세션 분리 목적)

#### 활동중인 액티브 세션 확인하기

현재 세션 혹은 `ssh`로 접속된 컴퓨터와 연결이 끊어지게 되면,
기존 세션에 재접속이 필요한 경우가 있다.
열려있는 기존 세션을 다음 명령어를 통해 확인할 수 있다:

- `tmux`

  ~~~
  $ tmux list-sessions
  ~~~
  {: .bash}

- `screen`

  ~~~
  $ screen -ls
  ~~~
  {: .bash}

#### 세션에 연결하기

기존 세션에 다음 명령어를 통해 다시 연결시킬 수 있다:

- `tmux`

  ~~~
  $ tmux attach -t session_name
  ~~~
  {: .bash} 

  The `-t` option = 'target'

- `screen`

  ~~~
  $ screen -r session_name
  ~~~
  {: .bash}

  `-r` 선택옵션: 분리된 screen 세션을 다시 시작

#### 세션 전환

다음 명령어로 세션을 전환시킬 수 있다:

- `tmux`

  ~~~
  $ tmux switch -t session_name
  ~~~
  {: .bash}

#### 세션 종료 (Kill a session)

다음 명령어소 세션을 종료시킬 수 있다:

- `tmux`

  ~~~
  $ tmux kill-session -t session_name
  ~~~
  {: .bash}

- `screen`

  ~~~
  $ screen -r session_name
  $ exit
  ~~~
  {: .bash}


### 소프트웨어 추가로 설치하기

`tmux` 소프트웨어는 기본 디폴트로 거의 모든 리눅스 인스턴스에 미설치 상태다.
하지만, 리눅스 배포판 팩키지 관리자 도구를 사용해서 
쉽게 신규 소프트웨어를 설치할 수 있다: APT (데비안 혹은 우분투), YUM (레드햇과 센토스).
우분투 APT (Advanced Package Tool) 팩키지 관리자를 사용하여 `tmux` 소프트웨어 설치 방법을 살펴보자.

#### APT 최신 상태로 갱신하기

어떤 소프트웨어를 설치하거나 최신 상태로 갱신하기 전에, 항상 로컬 APT 캐쉬를 최신 상태로 유지시켜야 한다:


~~~
$ sudo apt-get update
Hit:1 http://au.archive.ubuntu.com/ubuntu xenial InRelease
Get:2 http://au.archive.ubuntu.com/ubuntu xenial-updates InRelease [102 kB]
...
Fetched 3,413 kB in 1s (2,233 kB/s)
Reading package lists... Done
~~~
{: .bash}

#### APT 팩키지를 검색하기

흔한 소프트웨어 도구는 팩키지명이 동일한 경우가 많지만, 항상 그런 것은 아니다.
설치하고자 하는 프로그램 명칭을 알고는 있는데 팩키지 명칭을 확신하지 못하는 경우,
`apt` 프로그램을 사용해서 팩키지를 검색한다:


~~~
$ sudo apt search tmux
Sorting... Done
Full Text Search... Done
abduco/xenial 0.1-2 amd64
  terminal session manager

powerline/xenial-updates 2.3-1ubuntu0 amd64
  prompt and statusline utility

tmate/xenial 1.8.10-2build1 amd64
  terminal multiplexer with instant terminal sharing

tmux/xenial,now 2.1-3build1 amd64
  terminal multiplexer

tmuxinator/xenial 0.7.0-2 all
  Create and manage tmux sessions easily
$
~~~
{: .bash}

명백한 `tmux` 팩키지가 존재하기 때문에 이를 설치한다.

#### APT를 사용해서 팩키지 설치

팩키지 명칭을 알고 나면, `apt-get install` 명령어를 사용해서 `tmux` 팩키지를 설치한다:

~~~
$ sudo apt-get install tmux
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  libevent-2.0-5 libutempter0
The following NEW packages will be installed:
  libevent-2.0-5 libutempter0 tmux
0 to upgrade, 3 to newly install, 0 to remove and 0 not to upgrade.
Need to get 345 kB of archives.
After this operation, 949 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://mirror.overthewire.com.au/ubuntu xenial-updates/main amd64 libevent-2.0-5 amd64 2.0.21-stable-2ubuntu0.16.04.1 [114 kB]
Get:2 http://mirror.overthewire.com.au/ubuntu xenial/main amd64 libutempter0 amd64 1.1.6-3 [7,898 B]
Get:3 http://mirror.overthewire.com.au/ubuntu xenial/main amd64 tmux amd64 2.1-3build1 [223 kB]
Fetched 345 kB in 0s (863 kB/s)
(Reading database ... 130583 files and directories currently installed.)
...
Setting up libevent-2.0-5:amd64 (2.0.21-stable-2ubuntu0.16.04.1) ...
Setting up libutempter0:amd64 (1.1.6-3) ...
Setting up tmux (2.1-3build1) ...
Processing triggers for libc-bin (2.23-0ubuntu10) ...
~~~
{: .bash}

설치가 완료되면, EC2 인스턴스에 `tmux` 소프트웨어를 사용할 준비가 되었다.
