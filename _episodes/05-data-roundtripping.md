---
title: "데이터 왕복시키기"
teaching: 15
exercises: 5
questions:
- "데이터를 클라우드로 어떻게 이동시킬 수 있을까?"
- "본인 컴퓨터로 분석결과를 어떻게 하면 다시 돌려받을 수 있을까?"
objectives:
- "데이터를 클라우드 세션으로 전송한다."
- "데이터를 클라우드 세션에서 끄집어 낸다."
keypoints:
- "데이터를 어느 방향에서 옮기든지 관계 없이, 로컬 컴퓨터에서 데이터 송수신을 시작하는 것이 더 쉽다."
---

<script language="javascript" type="text/javascript">
function set_page_view_defaults() {
    document.getElementById('div_aws_win').style.display = 'block';
    document.getElementById('div_aws_unix').style.display = 'none';
};

function change_content_by_platform(form_control){
    if (!form_control || document.getElementById(form_control).value == 'aws_win') {
        set_page_view_defaults();
    } else if (document.getElementById(form_control).value == 'aws_unix') {
        document.getElementById('div_aws_win').style.display = 'none';
        document.getElementById('div_aws_unix').style.display = 'block';
    } else {
        alert("Error: Missing platform value for 'change_content_by_platform()' script!");
    }
}

window.onload = set_page_view_defaults;
</script>


## 데이터 이동은 단순하다.
(하지만, 항상 쉬운 것도 아니다!)

이제 클라우드 구름속에 있기 때문에, 데이터가 필요하다.
데이터를 꺼내올 수 있는 장소가 두군데 있다: 로컬 컴퓨터 혹은 클라우드에 있는 다른 컴퓨터 (예를 들면, NCBI).

데이터를 꺼내오는 방법은 데이터가 지금 당장 어디에 있느냐에 따라 달라진다.

### 클라우드에서 데이터를 꺼내오기

There are two programs that will download data from a remote server to your local
(or remote) machine: ``wget`` and ``curl``. They were designed to do slightly different
tasks by default, so you'll need to give the programs somewhat different options to get
the same behaviour, but they are mostly interchangeable.

원격 서버로부터 로컬(혹은 원격) 컴퓨터로 데이터를 다운로드를 도와주는 
프로그램이 두개 있다: ``wget``, ``curl``.
두 프로그램은 다소 차이나는 작업을 수행하도록 처음에 설계되었다.
따라서, 동일한 동작을 얻어오는데 다소 차이나는 선택옵션을 전달해야 한다.
하지만, 거의 상호호환된다.

 - ``wget``은 "world wide web get"의 줄임말이다. 따라서, 웹주소에서 데이터 혹은 웹페이지를 **다운로드(donwload)**하는 것이 기본 기능이 된다.

 - ``cURL``은 동음이의어를 이용한 말장난으로, "see URL"을 소리나는 대로 읽은 것이다. 따라서, 웹주소의 웹페이지 혹은 데이터를 있는 그대로 **출력(display)**하는 것이 기본 기능이다.

두 프로그램 중 어는 것을 가용할지는 본인 운영체제에 전적으로 달려있다.
컴퓨터 대부분에 둘 중 하나만 기본으로 장착되어 있다.

`Ensembl`에서 데이터를 다운로드 받아보자.
탭구분자를 갖는 매우 작은 텍스트 파일을 다운로드 받을 것이다.
다운로드를 시작하기 전에, ``curl`` 혹은 ``wget``이 사용가능하지 살펴보자.


어떤 프로그램이 장착되어 있는지 확인하려면, 다음 명령어를 타이핑한다:
 
~~~
$ which curl
$ which wget
~~~
{: .bash}

``which``는 설치된 프로그램 찾아주는 배쉬 프로그램으로 설치된 폴더도 함께 알려준다.
요청한 프로그램을 찾을 수 없는 경우, 
아무 것도 반환하지 않는다; 즉, 반환되는 결과가 하나도 없다.

맥 OSX 운영체제애, 다음 출력결과가 반환될 듯 싶다:

~~~
$ which curl
~~~
{: .bash}

~~~
/usr/bin/curl
~~~
{: .output}

~~~
$ which wget
~~~
{: .bash}

~~~
$
~~~
{: .output}


상기 출력결과를 해석하면 ``curl``은 설치되어 있지만, 
``wget``은 설치되어 있지 않다는 의미가 된다.


``curl`` 혹은 ``wget`` 프로그램이 설치된 것이 확인되면, 파일을 다운로드 받는데 
다음 명령어 중 하나를 사용한다:

~~~
$ cd
$ wget ftp://ftp.ensemblgenomes.org/pub/release-37/bacteria/species_EnsemblBacteria.txt
~~~
{: .bash}

혹은,

~~~
$ cd
$ curl -O ftp://ftp.ensemblgenomes.org/pub/release-37/bacteria/species_EnsemblBacteria.txt
~~~
{: .bash}

파일을 보려고 하는 것이 목적이 아니라 파일을 *다운로드* 받으려고 하기 때문에,
어떤 변경자(modifier)도 없이 ``wget`` 명령어를 사용했다.
하지만, ``curl`` 명령어의 경우 `-O` 플래그를 사용해야 된다.
`-O` 플래그는 데이터를 보여주는 대신에 ``curl``로 하여금 데이터를 다운로드하게 하고 
**동시에** 서버 데이터 파일과 동일한 파일명으로 다운로드해서 저장되도록 한다: `species_EnsemblBacteria.txt`


``curl``, ``wget`` 모두 명령라인(command line)이 동작하는 컴퓨터에 다운로드한다는 점이 주목한다.
따라서, 명령라인으로 AWS에 로그인한 후에 ``curl`` 명령을 AWS 터미널에서 실행하게 되면,
파일은 본인 로컬 컴퓨터가 아닌 AWS 컴퓨터로 다운로드된다.

``curl``, ``wget`` 프로그램 모두 해당 명령라인을 실행시킨 컴퓨터로 데이터파일을 다운로드 시킨다는 것이 중요하다.
따라서, AWS EC 인스턴스에 명령라인으로 로그인해서 AWS 터미널에서 상기 명령어를 ``curl``로 때리게 되면, 
AWS 컴퓨터로 파일이 다운로드 된다; 절대로 로컬 컴퓨터가 아니다. 

### 노트북 컴퓨터와 AWS 인스턴스 사이 파일 이동

작업할 데이터가 로컬 컴퓨터에 있지만, 클라우드 AWS로 가져갈 필요가 있다면 어떨까?
작업방식이 몇가지 있지만, *항상* 로컬에서 전송작업을 시작하는 것이 더 쉽다.
**파일 전송 프로그램을 사용한다면, AWS 인스턴스가 아니라, 로컬 컴퓨터에 전송프로그램이 설치되어 있어야 된다는 의미다.**


파일 작업방식은 플랫폼마다 특성을 타서, 작업자 운영체제에 따라 다음 전차를 따른다:

**연습문제로 사용할 플랫폼을 선택한다: <select id="id_platform" name="platformlist" onchange="change_content_by_platform('id_platform');return false;"><option value="aws_unix" id="id_aws_unix" selected> AWS_UNIX </option><option value="aws_win" id="id_aws_win" selected> AWS_Windows </option></select>**

<div id="div_aws_win" style="display:block" markdown="1">

### 데이터를 가상컴퓨터에 업로드하기 

PC를 사용한다면, *PSCP* 프로그램 사용을 추천한다. AWS EC2 인스턴스에 접속하는데 사용했던 `putty` 프로그램과 통일한 종류의 도구 프로그램이다.

1. 설치가 되어 있지 않는 경우, [http://the.earth.li/~sgtatham/putty/latest/x86/pscp.exe](http://the.earth.li/~sgtatham/putty/latest/x86/pscp.exe)을 클릭하여 `pscp`를 다운로드 받는다.
2. *PSCP* 프로그램이 본인 컴퓨터 어느 곳에 있는지 확인한다. 보통 `다운로드`(Downloads) 폴더에 저장된다. 
3. [PowerShell](https://en.wikipedia.org/wiki/Windows_PowerShell) 윈도우를 연다; 시작 메뉴 / 검색으로 가서 **'cmd'** 을 타이핑하고 엔터를 친다. 쉘이 시작된다. (쉘은 `C:\Users\your-pc-username>`에서 프롬프트를 깜빡이며 대기하고 있다.)
4. `다운로드`(Downloads) 폴더로 이동한다.

~~~
> cd Downloads
~~~
{: .bash}

업로드할 파일을 특정하여 위치시킨다.(더불어 업로드할 파일 경로를 확인한다)
그리고 나서 원격 컴퓨터에 파일을 업로드시킨다.(**ip주소, 로그인 자격증명(credential)**을 알고 있어야 한다.**)
비밀번호를 입력하고 나면 파일 업로드 작업이 개시된다.

~~~
C:\User\your-pc-username\Downloads> pscp.exe local_file.txt dcuser@ip.address:/home/dcuser/
~~~
{: .bash}

### 가상 컴퓨터에서 데이터 다운로드 받기 

1. 먼저 파일 업로드 절차를 따른다. *PSCP* 프로그램을 설치해서 다운로드 받는다.(1-3 단계)
2. 다음 명령어를 사용해서 보고서를 압축한 `SRR097977_fastqc.zip` 파일을 다운로드 받는다. **('your-pc-username'을 본인 PC 사용자명으로 바꾸고, `dcuser@ ip.address`도 원격 로그인 자격증명(credential)로 바꾼다.)**

~~~
C:\User\your-pc-username\Downloads> pscp.exe dcuser@ip.address:/home/dcuser/dc_workshop/results/fastqc_untrimmed_reads/SRR097977_fastqc.zip C:\User\your-pc-username\Downloads
~~~
{: .bash}

</div>

<div id="div_aws_unix" style="display:block" markdown="1">

### `scp`

`scp`는 'secure copy protocol'의 약자로 컴퓨터간에 파일을 이동하는데 폭넓게 사용되는 유닉스 도구다.
'scp'를 사용하는 가장 간단한 방식은 로컬 터미널에서 실행시키고 파일을 복사하는데 사용하는 것이다:

~~~
scp <file I want to move> <where I want to move it>
~~~
{: .bash}

로컬 컴퓨터에서 `scp`를 항상 실행하는 것에 주목한다.
그렇다고 해서, 로컬 컴퓨터에서 파일만 이동할 수 있다는 것은 아니다. 다음 명령어처럼:

~~~
$ scp <로컬 파일> <AWS 인스턴스>
~~~
{: .bash}

역으로 이동시키려면, `to`와 `from` 순서를 바꾸면 된다:

~~~
$ scp <AWS 인스턴스> <로컬 파일>
~~~
{: .bash}

### 파일을 가상 컴퓨터에 업로드

1. 터미널을 열고 `scp` 명령어를 사용해서 파일(`local_file.txt`)을 `dcuser` home 디렉토리로 업로드한다:

~~~
$  scp local_file.txt dcuser@ip.address:/home/dcuser/
~~~
{: .bash}

### 가상 컴퓨터에서 데이터 다운로드

원격 컴퓨터에서 압축 파일을 다운로드하자. 
먼저 원격 컴퓨터에 `~/dc_workshop/results/fastqc_untrimmed_reads/SRR097977_fastqc.zip` 리포트 파일이 있어야 한다.

**Tip:** home 디렉토리에서 다른 압축파일(zip)을 찾고자 한다면, 다음 명령어를 사용한다:

~~~
$ find ~ -name *.zip
~~~
{: .bash}


1. `~/dc_workshop/results/fastqc_untrimmed_reads/SRR097977_fastqc.zip` 압축 리포트 파일을 
로컬 컴퓨터 `~/Downloads` 디렉토리에 다음 명령어로 다운로드 받는다.
**(`dcuser@ ip.address`를 원격 로그인 자격증명(credentials)로 바꾼다)**:


~~~
$ scp dcuser@ip.address:/home/dcuser/dc_workshop/results/fastqc_untrimmed_reads/SRR097977_fastqc.zip ~/Downloads
~~~
{: .bash}

상기 명령어는 작업자 본인 로컬 컴퓨터에서 실행되는 것을 기억한다.
명령어의 `from`, `to` 순서를 뒤바꿨을 뿐이다.

</div>
