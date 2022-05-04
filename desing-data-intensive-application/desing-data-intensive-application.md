# Desing Data-intensive Application Martin Kleppmann

# Table of Contents
1. [CHAPTER 1-4](#chapter-1-4)
2. [CHAPTER 5. Replication and CHAPTER 6. Partitioning](#chapter-5-replication-and-chapter-6-partitioning)
3. [CHAPTER 7. Transactions](#chapter-7-transactions)

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


