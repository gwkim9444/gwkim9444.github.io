---
title: Moveit 한글 Package 정리
---

### Moveit! 정리
----
Moveit!은 모션계획(Motion Planning)에서 비롯되어 ROS에서 보다 손쉽게 모션을 수행하기 위해 만든 하나의 프레임워크 오픈 API 모음이다. 
이미 수많은 기업에서 Moveit!을 활용해 Motion Planning을 하고 있으며, 대표적으로는 NASA / Google / MicroSoft 등의 굴지의 IT기업이 주목하는 프레임워크라고 할 수 있다.  

![Relation](https://github.com/gwkim9444/gwkim9444.github.io/blob/master/_posts/picture/moveit_logo-white.png?raw=true)  

Moveit!은 최근 ROS2의 도입에 의해 Moveit!2를 론칭하면서, 사용자 환경을 개선해 나아가고 있다. ROS에서는 다관절 로봇 및 다양한 Motion 을 수행하는 로봇들이 존재하며, 이에대한 다양한 임무를 수행할 수 있는 로봇들이 있다. 각 로봇들의 관절 및 모션은 기구학적 설계에 따른 각기 다른 역학운동을 하도록 설계 되어있으며 임무수행을 위한 계획 역시 다르다. 이에, ROS에서는 Rviz와 연계를 통한 모션플랜을 통하여 로봇의 임무수행을 보다 원활하게 진행 할 수 있도록 Moveit! 이라는 프레임워크를 도입 및 사용하고 있다.  

본 글에서는 Moveit!의 활용방법과 가장 보편적으로 사용되고있는 6Axis 이론을 접목시킨 API에 대해 언급하고자 한다. 일반적인 Moveit!은 Rviz를 활용하여 인터페이스 및 테스트를 진행하기 때문에, Rviz에서 Motion Planning 기능을 언급하게 될 것이다. 참고로 현재 Moveit 소프트웨어 솔루션을 참조한 로봇 메니퓰레이터는 약 150개 이상의 로봇이 채택하여 사용하고 있으며, 점점 더 넓고 광범위한 분야의 로봇들이 활용할 것으로 예상된다.

Moveit은 현재 2.2 Galactic Geochelone으로 개발자 버전이 배포되고 있으며 __(현재시간 2020-10-14 14:10)__ 상용버전으로는 Foxy 2.1 LTS 버전이 존재한다.
Moveit!은 ROS의 수많은 버전과 함께 같이 개발되었으며, 해당 개발 종료 시점은 하기와 같이 나와있다.

ROS - Kinetic 0.9 LTS - 2021 April  
ROS - Melodic 1.0 LTS - 2023 May  
ROS - Eloquent 2.0 Betat - 2020 Feb  
ROS - Noetic 1.1 LTS - 2025 May  
ROS - Foxy 2.1 LTS - 2023 May  
ROS - Glactic 2.2 Devel - TBD  

Moveit Tutorial에서 활용하고 있는 내용은 Franka Emika 회사의 Panda 협동 Robot으로 대중적으로 활용되고 있는 6축로봇인듯 하다.  

### Motion Planning(모션계획)이란?

	 로봇팔이 자신의 구동부인 서보(Servo)와 연계되어 작업이나 이동을 위한 경로(Path 또는 Trajectory)를 정하는 행위를 의미

모션계획은 크게 2가지로 나뉘는데, Local Motion Planning 과 Global Motion Planning으로 분류되어진다.

1. Local Motion Planning
 - 하나의 국지적 장소에서 충돌 회피 등을 위한 Path를 정하는 것
2. Global Motion Planning
 - 전체적인 Task에 기반한 Motion Planning

  
![Relation](https://github.com/gwkim9444/gwkim9444.github.io/blob/master/_posts/picture/Rviz_Motion_Planning_Plugin.png?raw=true)  
  
ROS에서는 다관절 로봇 및 다양한 Motion 을 수행하는 로봇들이 존재하며, 이에대한 다양한 임무를 수행할 수 있는 로봇들이 있다.

이에 따라, __Moveit!은 홈페이지에서 다음과 같은 기능을 기술하고있다.__

1. 3D 상호 가시화 기능
2. Gazebo(물리시뮬레이션) 과의 연동
3. 각종 역학적인 동작을 수행하는 기구들의 설정을 도와주는 Setup Assisant Tool
4. 각 기구들의 동작을 수행하는 수행계획과 상호작용의 분석
5. 각 기구들의 파이프 라인 및 실린더 객체에 대한 개체 분석 및 그립을 통한 Pick and Place 시뮬레이션 등을 가능하도록 하는 도구


해당 각 기능에 대한 API와 시작 방법은 하기에 기술하도록 하겠다.

### Moveit! 지원 언어
---
Moveit!의 경우 Python, C++를 지원하고 있으며, Script인 Python 언어 보다는 C++ 을 활용한 객체 언어를 활용하는것이 좋다.  

![Relation](https://github.com/gwkim9444/gwkim9444.github.io/blob/master/_posts/picture/moveit_pipline.png?raw=true)   


__Class : MoveGroup__  
가장 간단한 유저간의 인터페이스는 MoveGroup Class를 활용하는것이다. 이 클래스는 사용자들이 쉽게 기능을 수행 할 수 있도록 관절 상태 세팅을 구체화 하고, 목표 도달점과 모션계획을 생성하고 로봇을 움직이고 객체를 더하고 환경정보와 사물의 Pick & Place를 관리한다. ROS1에서 사용하는 통신인 Services, Actions를 통하여 메세지 정보를 전달하고 메시지를 받는 노드들은 각 기능을 수행한다.

C++ 을 활용한 각 API에 대한 활용 및 이를 활용한 Visualization 등에 대해 정리하도록 하겠다.  

### __Setup__

Moveit!에서는 Planning groups라는 객체선언을 통해 해당 Class에 API를 Trigger 시키는 형태로 운영한다.  

C++ 기준 Rviz 내 존재하는 Planning Group의 로봇 이름을 찾아 선언한다.

    static const std::string 선언 이름 = "로봇 이름";

먼저 MoveGroup 객체를 잡고 해당 로봇의 제어와 계획을 잡고싶은 것에 대한 API를 호출한다.
해당 MoveGroup 객체를 선언하는 방법은 다음과 같다.  

    moveit::planning_interface::MoveGroupInterface move_group(PLANNING_GROUP);


