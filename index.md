---
layout: lesson
---
{% include base_path.html %}

클라우드(cloud)는 엄청 많은 컴퓨터를 네트워크로 엮어낸 정말 멋진 명칭이다; 
본인이 선호하는 웹사이트를 호스트하고, 영화를 스트림으로 상영하고, 
온라인 쇼핑서비스를 제공하는 등 역할도 다양하다.
하지만, 로컬 컴퓨터에서 몇일, 몇주, 심지어 몇년 소요되는 분석을 수행할 수 있는 컴퓨팅 파워를 
제공하기 때문에 이를 이용하는데 활용하는 것도 가능하다.

In this lesson, you'll learn about renting cloud services that fit your analytic needs,
and how to interact with one of those services (AWS) via the command line. 

이번 학습에서, 본인 분석 작업에 적절한 클라우드 서비스를 빌려쓰는 방법과 
명령-라인 인터페이스(CLI)를 활용해서 클라우드 서비스 제공업체(AWS, 구글 클라우드, 마이크로소프트 애져)를 
다루는 방법도 학습한다.


> ## 선행학습
>
> 이번 학습은 배쉬 쉘을 실질적으로 이해하고 있다는 것을 전재로 깔고 있다.
> 아직 [유닉스 쉘](https://statkclee.github.io/shell-novice-kr/) 학습을 이수하지 않았고, 배쉬 쉘과 친숙하지 않다면, 선행학습을 마칠 것을 권장한다.
> 
> 윈도우 사용자는 PuTTY를 설치해야 하는데, [환경 설정]({{ relative_root_path }}{% link setup.md %}) 웺페이지에서 설치방법을 참조한다.
{: .prereq}

