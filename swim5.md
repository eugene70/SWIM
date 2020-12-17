## 5. Performance Evaluation of a Prototype 

## 5. 프로토타입의 성능 평가

A prototype of the SWIM protocol was implemented over the Winsock 2 API, and was tested in a large cluster of 
commodity PCs running Windows 2000. The PC cluster was comprised of 16 450-MHz Dell PII’s, 16 1-GHz IBM x220’s, and a collection of dual and quad nodes (200-MHz to 500-MHz PII and PIII processors), communicating over a 100 Mbps Ethernet with no external load. Each node contained at most one process group member.

SWIM 프로토콜의 프로토타입은 Winsock 2 API를 통해 구현되었으며 Windows 2000을 실행하는 대규모 상용 PC 클러스터에서 테스트되었습니다. PC 클러스터는 16 x 450MHz Dell PII, 16 x 1GHz IBM x220, 듀얼 및 쿼드 노드 (200MHz ~ 500MHz PII 및 PIII 프로세서)의 집합, 외부로드 없는 100Mbps 이더넷 통신망으로 구성되었습니다. 각 노드는 최대 하나의 프로세스 그룹 구성원을 포함했습니다.

The experimental parameters were set as follows. The number of members chosen for ping-reqs was K = 1, and the protocol period used was 2 sec. Each infection (membership update) was piggybacked on at most (3 [log (N + 1)]) messages sent by each member. With this value setting, our experimental runs observed no evidence of perpetual partial membership lists at members or involuntary partitions of the group. The suspicion time-out for a suspected member to be declared as failed was also set to this value.

실험 파라미터는 다음과 같이 설정 하였다. ping-reqs에 선택된 구성원 수는 K = 1 이고 사용 된 프로토콜 기간은 2 초입니다. 각 감염(멤버십 업데이트)에는 각 멤버가 보낸 최대 (3 [log (N + 1)]) 메시지가 피기백되었습니다. 이 값 설정을 사용하여 실험을 실행하면 지속적인 부분 멤버십 목록 또는 의도하지 않은 그룹의 조각화의 증거가 관찰되지 않았습니다. 의심되는 구성원이 실패한 것으로 선언되는 의심 시간 종료도 이 값으로 설정되었습니다.

We compared three different versions of the protocol: (1) (SWIM:Basic) the basic SWIM protocol of Section 3, modified with the Round-Robin scheme described in Section 4.3, (2) (SWIM+Inf.) SWIM with an infection-style Dissemination Component (as described in Section 4.1), and (3) (SWIM+Inf.+Susp.) SWIM+Inf. with the Suspicion subprotocol extension described in Section 4.2.

우리는 3 가지 버전의 프로토콜을 비교했습니다: (1) (SWIM : Basic) 3장의 기본 SWIM 프로토콜에 4.3 절에 설명 된 round-robin 방식으로 수정, (2) (SWIM+Inf.) SWIM with 감염스타일 전파기(4.1 절에서 설명), 그리고 (3) (SWIM+Inf.+Susp.) SWIM+Inf. with 4.2 절에서 설명된 의심 하위 프로토콜 확장.

All point-to-point messages sent by the SWIM protocol were UDP packets. The maximum message payload sizes were 15 B in SWIM:Basic, and 135 B in the SWIM+Inf. and SWIM+Inf.+Susp. protocols (since at most 6 membership updates were piggybacked per message(Ibid.7, This parameter could be adapted to high membership update rates, thus trading off overhead for dissemination time.)).

SWIM 프로토콜에 의해 전송 된 모든 지점 간 메시지는 UDP 패킷이었습니다. 최대 메시지 페이로드 크기는 SWIM:Basic에서 15B, SWIM+Inf. 및 SWIM+Inf.+Susp. 프로토콜에서 135B입니다(메시지 당 최대 6 개의 멤버쉽 업데이트가 피기백되므로 (주7, 이 매개 변수는 빠른 멤버쉽 업데이트 속도와 전파 시간에 대한 오버 헤드 사이에서 적절히 조절할 수 있습니다)).

### 5.1. Message Loads

### 5.1. 메시지로드

Figure 2 shows the measured message send and receive loads imposed by the SWIM failure detector at an arbitrary group member. The readings are taken over a time period spanning over 40 protocol periods. Up to a group size of 55 members, the average overhead stays around 2.0. This matches the analytic estimate as, during each protocol period, a group member sends a single ping and receives the corresponding ack, as well as receives one ping message (on expectation) and sends the corresponding ack. The standard deviation bars indicate that typical message overheads stay low, e.g. at N=28 members, the overhead of sent messages is smaller than 5 messages per protocol period with a probability of 0.99.

그림 2는 임의의 그룹 구성원에서 SWIM 장애 감지기에 의해 부과된 측정된 메시지 송수신 부하를 보여줍니다. 판독 값은 40 개의 프로토콜 기간에 걸친 기간에 걸쳐 취해집니다. 최대 55 멤버로 구성된 그룹의 평균 오버 헤드는 약 2.0입니다. 이는 각 프로토콜 기간 동안 그룹 구성원이 단일 핑을 전송하고 해당 ack를 수신 할뿐만 아니라 하나의 핑 메시지를 수신(기대 대로)하고 ack를 전송하는 분석 추정값과 일치합니다. 표준 편차 막대는 일반적인 메시지 오버 헤드가 낮게 유지됨을 나타냅니다. 예: N=28 구성원에서, 전송 된 메시지의 오버 헤드는 프로토콜 기간 당 5 개 메시지보다 작고 확률은 0.99입니다.

Figure 2. Send and Receive message overheads at a group member. Points perturbed horizontally for clarity.

[그림2 삽입]

그림 2. 한 그룹 멤버에서의 전송 및 수신 메시지 오버헤드. 포인트가 명확성을 위해 수평으로 조정되었다.

Recollect from our description in Section 4 that this plot shows the total message overhead per member in SWIM+Inf. and SWIM+Inf.+Susp, while only the base load in SWIM:Basic (where additional multicasts disseminate membership updates).

4 장의 설명을 상기하자면, SWIM:Basic에서 기본 로드만 있는 반면  (추가 멀티 캐스트가 멤버십 업데이트를 유포하는 경우), 이 플롯은 SWIM+Inf.와 SWIM+Inf.+Susp.의 멤버 당 총 메시지 오버 헤드를 보여줍니다. 

### 5.2. Detection and Dissemination Latency of Membership Updates

### 5.2. 멤버십 업데이트의 감지 및 전파 지연

Figure 3 shows the break up of failure detection and dissemination times for the three different protocols. Figure 3(a) shows the mean time between a process failure and its first detection at some non-faulty group member. The average detection time does not appear to be correlated to group size and thus matches the analytic estimate. In the SWIM:Basic protocol, the vertical axis values correspond to the interval between a process failure and the instant of multicast of its failure notification. In the SWIM+Inf. and SWIM+Inf.+Susp. protocols, these values correspond to the interval between a failure and the start of spread of the failure notification and suspicion notification infections, respectively.

그림 3은 서로 다른 세 가지 프로토콜에 대한 장애 감지 및 배포 시간을 보여줍니다. 그림 3(a)는 결함이없는 그룹 구성원에서 프로세스 실패와 첫 번째 감지 사이의 평균 시간을 보여줍니다. 평균 검출 시간은 그룹 크기와 상관 관계가 없는 것으로 보이므로 분석 추정치와 일치합니다. SWIM:Basic 프로토콜에서 세로 축 값은 프로세스 실패와 실패 알림의 멀티 캐스트 순간 사이의 간격에 해당합니다. SWIM+Inf. 및 SWIM+Inf.+Susp. 프로토콜에서 이러한 값은 각각 실패 알림과 실패 알림의 확산 시작 및 의심 알림 감염 사이의 간격에 해당합니다.

Figure 3(b) plots the variation with group size of the delay in infection-style dissemination of membership updates. The median latency of dissemination is always only a few protocol periods, and appears to be rise slowly with group size (recall the analysis of Section 4.1 predicted a logarithmic variation of the average infection time with group size).

그림 3(b)는 멤버십 업데이트의 감염 스타일 전파 지연의 그룹 크기에 따른 변동을 나타냅니다. 전파 지연 시간 중간값은 항상 몇 개의 프로토콜 기간 일 뿐이며 그룹 크기에 따라 천천히 증가하는 것으로 보입니다 (4.1 절의 분석은 그룹 크기에 따른 평균 감염 시간의 로그 변동을 예측 함).

Figure 3(c) shows the suspicion time-out (3 [log(N + 1)]), and is applicable only to the SWIM+Inf.+Susp. protocol. 

그림 3 (c)는 의심 타임 아웃 (3 [log (N + 1)])을 보여 주며 SWIM+Inf.+Susp. 프로토콜에만 적용됩니다.

The average time between a process failure and its removal from the membership lists of other members is obtained from the three plots in Figure 3. For the SWIM:Basic protocol, this is the value plotted in Figure 3(a) plus the time taken by a network multicast. For the SWIM+Inf. protocol, this is the sum of the distributions on the vertical axes of Figure 3(a) and Figure 3(b), while for the SWIM+Inf.+Susp. protocol, this delay is the sum of the distributions on the three plots in Figure 3.

프로세스 실패와 다른 멤버의 멤버십 목록에서 제거 간의 평균 시간은 그림 3의 3 가지 플롯에서 얻습니다. SWIM:Basic 프로토콜의 경우 이 값은 그림 3(a)에 표시된 값에 네트워크 멀티 캐스트 시간을 더한 값입니다. SWIM + Inf. 프로토콜, 이것은 그림 3(a)와 그림 3(b)의 세로축에 대한 분포의 합이고, SWIM+Inf.+Susp. 프로토콜의 경우 이 지연은  그림 3에서 세 도표의 분포의 합입니다.

[그림3 삽입]

Figure 3. Detection and Dissemination Time of membership updates.
그림 3. 멤버십 업데이트 감지 및 전파 시간.

(a) Variation with group size of time to first detection of a process failure (points), and means (dark horizontal lines) obtained in the shown intervals (light vertical lines), for the three protocols. Also shown is the analytic estimate from Section 3.1.

(a) 3 개의 프로토콜에 대해 실패 프로세스를 처음 감지하기까지의 시간(포인트), 표시된 간격(밝은 수직선)으로 얻어진 평균(어두운 수평선)의 그룹 크기에 따른 변화. 3.1 절의 분석 추정치도 표시됩니다.

(b) Latency of spread of Infection through the group in SWIM+Inf. and SWIM+Inf.+Susp. Points shown correspond to first receipt times of infection at different group members.

(b) SWIM+Inf. 및 SWIM+Inf.+Susp. 포인트에서 그룹을 통한 감염 확산의 지연 시간은 상이한 그룹 구성원에서의 감염의 최초 수신 시간에 상응한다.

(c) Suspicion time-out used in the SWIM+Inf.+Susp. protocol.

(c) SWIM+Inf.+Susp. 프로토콜에서 사용되는 의심 타임 아웃.


### 5.3. Failure Detection False Positives 

### 5.3. 장애 감지 오 탐지

The runs of the SWIM+Inf.+Susp. protocol in Figure 3 generated an insignificant number of false failure detections. In order to evaluate the relative benefit of the infection and suspicion mechanisms, we synthesized a high artificial packet drop of 10%. Each packet (including each network multicast in SWIM:Basic) was dropped at the receiver with this loss probability. In our experiment, 17 processes attempted to join a SWIM group sequentially, as shown in Figure 4. The plot shows the variation of group size during and after this joining phase, in spite of the sustained packet loss rate. The plot is cut off at 175 s, after the last observed membership change. This plot shows the benefit of the Suspicion mechanism: while SWIM+Inf.+Susp. attains a stable group size of 12 members, SWIM:Basic and SWIM+Inf. stabilize at 2 and 4 members respectively.

그림 3의 SWIM+Inf.+Susp. 프로토콜 실행으로 인해 많은 수의 잘못된 오류 감지가 발생했습니다. 감염 및 의심 메커니즘의 상대적인 이점을 평가하기 위해 10%의 높은 인공적인 패킷 드롭을 발생시켰습니다. 각 패킷 (SWIM:Basic의 각 네트워크 멀티 캐스트 포함)은 이 손실 확률로 수신기에서 삭제되었습니다. 우리의 실험에서 17 개의 프로세스는 그림 4에 표시된 것처럼 순차적으로 SWIM 그룹에 참여하려고 시도했습니다. 플롯은 지속적인 패킷 손실률에도 불구하고 이 결합 단계 동안 및 이후에 그룹 크기의 변화를 보여줍니다. 도표는 마지막으로 멤버십이 변경된 후 175 초에 잘립니다. 이 도표는 의심 메커니즘의 이점을 보여줍니다. SWIM+Inf.+Susp.은 12 멤버의 안정적인 그룹 크기를 달성하지만 SWIM:Basic 및 SWIM+Inf.는 각각 2 개와 4 개의 멤버에서 안정화됩니다.


[그림4 삽입]

Figure 4. Effect of 10% packet loss rate on a member join sequence (blips at bottom), with conservative protocol parameters.

그림 4. 보수적인 프로토콜 파라미터를 사용하여 멤버 조인 시퀀스(하단에서 튕김)에 10% 패킷 손실률이 미치는 영향.

The data in Figure 4 holds for the described experimental settings only. A more aggressive setting of parameters K and the suspicion time-out would increase the robustness of the three protocols. Varying the suspicion time-out also provides a knob to trade off failure detection time with the frequency of false positives. We leave the investigation of such issues for a later article.

그림 4의 데이터는 명시된 실험 설정에만 적용됩니다. 보다 적극적인 매개 변수 K 설정과 의심 시간 초과는 세 가지 프로토콜의 견고성을 향상시킵니다. 의심 시간 제한을 변경하면 오 탐지 빈도와 실패 감지 시간을 절충 할 수있는 수단도 제공됩니다. 이후 지면에서는 이러한 문제에 대한 조사를 진행합니다.
