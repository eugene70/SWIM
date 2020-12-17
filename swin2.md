## 2. Previous Work
## 2. 사전 작업

In traditional distributed all-to-all heartbeating failure detection algorithms, every group member periodically transmits a “heartbeat” message (with an incremented counter) to all other group members. A member M[i] is declared as failed by a non-faulty member M[j] when does not receive heartbeats from for some consecutive heartbeat periods.

전통적인 분산 전역 하트 비트 실패 감지 알고리즘에서 모든 그룹 멤버는 주기적으로 (증가 된 카운터와 함께) "하트 비트" 메시지를 다른 모든 그룹 멤버에게 보냅니다. 멤버 M[i]가 연속적인 하트 비트 기간 동안 하트 비트를 전송하지 못하면 결함이 없는 멤버 M[j]가 해당 멤버를 실패한 것으로 선언합니다.

Distributed heartbeating schemes guarantee that a faulty member is always detected as such at any non-faulty member (within a time interval after its failure)(Ibid.4, This property is called Strong Completeness), since a member that has crashed also stops sending heartbeat messages. However, the accuracy and scalability guarantees of these protocols differ, depending on the actual mechanism used to disseminate the heartbeats.

분산 하트 비트 방식은 고장이 발생한 멤버가 하트 비트 메시지 전송을 중지함으로서  (결함 후 일정 시간 내) 항상 결함이 없는 멤버에게 감지되도록합니다. 그러나 이러한 프로토콜의 정확성과 확장성 보장은 하트 비트를 전파하는 데 사용되는 실제 메커니즘에 따라 다릅니다.

In the simplest implementation, each heartbeat is multicasted to all other group members. This results in a network load of θ(n^2/T) messages per second (even if IP multi- cast is used), where is the failure detection time required by the distributed application. van Renesse et al. [16] proposed that heartbeats be disseminated via a robust gossip-style protocol. In this protocol, every time units, each member gossips, to a few random targets, a θ(n)-sized list of the latest known heartbeat counters received from other members. While gossiping reduces the false positive frequency, a new heartbeat count typically takes, on expectation, time units to reach an arbitrary other group member. In order to satisfy the application- specified detection time, the protocol generates a network load of θ(n^2·log(n)/t[gossip]) bytes a second. The use of message batching to solve this is limited by the UDP packet size limit, e.g. 5B heartbeats (IP address and count) of 50 members would already occupy 250 B, while SWIM generates packets that have a size of at most 135 B, regardless of the group size.

가장 간단한 구현에서 각 하트 비트는 다른 모든 그룹 구성원에게 멀티 캐스트됩니다. 이로 인해 분산 응용 프로그램에 필요한 장애 감지 시간이있는 곳에서 초당 θ(n^2/T) 메시지의 네트워크로드가 발생합니다 (IP 멀티 캐스트를 사용하는 경우에도). van Renesse 등은 [16]에서 강력한 가십 스타일 프로토콜을 통해 하트 비트를 전파 할 것을 제안했습니다. 이 프로토콜에서, 각 구성원은, 매 t[gossip] 시간마다, 소수의 임의의 대상으로, 다른 구성원들로 부터 수신 한 최신 알려진 θ(n) 크기 하트 비트 카운터 목록을 소문냅니다. 소문은 오탐 빈도를 감소시키는 반면, 새로운 하트 비트 수는 일반적으로 임의의 다른 그룹 구성원에 도달하는 데  θ[log (n)·t[gossip]] 시간이 소요될 것으로 기대합니다. 응용 프로그램 지정 감지 시간을 만족시키기 위해 프로토콜은 초당 θ(n^2·log(n)/t[gossip]) 바이트의 네트워크 로드를 생성합니다. 이를 해결하기 위해 메시지 일괄 처리를 사용하는 것은 UDP 패킷 크기 제한에 의해 제한됩니다 (예: 50 멤버의 5바이트 하트 비트(IP 주소 및 카운트)는 이미 250 바이트를 차지하지만 SWIM은 그룹 크기에 관계없이 최대 135 바이트의 패킷을 생성합니다.

The quadratic increase in the network load results from the communication of heartbeat notification to all group members. This can be avoided by separating the failure detection operation from that of membership update dissemination.

네트워크 로드의 지수함수적 증가는 그룹 전체에 하트 비트 알림을 보내는 것에 기인합니다. 이는 장애 감지 작업을 멤버십 업데이트 전파와 분리함으로서 피할 수 있습니다.

Several hierarchical membership systems have been proposed, e.g. Congress [1]. This belongs to a broader class of solutions where each process heartbeats only a subgroup of processes. This class of protocols requires careful configuration and maintenance of the overlay along which membership information flows, and the accuracy of the protocol depends on the robustness of this graph. In comparison, the design of SWIM avoids the overhead of a virtual graph.

여러 계층 적 멤버십 시스템이 제안되었습니다 (예: Congress [1]. 이는 각 프로세스 하트 비트가 하위 프로세스 그룹에만 해당하는 광범위한 솔루션 클래스에 속합니다. 이 프로토콜 범주는 멤버십 정보가 흐르는 구조를 신중하게 구성하고 유지 보수해야하며 프로토콜의 정확성은 이 그래프의 견고성에 달려 있습니다. 이에 비해 SWIM의 설계는 가상 그래프의 오버 헤드를 피합니다.

SWIM’s solution to the above unscalability problems described above is based on (a) designing the failure detection and membership update dissemination components separately, and (b) using a non-heartbeat based strategy for failure detection.

위에서 설명한 비확장성 문제에 대한 SWIM의 솔루션은 (a) 오류 감지 및 멤버십 업데이트 배포 구성 요소를 별도로 설계하고 (b) 하트 비트 기반이 아닌 실패 감지전략을 사용합니다.

Before moving on to describe the SWIM protocol internals, we first lay the foundation for understanding the key characteristics of the efficiency and scalability of distributed failure detector protocols. Several research studies [6, 7, 12, 16], have led to the identification of these basic properties of distributed failure detector protocols (from both theoretical and practical angles), as well as impossibility results related to satisfying them concurrently. The resulting tradeoff is usually determined by the safety and liveness properties required by distributed applications.

SWIM 프로토콜 내부를 설명하기 전에 먼저 분산 장애 감지 프로토콜의 효율성과 확장성의 주요 특성을 이해하기 위한 기반을 마련했습니다. 여러 연구 [6, 7, 12, 16]는 분산 장애 감지 프로토콜의 이러한 기본 특성을 (이론적 및 실제적 각도에서) 동시에 이를 만족시키는 것은 불가능하다는 결과를 도출해왔습니다. 이런 트레이드 오프 결과는 일반적으로 분산 응용 프로그램에 필요한 안전 및 활력 속성에 의해 결정됩니다.

These properties are [12]:

(1) Strong Completeness: crash-failure of any group member is detected by all non-faulty members [6]);

(2) Speed of failure detection: the time interval between a member failure and its detection by some non-faulty group member;

(3) Accuracy: the rate of false positives of failure detection;

(4) Network Message Load, in bytes per second generated by the protocol.


이러한 속성은 [12]에 있습니다:

(1) 강력한 완전성: 고장난 멤버는 다른 결함이 없는 구성원에 의해 감지됩니다 [6]);

(2) 장애 탐지 속도: 장애 발생과 결함이 아닌 그룹 구성원에 의한 감지될 때까지의 시간 간격;

(3) 정확도: 장애 감지의 오탐율; 

(4) 프로토콜에 의해 생성 된 초당 바이트 단위의 네트워크 메시지로드.



[6] proved the impossibility of building a failure detector over an asynchronous network that is both accurate (no false detections) and strongly complete. However, since a typical distributed application relies on Strong Completeness always holding (in order to maintain up to date information in dynamic groups), most failure detectors, including heartbeating-based solutions, guarantee this property while attempting to maintain a low rate of false positives. SWIM takes the same approach.

[6]에서는 비동기 네트워크 내에서 정확하고 (잘못 탐지되지 않은) 완벽한 장애 탐지기를 구축 할 수 없음을 증명하였습니다. 그러나 일반적인 분산 응용 프로그램은 (동적 그룹의 최신 정보를 유지하기 위해) 항상 보유하는 강력한 완전성에 의존하기 때문에 하트 비트 기반 솔루션을 포함한 대부분의 오류 탐지기는 낮은 오탐률을 유지하면서 이 속성을 보장합니다. . SWIM도 같은 접근 방식을 취합니다.

In [12], a simple computation identifies the minimal total network load (bytes per second) required to satisfy specified parameters of false detection rate at each member (denoted PM(T)), and detection time (T) in a group of size n. [12] calculates this load as n·(log(PM(T))/log(Pml)), where Pml is the probability of a packet drop within the underlying network.

[12]에서, 한 간단한 수식은 n 크기 그룹의 각 구성원에서의 오탐율(PM (T)로 표시)과 탐지 시간(T)을 만족하기 위한 최소 총 네트워크 로드(초당 바이트)를 식별합니다. [12]에서는 이 부하를 n · (log (PM (T)) / log (Pml))로 계산하며, 여기서 Pml은 기본 네트워크 내에서 패킷을 잃을 확률입니다.

Although this calculation is done under idealized conditions of independent message loss probabilities on each message (Pml), it serves as a good baseline for comparing the scalability of different failure detection protocols. For example, the all-to-all heartbeat protocols discussed in Section 2 have a sub-optimality factor that varies linearly with group size.

이 계산은 각 메시지에 대한 독립적 인 메시지 손실 확률(Pml)의 이상적인 조건 하에서 수행되지만 다른 오류 감지 프로토콜의 확장성을 비교하기위한 좋은 기준으로 사용됩니다. 예를 들어, 2장에서 논의된 전역 하트 비트 프로토콜은 그룹 크기에 따라 선형적으로 변하는 하위 최적화 요소를 갖습니다.
