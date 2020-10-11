---
title: Moveit 한글 Package 정리
---

### Moveit! 정리
----
Moveit!은 모션계획(Motion Planning)에서 비롯되어 ROS에서 보다 손쉽게 모션을 수행하기 위해 만든 하나의 프레임워크 오픈 API 모음이다.  
ROS에서는 다관절 로봇 및 다양한 Motion 을 수행하는 로봇들이 존재하며, 이에대한 다양한 임무를 수행할 수 있는 로봇들이 있다.  
각 로봇들의 관절 및 모션은 기구학적 설계에 따른 각기 다른 역학운동을 하도록 설계 되어있으며 임무수행을 위한 계획 역시 다르다.  
이에, ROS에서는 Rviz와 연계를 통한 모션플랜을 통하여 로봇의 임무수행을 보다 원활하게 진행 할 수 있도록 Moveit! 이라는 프레임워크를 도입 및 사용하고 있다.  


![Relation](https://github.com/gwkim9444/gwkim9444.github.io/blob/master/_posts/picture/moveit_logo-white.png?raw=true)  


### 모션계획이란?

	 로봇팔이 자신의 구동부인 서보(Servo)와 연계되어 작업이나 이동을 위한 경로(Path 또는 Trajectory)를 정하는 행위를 의미

모션계획은 크게 2가지로 나뉘는데, Local Motion Planning 과 Global Motion Planning으로 분류되어진다.

1. Local Motion Planning
 - 하나의 국지적 장소에서 충돌 회피 등을 위한 Path를 정하는 것
2. Global Motion Planning
 - 전체적인 Task에 기반한 Motion Planning


ROS에서는 다관절 로봇 및 다양한 Motion 을 수행하는 로봇들이 존재하며, 이에대한 다양한 임무를 수행할 수 있는 로봇들이 있다.

이에 따라, __Moveit!은 홈페이지에서 다음과 같은 기능을 기술하고있다.__

1. 3D 상호 가시화 기능
2. Gazebo(물리시뮬레이션) 과의 연동
3. 각종 역학적인 동작을 수행하는 기구들의 설정을 도와주는 Setup Assisant Tool
4. 각 기구들의 동작을 수행하는 수행계획과 상호작용의 분석
5. 각 기구들의 파이프 라인 및 실린더 객체에 대한 개체 분석 및 그립을 통한 Pick and Place 시뮬레이션 등을 가능하도록 하는 도구


해당 각 기능에 대한 API와 시작 방법은 하기에 기술하도록 하겠다.
