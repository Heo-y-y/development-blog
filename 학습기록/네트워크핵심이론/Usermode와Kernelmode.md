# **User mode와 Kernel mode**

![스크린샷 2023-11-16 오후 5 22 21](https://github.com/Heo-y-y/development-blog/assets/112863029/fe579dae-e096-46af-8973-159b2cc644bf)

컴퓨터라고 하는 것은 **S/W**, **H/W**로 나뉘는데, 여기서 **S/W**는 다시 **User mode**, **Kernel mode**로 나뉜다.

하드웨어 중에서 **NIC**라는 **네트워크 인터페이스 카드**를 주의 깊게 봐야 한다.

**하드웨어를 제어하기 위한 소프트웨어**가 존재하는데 그것을 **드라이버**라고 한다. 드라이버라는 것이 설치가 되어 있어야 NIC가 작동하는 것이다.

**Kernel**에는 **프로토콜이 구현된 소프트웨어**가 들어있는데, 이것을 **TCP / IP**라고 한다.

![스크린샷 2023-11-16 오후 5 26 55](https://github.com/Heo-y-y/development-blog/assets/112863029/4e9d98f0-37f7-4c39-93f7-9014ea26ac59)

위 그림을 보고 설명하면,

**Kernel**의 구성요소를 **User mode** **애플리케이션 프로세스가 접근할 수 있도록** **Kernel에서 길을 열어**준다.

**TCP / IP를 추상화 시킨 인터페이스 파일**을 **소켓**이라고 한다.

L5에서 넘어가면은 유저모드 애플리케이션인데 즉, 프로세스이다.

**전송계층**은 **TCP**이고, **IP**는 **네트워크 계층**이다.

그림을 보면 알겠지만 TCP 밑에 IP가 있는데, TCP / IP 로 표현하는 것은 계층이 다름을 표기하는 것이다.

데이터 링크는 Driver부터 NIC까지이다.

**소켓의 실체는 파일**이다.

그런 형태의 특별히 **네트워크 추상화가 이루어진 파일을 소켓**이라고 한다.

그것을 여는 주체가 **프로세스**이 것이다.

**참고 자료**

- [https://www.inflearn.com/course/네트워크-핵심이론-기초/dashboard](https://www.inflearn.com/course/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%ED%95%B5%EC%8B%AC%EC%9D%B4%EB%A1%A0-%EA%B8%B0%EC%B4%88/dashboard)
