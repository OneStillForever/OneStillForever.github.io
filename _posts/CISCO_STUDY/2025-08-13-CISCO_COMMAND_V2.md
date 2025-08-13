---
layout: post
title: "CISCO_COMMAND_V2"
categories: [CISCO_STUDY]
date: 2025-08-13
---

# ✅ Cisco IOS CLI 명령어 통합 정리 (풀버전)

> 장비/IOS에 따라 일부 명령어가 다를 수 있습니다. (Catalyst/ISR/ASR 등)

---

## 📁 1. 기본 CLI 조작

| 명령어 | 설명 |
| --- | --- |
| `enable` / `disable` | 사용자 ↔ 특권 모드 전환 |
| `configure terminal` | 전역 설정 모드 진입 |
| `exit` / `end` | 모드 종료 (한 단계/전역) |
| `hostname <NAME>` | 장비 이름 변경 (SSH 키 생성에 필요) |
| `banner motd #...#` | MOTD 배너 설정 |
| `no ip domain-lookup` | 오타로 인한 DNS 조회 방지 |
| `clock set HH:MM:SS DD MMM YYYY` | 수동 시간 설정 |
| `show clock` | 현재 시간 확인 |
| `license ?` | 라이선스 관련(장비별 상이) |
| `sdm prefer dual-ipv4-and-ipv6 default` | 듀얼스택 SDM 템플릿(스위치, 재부팅 필요) |

---

## 🔍 2. 상태 확인 / Show / Debug

| 명령어 | 설명 |
| --- | --- |
| `show running-config` / `show startup-config` | 현재/부팅 설정 |
| `show ip interface brief` | IP/인터페이스 요약 |
| `show interfaces` | 인터페이스 상세 |
| `show interfaces counters errors` | 인터페이스 에러 확인 |
| `show version` | IOS/업타임/메모리 |
| `show vlan brief` | VLAN 요약 |
| `show mac address-table` | MAC 테이블 |
| `show arp` | ARP 캐시 |
| `show cdp neighbors` / `show cdp ne` | CDP 인접 요약 |
| `show cdp neighbors detail` | CDP 인접 상세 |
| `show ip route` / `show ipv6 route` | 라우팅 테이블 |
| `show ip protocols` | 라우팅 프로토콜 상태 |
| `show ip dhcp binding` | DHCP 임대 현황 |
| `show vtp status` | VTP 상태 |
| `show access-lists` | ACL 확인 |
| `show aaa servers` / `show radius` | AAA/RADIUS 상태 |
| `show dot1x all` | 802.1X 상태 |
| `show standby` / `show standby brief` | HSRP 상태 |
| `show etherchannel summary` / `show etherchannel detail` | EtherChannel 상태 |
| `show ip nat translation` | NAT 변환 테이블 |
| `show ip nat statistics` | NAT 통계 |
| `show logging` | 로그 확인 |
| `debug ip rip` / `debug ospf adj` / `undebug all` | 디버깅 / 해제 |

> CDP가 꺼져있다면 `cdp run`(글로벌), `cdp enable`(인터페이스)로 활성화

---

## 🌐 3. IP & 인터페이스 설정

```bash
interface g0/1
 ip address 192.168.1.1 255.255.255.0
 no shutdown
````

| 명령어                                  | 설명             |
| ------------------------------------ | -------------- |
| `interface <NAME>`                   | 인터페이스 진입       |
| `ip address <IP> <MASK> [secondary]` | IPv4 주소 설정     |
| `ipv6 address <ADDR>/<LEN>`          | IPv6 주소 설정     |
| `no shutdown` / `shutdown`           | 인터페이스 활성/비활성   |
| `ip default-gateway <IP>`            | L2 스위치 기본 GW   |
| `clock rate <Kbps>`                  | DCE 클럭 설정(시리얼) |
| `interface loopback <N>`             | 루프백 생성         |

---

## 📦 4. VLAN / 스위칭 / Router-on-a-Stick

```bash
vlan 10
 name IT
interface f0/1
 switchport mode access
 switchport access vlan 10
```

| 명령어                                    | 설명             |
| -------------------------------------- | -------------- |
| `vlan <ID>` / `name <NAME>`            | VLAN 생성/이름     |
| `switchport mode access`               | 액세스 포트         |
| `switchport access vlan <ID>`          | 액세스 포트 VLAN 지정 |
| `switchport mode trunk`                | 트렁크 포트         |
| `switchport trunk encapsulation dot1q` | 802.1Q 지정      |
| `switchport trunk allowed vlan <list>` | 트렁크 허용 VLAN 목록 |
| `switchport trunk native vlan <ID>`    | 네이티브 VLAN 지정   |
| `interface vlan <ID>`                  | SVI 생성/진입      |
| `ip helper-address <IP>`               | DHCP 릴레이       |
| `vtp mode {server/client/transparent}` | VTP 모드 설정      |
| `vtp domain <NAME>`                    | VTP 도메인 지정     |
| `vtp password <PWD>`                   | VTP 패스워드 설정    |

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
ipv6 route 2001:abcd::/64 2001:abcd::1
```

| 명령어                                 | 설명         |
| ----------------------------------- | ---------- |
| `ip route <DEST> <MASK> <NH/IP>`    | IPv4 정적 경로 |
| `ipv6 route <PREFIX>/<LEN> <NH/IP>` | IPv6 정적 경로 |

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

| 기능     | 명령어                                                                        |
| ------ | -------------------------------------------------------------------------- |
| 요약     | `ip summary-address rip <IP> <MASK>`                                       |
| 인증 평문  | `ip rip authentication mode text` + `ip rip authentication key-chain <KC>` |
| 인증 MD5 | `ip rip authentication mode md5` + `ip rip authentication key-chain <KC>`  |
| 키체인    | `key chain <KC>` → `key <N>` → `key-string <STR>`                          |

---

## 🚦 7. OSPF (Single / Multi Area)

```bash
router ospf 1
 router-id 1.1.1.1
 network 192.168.1.0 0.0.0.255 area 0
 default-information originate always
 auto-cost reference-bandwidth 10000
```

| 기능           | 명령어                                                                                  |
| ------------ | ------------------------------------------------------------------------------------ |
| 네트워크 등록      | `network <IP> <WC> area <AREA>`                                                      |
| 기본경로 광고      | `default-information originate [always]`                                             |
| 인증 평문        | `ip ospf authentication` + `ip ospf authentication-key <PWD>`                        |
| 인증 MD5       | `ip ospf authentication message-digest` + `ip ospf message-digest-key <N> md5 <PWD>` |
| Stub         | `area <A> stub [no-summary]`                                                         |
| NSSA         | `area <A> nssa [no-summary] [default-information-originate]`                         |
| Area 요약      | `area <A> range <IP> <MASK>`                                                         |
| ASBR 외부 요약   | `summary-address <IP> <MASK>`                                                        |
| Virtual-Link | `area <TRANSIT> virtual-link <RID>`                                                  |
| 참조대역폭        | `auto-cost reference-bandwidth <Mbps>`                                               |
| 타이머          | `ip ospf hello-interval <S>` / `ip ospf dead-interval <S>`                           |

### Redistribute

| 대상           | 명령어                                             |
| ------------ | ----------------------------------------------- |
| 정적 → OSPF    | `redistribute static subnets`                   |
| EIGRP → OSPF | `redistribute eigrp <AS> subnets metric-type 1` |
| 기본메트릭        | `default-metric <COST>`                         |
| Route-map 필터 | `redistribute static route-map <RM>`            |

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

| 기능        | 명령어                                                                                     |
| --------- | --------------------------------------------------------------------------------------- |
| 인터페이스 타이머 | `ip hello-interval eigrp <AS> <S>` / `ip hold-time eigrp <AS> <S>`                      |
| 대역폭 반영    | `bandwidth <Kbps>`                                                                      |
| 요약        | `ip summary-address eigrp <AS> <IP> <MASK>`                                             |
| 인증 MD5    | `ip authentication mode eigrp <AS> md5` + `ip authentication key-chain eigrp <AS> <KC>` |

### Named Mode

```bash
router eigrp CCNP
 address-family ipv4 unicast autonomous-system 100
  af-interface default
   authentication mode h
```


mac-sha256 <KEY>
summary-address 10.0.0.0 255.255.0.0
topology base
variance 2

````

---

## 🧭 9. 라우팅 필터링

| 방식 | 명령어 | 비고 |
| --- | --- | --- |
| Distribute-list (ACL) | `access-list 10 permit 192.168.1.0 0.0.0.255` → `router eigrp 100` → `distribute-list 10 in` | 프로토콜별 지원 범위 상이 |
| Prefix-list | `ip prefix-list PFX deny 10.0.0.0/8` → `neighbor 1.1.1.1 prefix-list PFX in` (BGP) | CIDR 정밀 |
| Route-map | `route-map RM deny 10` + `match ip address prefix-list PFX` → `redistribute static route-map RM` | 조건부 필터 |
| Passive-interface | `passive-interface <IF>` | 광고 차단(수신 가능) |
| PBR | `route-map PBR permit 10` + `match ip address <ACL>` + `set ip next-hop <IP>` → `(IF) ip policy route-map PBR` | 정책 기반 라우팅 |

---

## 🧱 10. ACL (Access Control List)

```bash
ip access-list extended BLOCK_SSH
 deny tcp any any eq 22
 permit ip any any
!
interface g0/0
 ip access-group BLOCK_SSH in
````

| 유형       | 명령어                                      |             |
| -------- | ---------------------------------------- | ----------- |
| 표준       | `access-list <1-99,1300-1999> ...`       |             |
| 확장       | `access-list <100-199,2000-2699> ...`    |             |
| 이름 기반    | \`ip access-list {standard               | extended}\` |
| 인터페이스 적용 | \`(IF) ip access-group <ACL> in          | out\`       |
| VTY 제한   | `line vty 0 4` → `access-class <ACL> in` |             |

---

## 🧰 11. Route-Map

```bash
route-map SET-NH permit 10
 match ip address 101
 set ip next-hop 192.0.2.1
```

| 용도     | 명령어                                  |       |
| ------ | ------------------------------------ | ----- |
| PBR    | `(IF) ip policy route-map <RM>`      |       |
| 재분배 제어 | `redistribute static route-map <RM>` |       |
| BGP 정책 | \`neighbor X route-map <RM> in       | out\` |

---

## 🧩 12. HSRP

```bash
interface g0/1
 standby version 2
 standby 1 ip 192.168.1.254
 standby 1 priority 110
 standby 1 preempt
 standby 1 authentication md5 key-string cisco
 standby 1 track GigabitEthernet0/2 decrement 10
```

| 기능    | 명령어                                          |
| ----- | -------------------------------------------- |
| 상태 확인 | `show standby` / `show standby brief`        |
| 트래킹   | `standby <GROUP> track <IF> decrement <VAL>` |

---

## 🔧 13. NAT

```bash
ip nat inside source list 1 interface g0/0 overload
access-list 1 permit 192.168.1.0 0.0.0.255
interface g0/0
 ip nat outside
interface g0/1
 ip nat inside
```

| 기능       | 명령어                                                      |
| -------- | -------------------------------------------------------- |
| NAT 풀    | `ip nat pool <NAME> <START> <END> netmask <MASK>`        |
| NAT 설정   | `ip nat inside source list <ACL> pool <POOL> [overload]` |
| 인터페이스 지정 | `(IF) ip nat inside / ip nat outside`                    |
| 변환 확인    | `show ip nat translation`                                |
| 통계 확인    | `show ip nat statistics`                                 |

---

## ⚡ 14. AAA / 802.1X / RADIUS

| 기능      | 명령어                                                   |
| ------- | ----------------------------------------------------- |
| 로컬 사용자  | `username <USER> privilege <PRIV> secret <PWD>`       |
| AAA 활성화 | `aaa new-model`                                       |
| 인증 서버   | `aaa authentication login default group radius local` |
| 802.1X  | `dot1x system-auth-control`                           |
| 포트 인증   | `interface f0/1` → `dot1x port-control auto`          |

---

## 🛠 15. 디버깅 & 운영

| 명령어                                | 설명                 |
| ---------------------------------- | ------------------ |
| `debug ip rip`                     | RIP 디버그            |
| `debug ospf adj`                   | OSPF 인접 디버그        |
| `debug eigrp packets`              | EIGRP 패킷 디버그       |
| `undebug all`                      | 모든 디버그 해제          |
| `show logging`                     | 로그 확인              |
| `show interfaces counters errors`  | 인터페이스 에러 확인        |
| `show ip route summary`            | 라우팅 요약 확인          |
| `show cdp neighbors`               | CDP 인접 확인          |
| `show etherchannel summary/detail` | EtherChannel 상태 확인 |

```

