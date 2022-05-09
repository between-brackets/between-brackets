# Designing Data-Intensive Applications Martin Kleppmann

# Table of Contents
1. [CHAPTER 1-4](#chapter-1-4)
2. [CHAPTER 5. Replication and CHAPTER 6. Partitioning](#chapter-5-replication-and-chapter-6-partitioning)
3. [CHAPTER 7. Transactions](#chapter-7-transactions)
4. [CHAPTER 8. The trouble with distributed systems](#chapter-8-the-trouble-with-distributed-systems)

## CHAPTER 1-4
### Moderator
Григорий Скобелев. Java backend разработчик в ЮMoney. Директор программного комитета Podlodka Backend Crew, активный спикер и лектор в нетологии.
### Experts
Максим Шатунов 🔥🔥🔥
Java Tech Lead в команде Альфа-Мобайл. Максим знает на практике, как создать масштабируемый и отказоустойчивый backend для мобильного приложения.
### Useful links
* Про мозг и его latency https://youtu.be/8423tw7mLc8
* Ссылки на билиографию https://github.com/ept/ddia-references
* CAP https://ru.wikipedia.org/wiki/Теорема_CAP https://arxiv.org/abs/1509.05393
* SnowflakeServer https://martinfowler.com/bliki/SnowflakeServer.html
* CanaryRelease https://martinfowler.com/bliki/CanaryRelease.html
* Building Evolutionary Architectures: Support Constant Change 1st Edition https://www.amazon.com/Building-Evolutionary-Architectures-Support-Constant/dp/1491986360
* Database Internals: A Deep Dive into How Distributed Data Systems Work 1st Edition https://www.amazon.com/Database-Internals-Deep-Distributed-Systems/dp/1492040347

## CHAPTER 5. Replication and CHAPTER 6. Partitioning
### Moderator
Григорий Скобелев. Java backend разработчик в ЮMoney. Директор программного комитета Podlodka Backend Crew, активный спикер и лектор в нетологии.
### Plan
📍Горизонтальное и вертикальное масштабирование
  + shared-disk architecture
  + shared-memory architecture
  + shared-nothing architectures
  
📍 Репликация
  + reduce latency
  + increase availability
  + increase read throughput
  + Sync, async, semi-sync replication
  + Когда считать что запись точно попала в бд
  + Синхронизация оффлайн клиентов
  + Пример chain replication
  + Выход из строя follower 
  + Выход из строя leader 
  + Split brean (shoot the other node in the head)
  + Ручное или автоматическое восстановление данных
  + eventual consistency
  + read- after-write consistency
  + consistent prefix reads
  + Leaders and followers
  + Multi-leaders
  + Leaderless 
  
📍 Шардирование 
  + по ключу 
  + по хэшу 
  + перебалансировка шардов
  
### Useful links 
+ https://github.com/kelseyhightower/nocode
+ https://instagram-engineering.com/instagration-pt-2-scaling-our-infrastructure-to-multiple-data-centers-5745cbad7834
+ https://www.youtube.com/watch?v=85GIFpG3zx4&t=2714s 
+ https://www.percona.com/blog/2020/08/21/postgresql-synchronous_commit-options-and-synchronous-standby-replication/
+ https://youtu.be/x9tvJjWCrr4
+ https://jepsen.io/consistency/models/read-your-writes
+ http://nms.csail.mit.edu/~stavros/pubs/hstore.pdf 
+ https://www.youtube.com/c/Hydraconf/featured
+ https://www.xenonstack.com/blog/sql-vs-nosql-vs-newsql
+ https://podlodka.io/101
+ https://habr.com/ru/company/oleg-bunin/blog/413557/
+ http://www.cidrdb.org/cidr2022/papers/p5-helland.pdf
+ https://cwiki.apache.org/confluence/download/attachments/188744725/Accord.pdf
+ https://www.confluent.io/blog/distributed-consensus-reloaded-apache-zookeeper-and-replication-in-kafka/
+ https://www.youtube.com/watch?v=Sr0sX-TId-g&ab_channel=DataStax
+ https://youtube.com/playlist?list=PL-_cKNuVAYAW8g2tpyNqCXvoPsUgh_l6i

## CHAPTER 7. Transactions
### Moderator
Григорий Скобелев. Java backend разработчик в ЮMoney. Директор программного комитета Podlodka Backend Crew, активный спикер и лектор в нетологии.
### Experts
📍Илья Казначеев - техлид в МТС Cloud, как архитектор понимает как построить отказоустойчивую систему, GDE on Cloud 🤩 Также Илья ведет собственный блог про разработку https://t.me/kaznacheev_feed 

📍Николай Голов - Chief Data Architect at ManyChat, знает все о том как построить OLAP и OLTP систему, в деталях разбирается в построении аналитических систем. 
### Puzzel 
#### Task A (Atomic operation)

У вас имеется текстовый файл с списком продуктов 
* яблоки 100
* тыква 200
* брокколи 400
сумма 700 

Так получилось, что вам надо поменять позицию - тыква 200, на кабачок 150. Важно, при изменении позиции должна меняться и сумма. Как сделать это атомарно? Сбой может произойти в любой момент.

#### Task B* (Isolation)
У вас имеется простой key-value store, в котором put/get операции являются atomic. Два счета, есть возможность делать атомарные операции в рамках одного счета, но нет глобальной транзакции. Как сделать чтобы деньги атомарно перенеслись с одного счета на другой? Вам надо сделать чтобы появилась изоляция. Не забудьте учесть сценарии, когда может произойти чтение данных еще незавершенной транзакции.

#### Task C* (Atomic operation)
Вы занимаетесь разработкой сервиса для орнитологов, который считает количество птиц на фотографии. Две папки input, output. На вход подается оригинальная фотография, на выход ожидается фотография/файл с отметкой позиций птиц на фотографии для дальнейшего их изучения. Обработанные фотографии удаляем из input. Так получилось, что ваш личный попугай - Иннокентий, заскучал и в качестве развлечения решил мешать вам обрабатывать файлы. Инокентий может в любой момент отключить электричество. Как удалять старые файлы при сбоях питания? И как избежать появления дубликатов.

## Useful links 
* https://sqlite.org/wal.html
* https://datacadamia.com/data/property/rollback_journal
* https://ru.wikipedia.org/wiki/OLAP
* https://www.cockroachlabs.com/
* https://cloud.yandex.ru/services/ydb
* https://habr.com/ru/company/yandex/blog/660271
* https://microservices.io/patterns/data/saga.html
* https://youtu.be/6HvSpqBc8fA
* https://youtu.be/bAhxpqHfP8I
* https://www.ozon.ru/product/mikroservisy-patterny-razrabotki-i-refaktoringa-154840382/?sh=IMwhNa1XQQ

## CHAPTER 8. The trouble with distributed systems
### Summary 
- #### Intro
    - This chapter is a thoroughly pessimistic and depressing overview of **things that may go wrong in a distributed system**. We will look into problems with networks (Unreliable Networks); clocks and timing issues (Unreliable Clocks); and we’ll discuss to what degree they are avoidable
    - In a distributed system, there may well be some parts of the **system that are broken in some unpredictable way, even though other parts of the system are working fine**. This is known as a partial failure. **The difficulty is that partial failures are nondeterministic**: if you try to do anything involving multiple nodes and the network, it may sometimes work and sometimes unpredictably fail. As we shall see, you may not even know whether something succeeded or not, as the time it takes for a message to travel across a network is also nondeterministic!
- #### 📉 Faults and Partial Failures
    - This is a deliberate choice in the design of computers: **if an internal fault occurs, we prefer a computer to crash completely rather than returning a wrong result**, because wrong results are difficult and confusing to deal with
    - The bigger a system gets, the more likely it is that one of its components is broken. Over time, broken things get fixed and new things break, but in a system with thousands of nodes, it is reasonable to **assume that something is always broken**
    - In other words, we need to **build a reliable system from unreliable components**
        - You may wonder whether this makes any sense—intuitively it may seem like a system can only be as reliable as its least reliable component (its weakest link). This is not the case: in fact, it is an old idea in computing to **construct a more reliable system from a less reliable underlying base**
- #### 🕸️ Unreliable Networks
    - If you send a request and expect a response, **many things could go wrong**
        - Your **request may have been lost** (perhaps someone unplugged a network cable).
        - Your **request may be waiting in a queue and will be delivered later** (perhaps the network or the recipient is overloaded).
        - The **remote node may have failed** (perhaps it crashed or it was powered down).
        - The **remote node may have temporarily stopped responding** (perhaps it is experiencing a long garbage collection pause; see Process Pauses), but it will start responding again later.
        - The **remote node may have processed your request, but the response has been lost on the network** (perhaps a network switch has been misconfigured).
        - The **remote node may have processed your request, but the response has been delayed and will be delivered later** (perhaps the network or your own machine is overloaded).
        - ![image.png](/desing-data-intensive-application/assets/08/image_1651981631775_0.png)
            - If you send a request and don’t get a response, it’s not possible to distinguish whether
                - (a) the request was lost
                - (b) the remote node is down
                - (c) the response was lost
    - The usual way of handling this issue is a **timeout**: after some time you give up waiting and assume that the response is not going to arrive. However, when a timeout occurs, you still don’t know whether the remote node got your request or not (and if the request is still queued somewhere, it may stillbe delivered to the recipient, even if the sender has given up on it)
    - When one part of the network is cut off from the rest due to a network fault, that is sometimes called a **network partition or netsplit** and corrupting it.
    - However, **you do need to know how your software reacts to network problems and ensure that the system can recover from them**
    - Many **systems need to automatically detect faulty nodes**.
        - For example: A **load balancer** needs to stop sending requests to a node that is dead (i.e., take it out of rotation).
        - In a distributed database with single-leader replication, if the **leader fails**, one of the followers needs to be promoted to be the new leader (see Handling Node Outages).
    - If a timeout is the only sure way of detecting a fault, then **how long should the timeout be**? There is unfortunately no simple answer.
        - A **long timeout means a long wait until a node is declared dead** (and during this time, users may have to wait or see error messages).
        - A **short timeout detects faults faster, but carries a higher risk of incorrectly declaring a node dead when in fact it has only suffered a temporary slowdown** (e.g., due to a load spike on the node or the network).
            - Prematurely declaring a node dead is problematic: if the node is actually alive and in the middle of performing some action (for example, sending an email), and another node takes over, the **action may end up being performed twice**.
            - We will discuss this issue in more detail in Knowledge, Truth, and Lies, and in Chapters 9 and 11.
        - **When a node is declared dead, its responsibilities need to be transferred to other nodes**, which places additional load on other nodes and the network. If the system is already struggling with high load, declaring nodes dead prematurely can make the problem worse. In particular, it could happen that the happen that **the node actually wasn’t dead but only slow** to respond due to overload; transferring its load to other nodes can cause a cascading failure (in the extreme case, all nodes declare each other dead, and everything stops working).
    - **TCP considers a packet to be lost if it is not acknowledged within some timeout** (which iscalculated from observed round-trip times), and lost packets are automatically retransmitted
        - Some latency-sensitive applications, such as videoconferencing and Voice over IP (VoIP), use **UDP rather than TCP**. It’s a trade-off between reliability and variability of delays: as UDP does not perform flow control and does not retransmit lost packets, it avoids some of the reasons for variable network delays
    - Systems can continually measure response times and their variability (jitter), and **automatically adjust timeouts** according to the observed response time distribution. This can be done with a Phi Accrual failure detector, which is used for example in Akka and Cassandra. TCP retransmission timeouts also work similarly
    - Currently deployed technology does not allow us to make any guarantees about delays or reliability of the network: we have to assume that **network congestion, queueing, and unbounded delays will happen**. Consequently, there’s no “correct” value for timeouts—they need to be determined experimentally
- #### 🕐 Unreliable Clocks
    - The problem with clocks is that while **they seem simple and easy to use, they have a surprising number of pitfalls**:
        - a day may not have exactly 86,400 seconds,
        - time-of-day clocks may move backward in time, and the time on one node may be quite different from the time on another node.
    - This conflict resolution strategy is called **last write wins** (LWW), and it is widely used in both multi-leader replication and leaderless databases such as Cassandra and Riak
        - ![image.png](/desing-data-intensive-application/assets/08/image_1651982795655_0.png)
    - So-called **logical clocks**, which are based on incrementing counters rather than an oscillating quartz crystal, are a safer alternative for ordering events (see Detecting Concurrent Writes). Logical clocks do not measure the time of day or the number of seconds elapsed, only the relative ordering of events (whether one event happened before or after another). In contrast, time-of-day and monotonic clocks, which measure actual elapsed time, are also known as physical clocks
    - Thus, **it doesn’t make sense to think of a clock reading as a point in time—it is more like a range of times, within a confidence interval**: for example, a system may be 95% confident that the time now is between 10.3 and 10.5 seconds past the minute, but it doesn’t know any more precisely than that If we only know the time +/– 100 ms, the microsecond digits in the timestamp are essentially meaningless.
    - Say you have a database with a single leader per partition Only the leader is allowed to accept writes. **How does a node know that it is still leader** (that it hasn’t been declared dead by the others), and that it may safely accept writes?
        - One option is for the leader to **obtain a lease** from the other nodes, which is similar to a lock with a timeout. Only one node can hold the lease at any one time—thus, when a node obtains a lease, it knows that it is the leader for some amount of time, until the lease expires. In order to remain leader, the node must periodically renew the lease before it expires. If the node fails, it stops renewing the lease, so another node can take over when it expires.
    - When writing multi-threaded code on a single machine, we have fairly good tools for making it **thread-safe**: mutexes, semaphores, atomic counters, lock-free data structures, blocking queues, and so on. Unfortunately, these tools **don’t directly translate to distributed systems, because a distributed system has no shared memory—only messages sent over an unreliable network**.
        - A node in a distributed system must assume that its execution **can be paused for a significant length of time at any point**, even in the middle of a function. During the pause, the rest of the world keeps moving and may even declare the paused node dead because it’s not responding. Eventually, the paused node may continue running, without even noticing that it was asleep until it checks its clock sometime later.
- #### 🤔 Knowledge, Truth, and Lies
    - A **node in the network cannot know anything for sure — it can only make guesses based on the messages it receives (or doesn’t receive) via the network**.
        - A node can only find out what state another node is in (what data it has stored, whether it is correctly functioning, etc.) by exchanging messages with it. If a remote node doesn’t respond, there is no way of knowing what state it is in, because problems in the network cannot reliably be distinguished from problems at a node
        - **Discussions of these systems border on the philosophical**
            - What do we know to be true or false in our system?
            - How sure can we be of that knowledge, if the mechanisms for perception and measurement are unreliable?
            - Should software systems obey the laws that we expect of the physical world, such ascause and effect?
        - The moral of these stories is that a node cannot necessarily trust its own judgment of a situation. A distributed system cannot exclusively rely on a single node, because a node may fail at any time, potentially leaving the system stuck and unable to recover. Instead, **many distributed algorithms rely on a quorum**, that is, voting among the nodes (see Quorums for reading and writing):decisions require some minimum number of votes from several nodes in order to reduce the dependence on any one particular node
            - That includes decisions about declaring nodes dead. **If a quorum of nodes declares another node dead, then it must be considered dead, even if that node still very much feels alive**. The individualnode must abide by the quorum decision and step down.
            - Most commonly, the **quorum is an absolute majority of more than half the nodes** (although other kinds of quorums are possible). A **majority quorum allows the system to continue working if individual nodes have failed** (with three nodes, one failure can be tolerated; with five nodes, two failures can betolerated). However, it is still safe, because there can only be only one majority in the system—**there cannot be two majorities with conflicting decisions at the same time**
    - Frequently, a system requires there to be **only one of some thing**. For example:
        - Only one node is allowed to be the leader for a database partition, to avoid split brain
        - Only one transaction or client is allowed to hold the lock for a particular resource or object, to prevent concurrently writing to it
    - Implementing this in a distributed system requires care: even if a node believes that it is the chosen one (the leader of the partition, the holder of the lock, the request handler of the userwho successfully grabbed the username), **that doesn’t necessarily mean a quorum of nodes agrees**! A node may have formerly been the leader, but if the other nodes declared it dead in the meantime(e.g., due to a network interruption or GC pause), **it may have been demoted and another leader may have already been elected**
        - Making access to storage safe by allowing writes only in the order of increasing **fencing tokens**
            - ![image.png](/desing-data-intensive-application/assets/08/image_1651981713178_0.png)
            - ![image.png](/desing-data-intensive-application/assets/08/image_1651981754470_0.png)
    - **Nodes may lie** (send arbitrary faulty or corrupted responses)—for example, if a node may claim to have received a particular message when in fact it didn’t. Such behavior is known as a Byzantine fault, and the problem of reaching consensus in this untrusting environment is known as the Byzantine Generals Problem
        - The Byzantine Generals Problem is a generalization of the so-called Two Generals Problem, which imagines a situation in which two army generals need to agree on a battle plan. As they
            have set up camp on two different sites, they can only communicate by messenger, and the messengers sometimes get delayed or lost (like packets in a network)
        - In the Byzantine version of the problem, there are n generals who need to agree, and their
            endeavour is hampered by the fact that there are some traitors in their midst. Most of the generals are loyal, and thus send truthful messages, but the traitors may try to deceive and confuse the others by sending fake or untrue messages (while trying to remain undiscovered). It is not known in advance who the traitors are.
    - **A system is Byzantine fault-tolerant** if it continues to operate correctly even if some of the nodes are malfunctioning and not obeying the protocol, or if malicious attackers are interfering with the network
    - Moreover, besides timing issues, we have to consider node failures. The three most common **system models for nodes** are:
        - **Crash-stop faults**. In the crash-stop model, an algorithm may assume that a node can fail in only one way, namely by crashing. This means that the node may suddenly stop responding at any moment, and thereafter that node is gone forever—it never comes back.
        - **Crash-recovery faults** We assume that nodes may crash at any moment, and perhaps start responding again after some unknown time. In the crash-recovery model, nodes are assumed to have stable storage (i.e., nonvolatile disk storage) that is preserved across crashes, while the in-memory state is assumed to be lost.
        - **Byzantine (arbitrary) faults** Nodes may do absolutely anything, including trying to trick and deceive other nodes, as described in the last section. For modeling real systems, the partially synchronous model with crash-recovery faults is generally the most useful model. But how do distributed algorithms cope with that model?

