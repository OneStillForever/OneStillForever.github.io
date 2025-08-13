---
layout: post
title: "CISCO_COMMAND"
date: 2025-08-10
---

# ✅ Cisco IOS CLI 명령어 통합 정리 (풀버전)

> 장비/IOS에 따라 일부 명령어가 다를 수 있습니다. (Catalyst/ISR/ASR 등)

---

## 📁 1. 기본 CLI 조작

| 명령어                                     | 설명                        |
| --------------------------------------- | ------------------------- |
| `enable` / `disable`                    | 사용자 ↔ 특권 모드 전환            |
| `configure terminal`                    | 전역 설정 모드 진입               |
| `exit` / `end`                          | 모드 종료 (한 단계/전역)           |
| `hostname <NAME>`                       | 장비 이름 변경 (SSH 키 생성에 필요)   |
| `banner motd #...#`                     | MOTD 배너 설정                |
| `no ip domain-lookup`                   | 오타로 인한 DNS 조회 방지          |
| `clock set HH:MM:SS DD MMM YYYY`        | 수동 시간 설정                  |
| `show clock`                            | 현재 시간 확인                  |
| `license ?`                             | 라이선스 관련(장비별 상이)           |
| `sdm prefer dual-ipv4-and-ipv6 default` | 듀얼스택 SDM 템플릿(스위치, 재부팅 필요) |

---

## 🔍 2. 상태 확인 (Show / Debug)

| 명령어                                           | 설명              |
| --------------------------------------------- | --------------- |
| `show running-config` / `show startup-config` | 현재/부팅 설정        |
| `show ip interface brief`                     | IP/인터페이스 요약     |
| `show interfaces`                             | 인터페이스 상세        |
| `show version`                                | IOS/업타임/메모리     |
| `show vlan brief`                             | VLAN 요약         |
| `show mac address-table`                      | MAC 테이블         |
| `show arp`                                    | ARP 캐시          |
| `show cdp neighbors` / `show cdp ne`          | CDP 인접 요약       |
| `show cdp neighbors detail`                   | CDP 인접 상세       |
| `show ip route`                               | 라우팅 테이블         |
| `show ip protocols`                           | 동적 라우팅 프로토콜 상태  |
| `show standby brief`                          | HSRP 요약         |
| `show etherchannel summary`                   | EtherChannel 요약 |
| `show ip dhcp binding`                        | DHCP 임대 현황      |
| `show vtp status`                             | VTP 상태          |
| `show access-lists`                           | ACL 확인          |
| `show aaa servers` / `show radius`            | AAA/RADIUS 상태   |
| `show dot1x all`                              | 802.1X 상태       |
| `debug ip rip` / `undebug all`                | RIP 디버그 / 해제    |

> CDP가 꺼져있다면 `cdp run`(글로벌), `cdp enable`(인터페이스)로 활성화

---

## 🌐 3. IP & 인터페이스

```bash
interface g0/1
 ip address 192.168.1.1 255.255.255.0
 no shutdown
```

| 명령어                                  | 설명             |
| ------------------------------------ | -------------- |
| `interface <NAME>`                   | 인터페이스 진입       |
| `ip address <IP> <MASK> [secondary]` | IPv4 주소 설정     |
| `ipv6 address <ADDR>/<LEN>`          | IPv6 주소 설정     |
| `no shutdown` / `shutdown`           | 인터페이스 활성/비활성   |
| `ip default-gateway <IP>`            | L2 스위치 기본GW    |
| `clock rate <Kbps>`                  | DCE 클럭 설정(시리얼) |
| `interface loopback <N>`             | 루프백 생성         |

---

## 📦 4. VLAN / 스위칭

```bash
vlan 10
 name IT
interface f0/1
 switchport mode access
 switchport access vlan 10
```

| 명령어                                    | 설명                   |
| -------------------------------------- | -------------------- |
| `vlan <ID>` / `name <NAME>`            | VLAN 생성/이름           |
| `switchport mode access`               | 액세스 포트               |
| `switchport access vlan <ID>`          | 액세스 포트 VLAN 지정       |
| `switchport mode trunk`                | 트렁크 포트               |
| `switchport trunk encapsulation dot1q` | 802.1Q 지정(장비에 따라 필요) |
| `switchport trunk allowed vlan <list>` | 트렁크 허용 VLAN 목록       |
| `switchport trunk native vlan <ID>`    | 네이티브 VLAN 지정         |
| `interface vlan <ID>`                  | SVI 생성/진입            |
| `ip helper-address <IP>`               | DHCP 릴레이(인터페이스)      |

### Router-on-a-Stick (RoaS)

```bash
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
```

---

## 📡 5. 정적 라우팅

```bash
ip route 0.0.0.0 0.0.0.0 192.168.1.1
```

| 명령어                 | 설명    |            |
| ------------------- | ----- | ---------- |
| \`ip route   \<NH   | IF>\` | 정적 경로      |
| \`ipv6 route / \<NH | IF>\` | IPv6 정적 경로 |

---

## 🛰 6. RIP (v2)

```bash
router rip
 version 2
 network 192.168.1.0
 no auto-summary
 passive-interface <IF>
 default-information originate
```

| 항목      | 명령어                                                                        |
| ------- | -------------------------------------------------------------------------- |
| 요약      | `ip summary-address rip <IP> <MASK>` (IF모드)                                |
| 인증(평문)  | `ip rip authentication mode text` + `ip rip authentication key-chain <KC>` |
| 인증(MD5) | `ip rip authentication mode md5` + `ip rip authentication key-chain <KC>`  |
| 키체인     | `key chain <KC>` → `key <N>` → `key-string <STR>`                          |

---

## 🚦 7. OSPF (Single Area / Multi Area)

```bash
router ospf 1
 router-id 1.1.1.1
 network 192.168.1.0 0.0.0.255 area 0
 default-information originate always
 auto-cost reference-bandwidth 10000
```

| 기능           | 명령어                                                                                       |
| ------------ | ----------------------------------------------------------------------------------------- |
| 네트워크 등록      | `network <IP> <WC> area <AREA>`                                                           |
| 기본경로 광고      | `default-information originate [always]`                                                  |
| 인증(평문)       | (IF) `ip ospf authentication` + `ip ospf authentication-key <PWD>`                        |
| 인증(MD5)      | (IF) `ip ospf authentication message-digest` + `ip ospf message-digest-key <N> md5 <PWD>` |
| Stub         | `area <A> stub [no-summary]` *(no-summary=Totally Stub)*                                  |
| NSSA         | `area <A> nssa [no-summary] [default-information-originate]`                              |
| Area 요약      | `area <A> range <IP> <MASK>`                                                              |
| ASBR 외부요약    | `summary-address <IP> <MASK>` *(ASBR)*                                                    |
| Virtual-Link | `area <TRANSIT> virtual-link <RID>`                                                       |
| 참조대역폭        | `auto-cost reference-bandwidth <Mbps>` *(모든 라우터 동일 권장)*                                   |
| 타이머          | (IF) `ip ospf hello-interval <S>` / `ip ospf dead-interval <S>`                           |

### 재분배 (Redistribute)

| 대상           | 명령어 예시                                          |
| ------------ | ----------------------------------------------- |
| 정적→OSPF      | `router ospf 1` → `redistribute static subnets` |
| EIGRP→OSPF   | `redistribute eigrp 100 subnets metric-type 1`  |
| 기본메트릭        | `default-metric <COST>` *(프로토콜별)*               |
| Route-map 필터 | `redistribute static route-map <RM>`            |

> OSPF의 `distribute-list`는 **LSA가 아니라 RIB 설치 시점**을 필터링합니다.

---

## 🚚 8. EIGRP (Classic & Named)

### Classic Mode

```bash
router eigrp 100
 eigrp router-id 2.2.2.2
 network 10.0.0.0 0.0.0.255
 passive-interface default
 no passive-interface g0/0
```

| 기능        | 명령어                                                                                          |
| --------- | -------------------------------------------------------------------------------------------- |
| 인터페이스 타이머 | (IF) `ip hello-interval eigrp <AS> <S>` / `ip hold-time eigrp <AS> <S>`                      |
| 대역폭 반영    | (IF) `bandwidth <Kbps>`                                                                      |
| 요약        | (IF) `ip summary-address eigrp <AS> <IP> <MASK>`                                             |
| 인증(MD5)   | (IF) `ip authentication mode eigrp <AS> md5` + `ip authentication key-chain eigrp <AS> <KC>` |

### Named Mode

```bash
router eigrp CCNP
 address-family ipv4 unicast autonomous-system 100
  af-interface default
   authentication mode hmac-sha256 <KEY>
   summary-address 10.0.0.0 255.255.0.0
  topology base
   variance 2
```

---

## 🧭 9. 라우팅 경로 필터링 (Routing Path Filtering)

| 방식                   | 명령어 예시                                                                                                         | 비고            |
| -------------------- | -------------------------------------------------------------------------------------------------------------- | ------------- |
| Distribute-list(ACL) | `access-list 10 permit 192.168.1.0 0.0.0.255` → `router eigrp 100` → `distribute-list 10 in`                   | 프로토콜별 지원범위 상이 |
| Prefix-list          | `ip prefix-list PFX deny 10.0.0.0/8` → `neighbor 1.1.1.1 prefix-list PFX in` (BGP)                             | CIDR 정밀       |
| Route-map(재분배)       | `route-map RM deny 10` + `match ip address prefix-list PFX` → `redistribute static route-map RM`               | 조건부 필터        |
| Passive-interface    | `passive-interface <IF>`                                                                                       | 광고 차단(수신 가능)  |
| PBR                  | `route-map PBR permit 10` + `match ip address <ACL>` + `set ip next-hop <IP>` → `(IF) ip policy route-map PBR` | 정책 기반 라우팅     |

---

## 🧱 10. ACL (Access Control List)

```bash
ip access-list extended BLOCK_SSH
 deny tcp any any eq 22
 permit ip any any
!
interface g0/0
 ip access-group BLOCK_SSH in
```

| 유형       | 핵심 명령                                    |              |
| -------- | ---------------------------------------- | ------------ |
| 표준       | `access-list <1-99,1300-1999> ...`       |              |
| 확장       | `access-list <100-199,2000-2699> ...`    |              |
| 이름 기반    | \`ip access-list {standard               | extended} \` |
| 인터페이스 적용 | \`(IF) ip access-group  in               | out\`        |
| VTY 제한   | `line vty 0 4` → `access-class <ACL> in` |              |

---

## 🧰 11. Route-Map

```bash
route-map SET-NH permit 10
 match ip address 101
 set ip next-hop 192.0.2.1
```

| 용도     | 명령                                   |       |
| ------ | ------------------------------------ | ----- |
| PBR    | `(IF) ip policy route-map <RM>`      |       |
| 재분배 제어 | `redistribute static route-map <RM>` |       |
| BGP 정책 | \`neighbor X route-map  in           | out\` |

---

## 🧩 12. HSRP

```bash
interface g0/1
 standby version 2
 standby 1 ip 192.168.1.254
 standby 1 priority 110
 standby 1 preempt
 standby 1 authentication md5 key-string cisco
 standby 1 track GigabitEthernet0/2 decrement 30
```

\| 확인 | `show standby brief` |

---

## 🔒 13. 802.1X & AAA (FreeRADIUS)

```bash
aaa new-model
aaa authentication login default group radius local
aaa authorization network default group radius
aaa accounting update periodic 5
radius-server host 192.168.1.10 auth-port 1812 acct-port 1813 key R@dKey
ip radius source-interface Loopback0
!
dot1x system-auth-control
interface g0/1
 authentication order dot1x mab
 authentication priority dot1x mab
 authentication port-control auto
 mab
 dot1x pae authenticator
```

---

## ♻ 14. EtherChannel (Port-Channel)

```bash
interface range g0/1-2
 channel-protocol lacp
 channel-group 1 mode active
!
interface port-channel 1
 switchport mode trunk
 switchport trunk allowed vlan 10,20
```

| 모드               | 설명   |
| ---------------- | ---- |
| `active/passive` | LACP |
| `desirable/auto` | PAgP |

---

## 🏷 15. DHCP (IPv4)

```bash
ip dhcp excluded-address 192.168.1.1 192.168.1.20
ip dhcp pool LAN1
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8 1.1.1.1
 domain-name example.local
 lease 7
```

\| 확인/관리 | `show ip dhcp binding` / `clear ip dhcp binding *` |

---

## 🧭 16. VTP (VLAN Trunking Protocol)

```bash
vtp domain MYDOMAIN
vtp password MyPass
vtp version 2
vtp mode server      ! 서버: VLAN DB 생성/배포
! vtp mode client    ! 클라이언트: 수신만, 로컬 생성 불가
vtp mode transparent ! 트랜스페어런트: 로컬만 관리, 광고는 통과(V1/2)
! vtp mode off       ! (일부 IOS) VTP 완전 비활성
vtp pruning          ! 불필요 VLAN 트래픽 가지치기(서버에서 활성)
```

| 확인/파일                     | 설명                                             |
| ------------------------- | ---------------------------------------------- |
| `show vtp status`         | VTP 도메인/모드/버전/리비전                              |
| `vtp file flash:vlan.dat` | VLAN DB 파일 지정(플랫폼별)                            |
| **VTPv3**                 | `vtp version 3` + `vtp primary vlan` (주 서버 승격) |

> Transparent 모드는 VLAN을 **로컬에서만** 생성/수정하며, VTP 광고에 의존하지 않습니다.

---

## 🌍 17. IPv6 기본

```bash
ipv6 unicast-routing
interface g0/0
 ipv6 address FE80::1 link-local
 ipv6 address 2001:db8:10::1/64
```

| 항목   | 요점             |
| ---- | -------------- |
| 주소유형 | GUA/LLA/ULA    |
| 전환기술 | 듀얼스택/터널/트랜슬레이션 |

---

## 🧷 18. 보안/접속

### 콘솔/VTY/사용자

```bash
line console 0
 password cisco
 login
 logging synchronous
 exec-timeout 0 0
!
line vty 0 4
 login local
 transport input ssh
!
username admin secret <SECRET>
service password-encryption
```

### SSH

```bash
hostname R1
ip domain-name lab.local
crypto key generate rsa modulus 2048
ip ssh version 2
```

---

## ⏱ 19. Syslog & NTP

```bash
service timestamps log datetime msec
logging 203.230.7.2
ntp server 203.230.7.2
```

---

## 💾 20. 저장/초기화/편의

| 명령어                                                   | 설명                    |
| ----------------------------------------------------- | --------------------- |
| `copy running-config startup-config` / `write memory` | 설정 저장                 |
| `reload`                                              | 재부팅                   |
| `erase startup-config`                                | 설정 초기화                |
| `delete vlan.dat`                                     | VLAN DB 초기화(스위치)      |
| `?`, `TAB`, `↑/↓`, `Ctrl+Z`, `Ctrl+Shift+6`           | 도움말/자동완성/히스토리/종료/세션중단 |

---

## 📌 참고 메모

- **Best Route = Longest Match** (최장일치)
- OSPF `auto-cost reference-bandwidth`는 **모든 라우터 동일** 권장
- VTP 구성 시 **도메인/비밀번호/버전** 일치 확인, 필요 시 `vtp mode transparent`로 안전하게 독립 운용
- 라우팅 필터는 **재분배/인접 업데이트/설치(RIB)** 각각의 시점이 다르므로 목적에 맞는 도구 선택

