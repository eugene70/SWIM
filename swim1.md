## 1. Introduction
## 1. 소개

As you swim lazily through the milieu, The secrets of the world will infect you.

당신이 주변을 느리게 헤엄칠 때, 세상의 비밀이 당신을 감염시킨다.

Several large-scale peer-to-peer distributed process groups running over the Internet rely on a distributed membership maintenance sub-system. Examples of existing middleware systems that utilize a membership protocol include reliable multicast [3, 11], and epidemic-style information dissemination [4, 8, 13]. These protocols in turn find use in applications such as distributed databases that need to reconcile re- cent disconnected updates [14], publish-subscribe systems, and large-scale peer-to-peer systems[15]. The performance of other emerging applications such as large-scale cooperative gaming, and other collaborative distributed applications, depends critically on the reliability and scalability of the membership maintenance protocol used within.

인터넷을 통해 실행되는 몇몇 대규모 P2P 분산 프로세스 그룹은 분산 멤버십 유지 관리 서브 시스템에 의존합니다. 멤버십 프로토콜을 사용하는 기존 미들웨어 시스템의 사례로는 신뢰할 수있는 멀티 캐스트[3, 11] 및 전염병 스타일 정보 전파 [4, 8, 13]가 있습니다. 이러한 프로토콜은 최근 연결이 끊긴 업데이트[14], 게시-구독 시스템 및 대규모 피어 투 피어 시스템[15]을 조정해야하는 분산 데이터베이스와 같은 응용 프로그램에서 사용됩니다. 대규모 협동 게임 및 분산 협업 응용 프로그램과 같은 최신 응용 프로그램의 성능은 사용 된 멤버십 유지 관리 프로토콜의 안정성과 확장성에 크게 좌우됩니다.

Briefly, a membership protocol provides each process (“member”) of the group with a locally-maintained list of other non-faulty processes in the group. The protocol en- sures that the membership list is updated with changes re- sulting from new members joining the group, or dropping out (either voluntarily or through a failure). The member- ship list is made available to the application either directly in its address space, or through a callback interface or an API. The application is free to use the contents of the list as required, e.g. gossip-based dissemination protocols would use the list to periodically pick target members for gossip.

간단히 말하면, 멤버십 프로토콜은 그룹의 각 프로세스( "멤버")에게 그룹내의 장애가 없는 다른 프로세스의 로컬 관리 목록을 제공합니다. 이 프로토콜은 멤버십 목록이 그룹에 참여하거나 탈퇴(자발적 또는 실패를 통해)한 결과로 멤버십 목록이 업데이트되도록 합니다. 멤버십 목록은 멤버의 주소 공간에 직접 또는 콜백 인터페이스를 통해 접근하거나 API를 통해 애플리케이션에 제공됩니다. 응용 프로그램은 필요에 따라 목록의 내용을 자유롭게 사용할 수 있습니다. 즉, 가십 기반 전파 프로토콜은 이 목록을 사용하여 가십을 위한 대상 구성원을 주기적으로 추릴 것입니다. 

The reliability and scalability of a membership sub- system can be measured via several performance metrics. Membership changes have to be propagated within the group quickly after their occurrence. The asynchrony and unreliability of the underlying network can cause messages to be lost, leading to false detection of process failures, since a process that is losing messages is indistinguishable from one that has failed [10]. This rate of false positives has to be low. Finally, the protocol needs to be peer-to-peer (not rely on a central server), and impose low message and computation loads on the network and processes.

멤버십 서브 시스템의 안정성과 확장성은 여러 성능 지표를 통해 측정 할 수 있습니다. 멤버십 변경은 발생 즉시 그룹 내에서 빠르게 전파되어야합니다. 기반 네트워크의 비동기성과 비신뢰성으로 인해 메시지가 손실 될 수 있으며, 메시지 손실 프로세스는 실패한 프로세스와 구분할 수 없으므로 프로세스 실패를 잘못 감지 할 수 있습니다 [10]. 이 오 탐지율은 다음과 같습니다. 마지막으로 프로토콜은 피어 투 피어 (중앙 서버에 의존하지 않음) 여야하고 네트워크 및 프로세스에 낮은 메시지 및 계산로드를 부과해야합니다.

Membership protocols have been difficult to scale in groups with beyond a few dozen processes [11, 16], thus affecting the performance of applications using them. As reported in [16], the main symptoms of bad performance at these group sizes is an increase in either the rate of false failure detections of processes, or the time to detect a failure. [12] identifies the quadratic increase in the message load imposed by such membership protocols as another symptom of the unscalability of traditional protocols for membership maintenance. An example of an application that relies heavily on the membership sub-system is the class of virtually synchronous multicast protocols [3]. Traditional implementations of this specification suffer a drastic reduction in performance, and partitioning, at beyond a few dozen members [11].

멤버십 프로토콜은 수십 개의 프로세스를 넘는 그룹으로 확장하기 어려워서 [11, 16], 이를 사용하는 애플리케이션의 성능에 영향을 미칩니다. [16]에 보고 된 바와 같이, 이들 그룹 규모에서 나쁜 성능의 주된 증상은 프로세스의 장애 오탐 비율 또는 탐지 시간의 증가입니다. [12]는 멤버쉽 유지 보수를 위한 전통적인 프로토콜의 확장이 어려운 증거로써 이 프로토콜에서 부과 된 메시지 로드의 지수함수적 증가를 보여줍니다. 멤버십 서브 시스템에 크게 의존하는 어플리케이션의 예는 가상 동기식 멀티 캐스트 프로토콜의 클래스입니다 [3]. 이 사양의 전통적인 구현으로는 수십 개 이상의 멤버에서 극적인 성능 저하와 분절화를 겪을 것입니다. [11].

This paper presents our effort in the SWIM project to implement a membership sub-system that provides stable failure detection time, stable rate of false positives and low message load per group member, thus allowing distributed applications that use it to scale well. We focus on a weaker variant of group membership, where membership lists at different members need not be consistent across the group at the same (causal) point in time. Stronger guarantees could be provided by augmenting the membership sub-system, e.g. a virtually-synchronous style membership can be provided through a sequencer process that checkpoints the membership list periodically. However, unlike the weakly consistent problem, strongly consistent specifications might have fundamental scalability limitations(Ibid.1, Discussion of this issue is outside the scope of this paper. The reader is referred to [11]).

이 논문은 SWIM 프로젝트에서 멤버십 하위 시스템을 구현하여 안정적인 장애 감지 시간과 탐지율 및 그룹 구성원 대비 낮은 메시지로드를 제공하여 이를 사용하는 분산 응용 프로그램을 확장 할 수 있도록하는 활동을 제시합니다. 우리는 그룹을 가로질러 존재하는 구성원이 동일한 (인과적) 시점에 일관성을 요구하지 않는 변형된 약한 그룹 멤버십에 중점을 둡니다. 멤버십 서브 시스템을 보강하여 더 강력한 보증을 제공 할 수 있습니다. 예를 들어, 멤버십 리스트를 주기적으로 체크 포인트하는 시퀀서 프로세스를 통해 가상 동기식 멤버십을 제공 할 수 있습니다. 그러나 약한 일관성 문제와 달리 강한 일관성 사양에서는 기본적인 확장성 제한이있을 수 있습니다(주1, 이 문제에 대한 논의는이 문서의 범위를 벗어납니다. 독자는 [11]을 참조하십시오).

The design of a distributed membership algorithm(Ibid.2, A “weakly-consistent” adjective is implicitly assumed, and dropped henceforth) has traditionally been approached through the technique of heart-beating. Each process periodically sends out an incremented heartbeat counter to the outside world. Another process is detected as failed when a heartbeat is not received from it for some time. However, actual implementations of heart-beating suffer from scalability limitations. Sending all heartbeats to a central server leads to hot-spot creation. Sending heartbeats to all members (through either network multicast, or gossiping [16]) leads to a message load on the network and group that grows quadratically with the group size. Heartbeating along a logical ring [9] suffers from unpredictability of failure detection time when there are multiple failures. Unfortunately, as the group size rises, so does the likelihood of simultaneous multiple failures.

분산 멤버십 알고리즘(주2, "약한 일관성" 형용사는 암시적으로 가정되어 이후에는 생략합니다)의 설계는 전통적으로 하트 비트 기술을 통해 접근합니다. 각 프로세스는 주기적으로 증가 된 하트 비트 카운터를 외부 세계로 보냅니다. 하트 비트가 일정 시간 동안 수신되지 않으면 다른 프로세스가 실패한 것으로 감지합니다. 그러나 실제 하트 비트 구현에는 확장성 제한이 있습니다. 모든 하트 비트를 중앙 서버로 보내면 핫스팟이 생성됩니다. 네트워크 멀티 캐스트 또는 가십[16]을 통해 모든 구성원에게 하트 비트를 보내면 그룹 크기에 따라 지수함수적으로 증가하는 네트워크 및 그룹에 메시지로드가 발생합니다. 논리적 링 방식의 하트 비트 [9]는 여러 번의 고장이있을 때 예측할 수없는 고장 감지 시간으로 어려움을 겪습니다. 불행히도 그룹 규모가 증가함에 따라 동시 다중 장애 가능성도 높아집니다.

An extended discussion of reasons behind the inherent unscalability of heartbeat-based membership maintenance mechanisms can be found in [12]. This paper also proposed a randomized distributed failure detector protocol based on members randomly probing each other instead of heart-beating(Ibid.3, In a sense, the protocol monitors the status of members, randomly, instead of using heartbeating). Mathematical analysis showed that as the group size is scaled up, the protocol’s properties of (expected) failure detection time, rate of false positives, and message load per member, are all independent of the group size. This is an improvement over all-to-all heart-beating based protocols that have a linear variation (with group size) of either the detection time for failures or the network bandwidth usage at each member (or an increase in the false positive rate).

하트 비트 기반 멤버십 유지 메커니즘의 근본적인 확장 불가능성의 이유에 대한 자세한 논의는 [12]에서 확인할 수 있습니다. 이 논문은 또한 하트 비트 대신에 무작위로 서로를 조사하는 회원들을 기반으로 한 무작위 분산 장애 탐지 프로토콜을 제안했습니다(주3, 어떤 의미에서, 프로토콜은 하트 비트를 사용하는 대신 무작위로 구성원의 상태를 모니터합니다). 수학적 분석에 따르면 그룹 크기가 확장됨에 따라 (예상되는) 실패 감지 시간, 오 탐지율 및 구성 원당 메시지 로드의 프로토콜 속성은 모두 그룹 크기와 무관합니다. 이는 장애 감지 시간 또는 각 구성원의 네트워크 대역폭 사용량 (또는 오 탐지율 증가)의 선형적인 변동(그룹 크기 대비)이 있는 전체 대 전체 하트 비트 기반 프로토콜에 비해 개선 된 것입니다.

Our work in this article is motivated by a realization from the work of [12] that the unscalability of the popular class of all-to-all heart-beating protocols arises from the implicit decision therein to fuse the two principal functions of the membership problem specification: 1) Membership update Dissemination: propagating membership updates arising from processes joining, leaving or failing, and 2) Failure detection: detecting failures of existing members. The overhead of multicasting heartbeats is eliminated by designing an efficient non-multicast based failure detector, and using the dissemination component only when a membership change occurs. The Membership Dissemination component can be implemented through either hardware multicast or in infection-style.
이 글에서 우리의 연구는 인기있는 모든 종류의 하트 비트 프로토콜의 비확장성이 구성원 문제의 다음 두 가지 주요 기능을 융합시키기위한 암묵적 결정에서 비롯된다는 [12]의 연구로 부터 출발하였습니다: 1) 멤버십 업데이트 배포: 참여, 탈퇴 또는 실패한 프로세스에서 발생하는 멤버십 업데이트 전파, 2) 오류 감지: 기존 멤버의 실패 감지. 효율적인 비 멀티 캐스트 기반 장애 탐지기를 설계하고 멤버십 변경이 발생할 때만 구성 요소 전파를 사용하면 멀티 캐스팅 하트 비트의 오버 헤드가 제거됩니다. 멤버십 구성 요소 전파는 하드웨어 멀티 캐스트 또는 감염 스타일을 통해 구현 될 수 있습니다.

While [12] presented a failure detection protocol and analyzed it theoretically, our work in the current paper looks at incorporating the Membership Dissemination component in to build a working membership sub-system. In addition, the resulting protocol is augmented by mechanisms that reduce the rate of false positives and give stronger deterministic guarantees on failure detection times at individual processes.

Our system, called SWIM, provides a membership substrate that:

[12]는 장애 감지 프로토콜을 제시하고 이론적으로 분석했지만, 이 논문에서의 우리의 작업은 멤버십 배포 구성 요소를 통합하여 멤버십 하위 시스템을 구축하는 방법을 살펴 봅니다. 또한 개발된 프로토콜은 오 탐지율을 낮추고 개별 프로세스에서 오류 감지 시간을 보다 확실하게 결정하는 메커니즘으로 보강됩니다. SWIM이라고 부르는 이 시스템은 다음과 같은 멤버십 산출물을 제공합니다.

(1) imposes a constant message load per group member; (2) detects a process failure in an (expected) constant time at some non-faulty process in the group;

(3) provides a deterministic bound (as a function of group size) on the local time that a non-faulty process takes to detect failure of another process;

(4) propagates membership updates, including information about failures, in infection-style (also gossip-style or epidemic-style [2, 8]); the dissemination latency in the group grows slowly (logarithmically) with the number of members;

(5) provides a mechanism to reduce the rate of false positives by “suspecting” a process before “declaring” it as failed within the group.

(1) 그룹 구성원마다 일정한 메시지로드를 부과합니다; 

(2) 그룹에서 결함이없는 일부 프로세스에서 (예상한) 일정한 시간의 프로세스 실패를 감지합니다; 

(3) 결함없는 프로세스가 다른 프로세스의 실패를 감지하는 데 걸리는 현지 시간에 대한 결정론적 한계 (그룹 크기의 함수)를 제공합니다;

(4) 감염 스타일 (가십 스타일 또는 전염병 스타일 [2, 8])에서 실패에 대한 정보를 포함하여 멤버십 업데이트를 전파합니다. 그룹의 보급 지연 시간은 구성원 수에 따라 (대수적으로) 느리게 증가합니다;

(5) 프로세스가 그룹 내에서 실패한 것으로 "선언"하기 전에 프로세스를 "의심"하여 오 탐지율을 감소시키는 메커니즘을 제공합니다.

While (1) and (2) are properties of the failure detection protocol of [12], (3)-(5) represent our subsequent work in the current paper. Experimental results of a prototype implementation of SWIM running on a PC cluster are discussed. The SWIM protocol can also be extended to work over a wide area network (WAN) or virtual private network (VPN), and we touch on this briefly in Section 6.

(1)과 (2)는 [12]의 장애 감지 프로토콜의 속성 인 반면, (3)-(5)는 현재 논문에서 우리의 후속 작업을 나타냅니다. PC 클러스터에서 실행되는 SWIM 프로토 타입 구현의 실험 결과가 논의됩니다. SWIM 프로토콜은 WAN (Wide Area Network) 또는 VPN (Virtual Private Network)을 통해 작동하도록 확장 할 수도 있으며, 6 장에서 이를 간략하게 설명합니다.

The rest of the paper is organized as follows. Section 2 summarizes previous work in this area, and the basics of scalable failure detection protocols from [12]. Section 3 describes the basic SWIM protocol, and Section 4 the improvements to the protocol. Experimental results from a prototype implementation are presented in Section 5. We conclude in Section 6.

논문은 다음과 같이 구성되어 있습니다. 2 장에서는 이 분야의 이전 연구와 [12]의 확장 가능한 장애 감지 프로토콜의 기본 사항을 요약합니다. 3 장에서는 기본 SWIM 프로토콜에 대해 설명하고 4 장에서는 프로토콜 개선 사항에 대해 설명합니다. 프로토타입 구현의 실험 결과는 5 장에 나와 있습니다. 6 장에서 결론을 내립니다.
