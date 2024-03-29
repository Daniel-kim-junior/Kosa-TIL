### Message Broker

Data in motion Platform (Event Streaming platform)

## 이벤트란?
    클라이언트가 요구하는 모든 현상

사용해서 얻을 수 있는 이점
- 이벤트 스트림 I/O로 굉장히 낮은 latency와 높은 Throughput을 얻을 수있음
- 이벤트 스트림을 디스크에 저장(Write to Disk like kafka소설가 처럼)
- 이벤트 스트림을 분석 및 처리

## 아파치 카프카 주요 요소

1. Cluster : 중앙에서 Topic을 발행하는 곳
    **Topic : 카프카 내에서 메시지가 저장되는 장소(논리적인 표현)
2. Producer : 메시지를 만들어서 Topic으로 보내는 어플리케이션
3. Consumer : Topic을 pulling 해서 데이터를 처리헤서 활용하는  어플리케이션
    - Consumer Group : 하나의 Consumer는하나의 CG에 포함된다.
                       Group안의 Consumer는 서로 협력하여 분산 병렬 처리한다.

- Consumer와 Producer는 서로 알지 못한다. 그들은 서로 각각 고유의 속도로 Commit
Log에 Write 및 Read를 수행
- 다른 CG에 속한 Consumer들은 서로 관련 X
- Commit Log에 있는 Event(Message)를 동시에 다른 위치에서 Read할 수 있음

## Commit Log란?
    추가만 가능하고 변경 불가능한 데이터 스트럭처
    ```
    데이터는(Event)는 항상 로그 끝에 추가되고 변경되지 않음
    Offset : Commit Log에서 Event의 위치
    ```

    - Offset : Producer가 Write하는 LOG-END-OFFSET과 Consumer Group의
    Consumer가 Read하고 처리한 후에 Commit한 CURRENT-OFFSET과의 차이
    Consumer Lag가 발생할 수 있음

## Topic
    - Partition : Commit Log, 눈에 보이는 단위, 하나의 토픽은 1개 이상의 Partition으로
    이루어져 있음 병렬처리(Throughput 향상)를 위해서 다수의 Partition을 사용
    Topic 생성시에 Partition 개수를 지정
    Topic 내의 Partition들은 서로 독립적이다. 번호는 0번부터 오름차순
    따라서 Partition 내부의 Offset 또한 독립적으로 서로 다른 의미이다.
    offset 값은 계속 증가하고 0으로 돌아가지 않는다.
    Event(Message)의 순서는 하나의 Partition 내부에서만 보장
    Partition에 저장된 Message는 Immutable
    Log-End위치에 항상 추가된다.


    - Segment : 메시지(데이터)가 저장되는 실제 물리 파일
        Segment File이 지정된 크기보다 크거나 지정된 기간보다 오래되면
        새 파일이 열리고 메시지는 새 파일에 추가 파일의 용량을 고려해
        여러개의 파일로 Rolling 정책을 가짐 (bytes , hours)

## Kafka Cluster
    여러개의 Broker들로 이루어져 있음
    브로커에는 파티션별로 클러스터링 되어 위치함
    ex) Brocker 101 - TopicA Partition 0
                      TopicB Partition 0

        Brocker 102 - TopicA Partition 1
                      TopicB Partition 1
    이런식으로 위치하는데 최적의 구조로 카프카 내부의 알고리즘으로 지정된다.

## Kafka Producer
    - 메시지를 생산(Produce)해서 Kafka의 Topic으로 메시지를 보내는 어플리케이션
    - Producer와 Consumer는 서로 알지 못함!!
    - 각각 고유의 속도로 Commit Log에 Write와 Read를 수행
    
    ### Producer가 만들어내는 것
        - Message == Record == Event == Data
        Headers : Topic, Partition, TimeStamp, etc - MetaData (어떤 Topic and Partition...)
        Key-Value: Body Business Relevant Data (내용 : Avro, String JSON, ProtoBuffer 등 다양한 형태)
        - 카프카는 수동적인 소프트웨어이기 때문에 개발자가 요청을 해야함
        카프카의 데이터 저장 방식은 Byte Array(Record -> Serializers -> Byte Array)
        Key와 Value 각각 다른 Serializer를 사용 할 수 있다.
        Compress 옵션 사용 가능
        Partitioner는 레코드를 어떤 파티션으로 보낼지 결정하는 라이브러리(key-해시 알고리즘으로 파티션에 배치 전제조건 : key가 null이 아닐때)
        
        Default Partitioner : 전제조건 key가 null일 때
        - kafka 2.4 이전에는 Round-Robin 정책
        - 이후에는 Sticky 정책 (배치가 닫힐때까지 하나의 파티션에 우겨넣음 후엔 랜덤하게 파티션 지정 후 반복)
        - Partitioner는 개발해서 교체 가능
        
        

## Kafka Consumer
    - Topic의 메시지를 가져와서 소비(Consume)하는 어플리케이션

## Consumer Group
    - Topic의 메시지를 사용하기 위해 협력하는 Consumer의 집합(협력 병렬처리)
    - 하나의 Consumer는 하나의 Consumer Group에 포함된다.



