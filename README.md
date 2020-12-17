# SWIM: Scalable Weakly-consistent Infection-style Process Group Membership Protocol

Abhinandan Das, Indranil Gupta, Ashish Motivala*
Dept. of Computer Science, Cornell University
Ithaca NY 14853 USA
{asdas,gupta,ashish} @cs.cornell.ed

## Abstract

## 초록

Several distributed peer-to-peer applications require weakly-consistent knowledge of process group membership information at all participating processes. SWIM is a generic software module that offers this service for largescale process groups. The SWIM effort is motivated by the unscalability of traditional heart-beating protocols, which either impose network loads that grow quadratically with group size, or compromise response times or false positive frequency w.r.t. detecting process crashes. This paper reports on the design, implementation and performance of the SWIM sub-system on a large cluster of commodity PCs.

분산 피어 투 피어 응용 프로그램에서는 그룹에 참여하는 모든 프로세스 멤버십 정보에 대하여 [약한 일관성](http://happinessoncode.com/2017/07/29/cap-theorem-and-pacelc-theorem/)을 제공해야합니다. SWIM은 대규모 프로세스 그룹에 이 서비스를 제공하는 소프트웨어 모듈입니다. SWIM은 기존의 [하트 비트 프로토콜](https://en.wikipedia.org/wiki/Heartbeat_(computing))의 비확장성을 개선하고자 합니다. 하트 비트 프로토콜에서는 그룹 크기에 따라 네트워크 로드가 지수적으로 증가하고, 응답 시간 또는 프로세스 충돌 감지에 대한 오탐 빈도와 타협합니다. 이 논문은 대규모 상용 PC 클러스터에서 SWIM 하위 시스템의 설계, 구현 및 성능에 대해 보고합니다.

Unlike traditional heart-beating protocols, SWIM separates the failure detection and membership update dissemination functionalities of the membership protocol. Processes are monitored through an efficient peer-to-peer periodic randomized probing protocol. Both the expected time to first detection of each process failure, and the expected message load per member, do not vary with group size. Information about membership changes, such as process joins, drop-outs and failures, is propagated via piggybacking on ping messages and acknowledgments. This results in a robust and fast infection style (also epidemic or gossip-style) of dissemination.

기존의 하트 비트 프로토콜과 달리 SWIM은 멤버십 프로토콜의 장애 감지 및 멤버십 업데이트 전파 기능을 분리합니다. 프로세스는 효율적인 주기적 피어 투 피어 랜덤 진단 프로토콜을 통해 모니터링됩니다. 각 프로세스의 실패를 처음 감지하는 데 예상되는 시간과 구성원당 예상되는 메시지 로드는 그룹 크기에 따라 달라지지 않습니다. 프로세스 참여, 탈락 및 실패와 같은 멤버쉽 변경에 대한 정보는 Ping 메시지 및 승인에 대한 [피기백](http://www.ktword.co.kr/abbr_view.php?m_temp1=3242)을 통해 전파됩니다. 그 결과 견고하고 빠른 감염 스타일 (전염병 또는 가십 스타일)로 전파됩니다.

The rate of false failure detections in the SWIM system is reduced by modifying the protocol to allow group members to suspect a process before declaring it as failed - this allows the system to discover and rectify false failure detections. Finally, the protocol guarantees a deterministic time bound to detect failures. 

SWIM 시스템에서는 오탐 비율을 줄이기 위해 프로토콜을 수정하여 장애로 선언하기 전에 먼저 의심 상태로 관리하여 시스템이 잘못된 오류 감지를 발견하고 수정할 수 있도록 합니다. 마지막으로 이 프로토콜은 정해진 시간내 장애 감지를 보장합니다.

Experimental results from the SWIM prototype are presented. We discuss the extensibility of the design to a WAN-wide scale.

SWIM 프로토 타입의 실험 결과가 제공됩니다. WAN 규모로의 디자인의 확장성을 논의합니다.

Author last names are in alphabetical order. The authors were supported in part by NSF CISE grant 9703470, in part by DARPA/AFRLIFGA grant F30602-99-1-0532, and in part by a grant under NASA’s REE program, administered by JPL.)

* 저자의 성은 알파벳 순서로되어 있습니다. 저자는 NSF CISE 교부금 9703470, DARPA / AFRLIFGA 교부금 F30602-99-1-0532, 부분적으로 JPL이 관리하는 NASA의 REE 프로그램에 의한 교부금에 의해 부분적으로 지원되었습니다.
