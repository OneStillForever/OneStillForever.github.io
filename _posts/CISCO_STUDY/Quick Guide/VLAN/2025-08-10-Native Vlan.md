---
layout: post
title: "Native Vlan"
date: 2025-08-10
categories:
  - "CISCO_STUDY/Quick Guide/VLAN"
tags:
---


# Native Vlan

# Native VLAN 정리

## 1. 정의

- **트렁크 포트에서 VLAN 태그가 없는(untagged) 프레임을 어떤 VLAN으로 처리할지 지정하는 VLAN ID**
- 기본값은 **VLAN 1**

## 2. 동작 원리

- 트렁크 포트에서 **Native VLAN에 속하는 트래픽은 VLAN 태그 없이 전송됨 (untagged 상태)**
- 수신 시에도 태그 없는 프레임은 Native VLAN으로 간주하여 처리
- 다른 VLAN 트래픽은 802.1Q 태그가 붙어서 전달됨

## 3. 주요 특징

- Native VLAN은 **트렁크 포트에서 특별히 ‘untagged’ 프레임을 처리하기 위한 VLAN임**
- Access 포트에서 들어오는 태그 없는 프레임도 해당 포트에 할당된 VLAN으로 처리되지만, Native VLAN은 트렁크 포트 전용 개념
- Native VLAN 설정이 양쪽 스위치에서 일치해야만 정상 동작

## 4. 실무 시 주의점

- Native VLAN이 다르면 CDP 경고가 발생하고, 트래픽이 올바른 VLAN으로 처리되지 않아 통신 장애 발생 가능
- 보안을 위해 기본 Native VLAN(1)을 다른 VLAN으로 변경하는 경우가 많음 (예: VLAN 99)
- Native VLAN 트래픽은 태그가 없으므로 보안상 취약할 수 있어 주의 필요

## 5. 주요 명령어

```bash
bash
복사편집
interface [포트명]
 switchport trunk native vlan [VLAN_ID]

```