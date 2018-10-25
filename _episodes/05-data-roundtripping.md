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


It's important to note that both ``curl`` and ``wget`` download to the computer that the
command line belongs to. So, if you are logged into AWS on the command line and execute
the ``curl`` command above in the AWS terminal, the file will be downloaded to your AWS
machine, not your local one.

``curl``, ``wget`` 프로그램 모두 해당 명령라인을 실행시킨 컴퓨터로 데이터파일을 다운로드 시킨다는 것이 중요하다.
따라서, AWS EC 인스턴스에 명령라인으로 로그인해서 AWS 터미널에서 상기 명령어를 ``curl``로 때리게 되면, 
AWS 컴퓨터로 파일이 다운로드 된다; 절대로 로컬 컴퓨터가 아니다. 

### Moving files between your laptop and your instance

What if the data you need is on your local computer, but you need to get it *into* the
cloud? There are also several ways to do this, but it's *always* easier
to start the transfer locally. **This means if you're using a transfer program, it needs to be
installed on your local machine, not on your instance. If you're typing into a terminal,
the terminal should not be logged into your instance, it should be showing your local computer.**

These directions are platform specific so please follow the instructions for your system:

**Please select the platform you wish to use for the exercises: <select id="id_platform" name="platformlist" onchange="change_content_by_platform('id_platform');return false;"><option value="aws_unix" id="id_aws_unix" selected> AWS_UNIX </option><option value="aws_win" id="id_aws_win" selected> AWS_Windows </option></select>**


<div id="div_aws_win" style="display:block" markdown="1">


### Uploading Data to your Virtual Machine

If you're using a PC, we recommend you use the *PSCP* program. This program is from the same suite of
tools as the putty program we have been using to connect.

1. If you haven't done so, download pscp from [http://the.earth.li/~sgtatham/putty/latest/x86/pscp.exe](http://the.earth.li/~sgtatham/putty/latest/x86/pscp.exe)
2. Make sure the *PSCP* program is somewhere you know on your computer. In this case,
your Downloads folder is appropriate.
3. Open the windows [PowerShell](https://en.wikipedia.org/wiki/Windows_PowerShell);
go to your start menu/search enter the term **'cmd'**; you will be able to start the shell
(the shell should start from C:\Users\your-pc-username>).
4. Change to the download directory

~~~
> cd Downloads
~~~
{: .bash}

5. Locate a file on your computer that you wish to upload (be sure you know the path). Then upload it to your remote machine (**you will need to know your ip address, and login credentials**). You will be prompted to enter a password, and then your upload will begin. **(make sure you use substitute 'your-pc-username' for your actual pc username)**

~~~
C:\User\your-pc-username\Downloads> pscp.exe local_file.txt dcuser@ip.address:/home/dcuser/
~~~
{: .bash}

### Downloading Data from your Virtual Machine

1. Follow the instructions in the Upload section to download (if needed) and access the *PSCP* program (steps 1-3)
2. Download the zipped fastqc report using the following command **(make sure you use substitute 'your-pc-username' for your actual pc username and dcuser@ ip.address with your remote login credentials)**

~~~
C:\User\your-pc-username\Downloads> pscp.exe dcuser@ip.address:/home/dcuser/dc_workshop/results/fastqc_untrimmed_reads/SRR097977_fastqc.zip C:\User\your-pc-username\Downloads
~~~
{: .bash}

</div>



<div id="div_aws_unix" style="display:block" markdown="1">

### scp

`scp` stands for 'secure copy protocol', and is a widely used UNIX tool for moving files
between computers. The simplest way to use `scp` is to run it in your local terminal,
and use it to copy a single file:

~~~
scp <file I want to move> <where I want to move it>
~~~
{: .bash}

Note that you are always running `scp` locally, but that *doesn't* mean that
you can only move files from your local computer. A command like:

~~~
$ scp <local file> <AWS instance>
~~~
{: .bash}

To move it back, you just re-order the to and from fields:

~~~
$ scp <AWS instance> <local file>
~~~
{: .bash}

### Uploading Data to your Virtual Machine

1. Open the terminal and use the `scp` command to upload a file (e.g. local_file.txt) to the dcuser home directory:

~~~
$  scp local_file.txt dcuser@ip.address:/home/dcuser/
~~~
{: .bash}

### Downloading Data from your Virtual Machine

Let's download a zipped file from our remote machine.  You should have a fastqc report in ~/dc_workshop/results/fastqc_untrimmed_reads/SRR097977_fastqc.zip

**Tip:** If you are looking for another (or any really) zip file in your home directory to use instead try

~~~
$ find ~ -name *.zip
~~~
{: .bash}


1. Download the fastqc report in ~/dc_workshop/results/fastqc_untrimmed_reads/SRR097977_fastqc.zip to your home ~/Dowload directory using the following command **(make sure you use substitute dcuser@ ip.address with your remote login credentials)**:

~~~
$ scp dcuser@ip.address:/home/dcuser/dc_workshop/results/fastqc_untrimmed_reads/SRR097977_fastqc.zip ~/Downloads
~~~
{: .bash}

Remember that in both instances, the command is run from your local machine, we've just flipped the order of the to and from parts of the command.
</div>
