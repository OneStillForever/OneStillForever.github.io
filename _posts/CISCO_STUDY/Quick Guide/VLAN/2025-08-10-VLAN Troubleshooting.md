---
layout: post
title: "VLAN Troublgshooting"
date: 2025-08-10
categories:
  - "CISCO_STUDY/Quick Guide/VLAN"
tags:
---


# VLAN Trougleshooting

Inter-VAN 구성이 작동하지 않는 데에는 여러 가지 이유가 있다. 모두 연결성 문제와 관련이 있다. 먼저, 물리 계층을 확인하여 케이블이 포트에 잘못 연결되었는지에 대한 이슈를 해결한다. 올바로 연결되었다면, 아래 표의 목록을 사용하여 Inter-VLAN 연결성이 실패할 수 있는 다른 일반적인 이유를 확인한다.

| 이슈 유형 | 해결 방법 |  확인 방법 |
| --- | --- | --- |
|  누락된 VLAN |  •VLAN이 없을 경우 VLAN을 생성(또는 재생성)
 •호스트 포트가 올바른 VLAN에 할당되었는지 확인 | showvlan [brief]
show interfaces switchport
ping |
|  Switch Trunk 포트 이슈 |  •Trunk가 올바르게 구성되었는지 확인
 • 포트가 Trunk 포트이고 활성화되어 있는지 확인 | show interface trunk
show running-config |
| Switch 액세스 포트 이슈 | •액세스 포트에 올바른 VLAN을 할당
•포트가 액세스 포트이고 활성화되어 있는지 확인
•호스트가 다른 서브넷에 잘못 구성되어 있음 | show interfaces switchport
show running-config interface ipconfig |
| 라우터 구성 이슈 | •라우터 서브 Interface IPv4 주소가 잘못 구성되어 있음
•라우터 서브 Interface VLAN ID가 잘못 구성되어 있음 | show ip interface brief
show interfaces |