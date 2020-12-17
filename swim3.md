## 3. The Basic SWIM Approach
## 3. 기본 SWIM 접근

As mentioned earlier, the SWIM approach has two components:

(1) a Failure Detector Component, that detects failures of members, and

(2) a Dissemination Component, that disseminates information about members that have recently either joined or left the group, or failed.

앞에서 언급했듯이 SWIM 방식에는 두 가지 구성 요소가 있습니다.

(1) 멤버의 고장을 감지하는 장애 감지기

(2) 최근에 그룹에 가입했거나 탈퇴했거나 실패한 회원에 대한 정보를 유포하는 전파기.

We now lay the ground by describing the basic SWIM protocol. The basic protocol uses the random-probing based failure detector protocol of [12] (Section 3.1) and disseminates membership updates via network multicast (Section 3.2). The SWIM protocol is developed in the succeeding section (Section 4) by refining this initial design.

이제 기본 SWIM 프로토콜을 설명하여 기반을 마련합니다. 기본 프로토콜은 [12]의 랜덤 프로빙 기반 장애 감지기 프로토콜 (3.1 절)을 사용하며 네트워크 멀티 캐스트 (3.2 절)를 통해 멤버십 업데이트를 유포합니다. 다음 장(4 장)에서 소개하는 SWIM 프로토콜은 이 초기 설계를 개선하여 개발하였습니다.

### 3.1. SWIM Failure Detector

### 3.1. SWIM 장애 감지기

The SWIM failure detector algorithm [12] uses two parameters: protocol period T' (in time units) and integer k, the size of failure detection subgroups. The protocol does not require clocks to be synchronized across members, and properties of the protocol hold if T' is the average protocol period at group members.

SWIM 장애 감지기 알고리즘 [12]은 프로토콜 기간 T'(시간 단위)와 장애 탐지 하위 그룹의 크기 정수 k 라는 두 가지 파라미터를 사용합니다. 이 프로토콜은 멤버간에 클럭을 동기화 할 필요가 없으며 T'가 그룹 멤버의 평균 프로토콜 기간인 경우 프로토콜 홀드(역주: 홀드타임 - 다음 사건이 일어날 때까지 최소로 유지해야만 하는 시간)의 특성을 가질 필요가 없습니다.

Figure 1 illustrates the working of the protocol at an arbitrary member Mi. During each protocol period of length T' time units (on Mi’s local clock), a random member is selected from Mi’s membership list (say Mj), and a ping message sent to it. Mi then waits for a replying ack from Mj. If this is not received within a prespecified time-out (determined by the message round-trip time, which is chosen smaller than the protocol period), Mi indirectly probes Mj. Mi selects k members at random and sends each a ping-req(Mj) message. Each of these members in turn (those that are non-faulty), on receiving this message, pings Mj and forwards the ack from Mj (if received) back to Mi. At the end of this protocol period, Mi checks if it has received any acks, directly from Mj or indirectly through one of the k members; if not, it declares Mj as failed in its local membership list, and hands this update off to the Dissemination Component.

그림 1은 임의의 멤버 Mi에서 프로토콜의 작동을 보여줍니다. T'시간단위(Mi의 로컬 클럭)의 각 프로토콜 기간 동안 임의의 멤버(Mj)를 Mi의 멤버십 목록에서 선택하고 Ping 메시지를 전송합니다. 그런 다음 Mi는 Mj의 응답 응답을 기다립니다. 미리 지정된 타임아웃(프로토콜 기간보다 작은 메시지 왕복 시간으로 결정됨) 내에 수신되지 않으면 Mi는 간접적으로 Mj를 검사합니다. Mi는 k 개의 멤버를 무작위로 선택하고 각 ping-req(Mj) 메시지를 보냅니다. 이 메시지를 수신 한 각 구성원 (결함이없는 구성원)은 이 메시지를 수신하면 Mj에 ping을 전송하고 Mj는 ack를 Mi로 다시 전달합니다(수신 된 경우). 이 프로토콜 기간이 끝나면 Mi는 Mj로부터 직접 또는 k 개의 멤버 중 하나를 통해 간접적으로 ack를 받았는지 확인합니다. 그렇지 않은 경우 로컬 멤버십 목록에서 Mj가 실패한 것으로 선언하고 이 업데이트를 전파기에 전달합니다.

![그림1](http://3.bp.blogspot.com/-eHQF7eQZrSo/VgrOfyiYS8I/AAAAAAAAIEA/vZOUlRATxgQ/s1600/Screen%2BShot%2B2015-09-29%2Bat%2B13.35.36.png)


Figure 1. SWIM failure detection: Example protocol period at Mi. This shows all the possible messages that a protocol period may initiate. Some message contents excluded for simplicity

그림 1. SWIM 오류 감지: Mi의 프로토콜 기간 예. 프로토콜 기간에 시작될 수있는 모든 가능한 메시지가 표시됩니다. 단순화를 위해 일부 메시지 내용을 제외하였습니다.

In the example of Figure 1, one of the k members manages to complete this cycle of events as Mj is up, and Mi does not suspect Mj as faulty at the end of this protocol period.

그림 1의 예에서 k 개의 멤버 중 하나가 Mj가 가동되어 이 이벤트주기를 완료하게되면 Mi는 이 프로토콜 기간이 끝날 때 Mj에 결함이 있다고 의심하지 않습니다.

The prespecified time-out used to initiate indirect probing is based on an estimate of the distribution of round-trip time within the network, e.g. an average or 99th percentile could be used. Note that the protocol period T' has to be at least three times the round-trip estimate. In our experiments, we use the average measured round-trip time to set the time-out, and our protocol period is significantly larger than this value.

간접 진단을 시작하는 데 사용되는 사전 지정된 타임아웃은 네트워크 내에서 왕복 시간 분포의 추정치를 기반으로합니다(예: 평균 또는 99 번째 백분위 수가 사용될 수 있습니다). 프로토콜 기간 T'은 왕복 추정치의 3 배 이상이어야합니다. 이 실험에서는 측정된 평균 왕복 시간을 사용하여 타임아웃을 설정하였고, 프로토콜 기간은 이 값보다 훨씬 큽니다.

The data contained in each message of this protocol is tagged with the unique sequence number of the protocol period at the initiator(Mi). Notice that the size of ping,pingreq,ack messages is bounded by a constant, and is independent of group size.

이 프로토콜의 각 메시지에 포함 된 데이터는 이니시에이터 (Mi)에서 프로토콜 기간의 고유한 시퀀스 번호로 태그됩니다. ping, pingreq, ack 메시지의 크기는 상수로 제한되며 그룹 크기와 무관합니다.

The second part of the above protocol uses an indirect probing subgroup of members to relay both pings and acks. The rationale for using this approach, rather than sending k ping messages directly to Mj, or relaying back acks in reply to ping-reqs directly back to Mi, is to avoid the effect of any congestion on the network path between Mi and Mj; this might have led to the dropping of the original ping message or its ack.

위 프로토콜의 두 번째 부분은 간접 진단 하위 그룹 멤버를 사용하여 ping과 ack를 모두 릴레이합니다. k 번의 ping 메시지를 Mj로 직접 보내거나 ping 요청에 대한 응답으로 ack를 Mi로 직접 보내는 대신 이 방법을 사용하는 이유는 Mi와 Mj 사이의 네트워크 경로에 대한 혼잡의 영향을 피하는 것입니다. 이로 인해 원래 구간의 ping 메시지 또는 이에 대한 ack가 줄어들 수 있습니다.

This failure detector protocol is analyzed in [12]. Here, we summarize the results of the analysis (Ibid.5, The reader is encouraged to work out these results, or refer to [12].) :

이 장애 감지기 프로토콜은 [12]에서 분석하였습니다. 여기에 분석 결과를 요약합니다(주5, 이러한 결과를 살펴보거나 [12]를 참조하십시오):

• If each member has a membership list of size n, and a fraction qf of these are non-faulty, the likelihood of an arbitrary member being chosen as a ping target in a protocol period is 1-(1-(1/n)·qf)^(n-1), which decreases quickly (and asymptotically as n->∞) to 1-℮^(-qf).

• 각 구성원의 구성원 목록 크기가 n 인 경우 이들 중 일부 qf가 결함이없는 경우 프로토콜 기간에 ping 대상으로 임의 구성원이 선택 될 가능성은 1-(1-(1/n)·qf)^(n-1) 입니다. 이것은 빠르게 (그리고 무조건 n-> ∞로) 1-℮^(-qf)로 감소합니다.

• As a result, the expected time between failure of an arbitrary member and its detection by some process in the group is at most T'·(1/(1-℮^(qf))). This gives an estimate of the protocol period length in terms of the application-specified expected detection time.

• 결과적으로 임의 구성원의 실패와 그룹의 일부 프로세스에 의한 감지 사이의 예상 시간은 최대 T'· (1/(1-℮^(qf))) 입니다. 이를 통해 응용 프로그램에서 지정한 예상 탐지 시간으로 프로토콜 기간 길이를 추정 할 수 있습니다.

• If qml is the probability of timely delivery of a packet by the network, independent across all packets, an arbitrary non-faulty member will be falsely detected as failed within a protocol period with probability qf·(1-q²ml)·((1-qf·q⁴ml)^k)·(℮^(qf)/℮^(qf)-1).

• qml이 모든 패킷에서 독립적으로 네트워크에 의해 패킷이 적시에 전달 될 확률인 경우, 임의의 결함이 없는 구성원은 프로토콜 기간 내에 실패한 것으로 잘못 감지될 확율은 qf·(1-q²ml)·((1-qf·q⁴ml)^k)·(℮^(qf)/℮^(qf)-1) 입니다.

• This gives a configurable value for k in terms of the false positive probability required by the application.

• 이것은 응용 프로그램에 필요한 오탐지 확률 관점에서 설정 가능한 k 값을 제시합니다.

• This failure detector satisfies Strong Completeness: a faulty member will eventually be chosen a ping target at each non-faulty member, and deleted from its membership list.

• 이 장애 탐지기는 강력한 완전성을 충족시킵니다: 결함이있는 멤버는 결함이 없는 각 멤버에서 ping 대상으로 선택되어 멤버십 목록에서 삭제됩니다.

• The expected message load per member imposed by the protocol is a constant that does not vary with group size, and is symmetrical across all members. This load can be calculated from the estimate of k.

• 프로토콜에 의해 부과된 구성원당 예상되는 메시지 부하는 그룹 크기에 따라 달라지지 않고 모든 구성원에 걸쳐 대칭인 상수입니다. 이 부하는 k의 추정치에서 계산할 수 있습니다.

• None of these properties depend (except asymptotically) on the group size n.

• 점근적(특정 지점에 점점 가까워지는)인 경우 제외하면 이러한 속성 중 어느 것도 그룹 크기 n에 의존되지 않습니다.


### 3.2. Dissemination Component and Dynamic Membership

### 3.2. 전파기 및 동적 멤버십

Upon detecting the failure of another group member, the process simply multicasts this information to the rest of the group as failed(Mj) message. A member receiving this message deletes Mj from its local membership list.

다른 그룹 구성원의 장애를 감지하면 프로세스는 이 정보를 그룹의 나머지 멤버들에게 failed(Mj) 메시지로 멀티 캐스트합니다. 이 메시지를 받은 회원은 로컬 회원 목록에서 Mj를 삭제합니다.

Information about newly joined members or voluntarily leaving members are multicast in a similar manner. However, for a process to join the group, it would need to know at least one contact member in the group. This can be realized through one of several means: if the group is associated with a well known server or IP multicast address, all joins could be directed to the associated address. In the absence of such infrastructure(Ibid.6, Absence of centralization is a common design philosophy in peer-to-peer systems today), join messages could be broadcast, and group members hearing it can probabilistically decide (by tossing a coin) whether to reply to it. Alternatively, to avoid multiple member replies, a static coordinator could be maintained within the group for the purpose of handling group join requests. In fact, existence of multiple coordinators does not affect the correctness of the protocol, and only leads to multiple replies to the join request. Discovery and resolution of multiple coordinators can be done over time through the Dissemination Component. In the current version of SWIM, we have chosen to maintain a coordinator, although there is no reason to preclude any of the other strategies.

새로 가입 한 회원 또는 자발적으로 떠나는 회원에 대한 정보는 유사한 방식으로 멀티 캐스트됩니다. 그러나 그룹에 참여하는 프로세스의 경우 그룹에서 하나 이상의 컨택 구성원을 알아야합니다. 이는 여러 방법 중 하나를 통해 실현 될 수 있습니다: 그룹이 잘 알려진 서버 또는 IP 멀티 캐스트 주소와 연관된 경우 모든 참여가 연관된 주소로 전달 될 수 있습니다. 그러한 인프라가 없다면 (주6, 오늘 날 중앙 집중식의 부재는 피어 투 피어 시스템에서 공통적인 디자인 철학이다), 참여 메시지가 방송 될 수 있으며 그룹 구성원은 이를 듣고 답장할지의 여부를 확률 적으로 결정(동전 던지기로)할 수 있습니다. 또는 여러 구성원 응답을 피하기 위해 그룹 참여 요청을 처리하기 위해 그룹 내에서 정적 코디네이터를 유지할 수 있습니다. 실제로 여러 코디네이터가 존재해도 프로토콜의 정확성에 영향을 미치지 않으며 단지 참여 요청에 여러 번 응답할 뿐입니다. 여러 코디네이터에 의한 검색 및 해결은 전파기를 통해 시간이 지남에 따라 수행 될 수 있습니다. 현재 SWIM 버전에서는 다른 전략을 배제 할 이유가 없지만, 코디네이터 유지를 선택했습니다.
