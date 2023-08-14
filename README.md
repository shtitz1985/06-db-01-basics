# `Домашнее задание к занятию «Типы и структура СУБД» - Зозуля Максим`


## Задача 1

Архитектор ПО решил проконсультироваться у вас, какой тип БД 
лучше выбрать для хранения определённых данных.

Он вам предоставил следующие типы сущностей, которые нужно будет хранить в БД:

- электронные чеки в json-виде,

Рекомендуемый тип СУБД: нереляционные (например, MongoDB).
Обоснование: Документоориентированные базы данных хорошо подходят для хранения и обработки JSON-подобных документов, таких как электронные чеки. Они позволяют хранить данные в виде документов без жесткой схемы, что удобно для чеков с различными полями.

- склады и автомобильные дороги для логистической компании,

Для хранения данных о складах и автомобильных дорогах, рекомендуется использовать реляционные базы данных (SQL). Такие СУБД, как PostgreSQL или MySQL, предоставят возможность моделирования связей между различными сущностями и эффективно выполнять сложные запросы.

- генеалогические деревья,

В случае генеалогических деревьев, подходящим типом СУБД является графовая база данных. Например, Neo4j или Amazon Neptune. Графовая база данных позволит эффективно моделировать и обрабатывать сложные структуры связей между разными участниками родословной.

- кэш идентификаторов клиентов с ограниченным временем жизни для движка аутенфикации,

Для хранения кэша идентификаторов клиентов с ограниченным временем жизни лучше всего подойдет ключ-значение (key-value) хранилище. Redis или Memcached прекрасно подходят для этой цели, так как они специализируются на хранении пар ключ-значение с возможностью установки времени жизни.

- отношения клиент-покупка для интернет-магазина.

Рекомендуемый тип СУБД: Реляционные (например, PostgreSQL, MySQL).
Обоснование: Реляционные базы данных хорошо подходят для хранения данных с явными связями и отношениями между ними, как это часто бывает в интернет-магазинах. Они предоставляют сильные возможности для структурирования и выполнения сложных SQL-запросов.

Выберите подходящие типы СУБД для каждой сущности и объясните свой выбор.

## Задача 2

Вы создали распределённое высоконагруженное приложение и хотите классифицировать его согласно 
CAP-теореме. Какой классификации по CAP-теореме соответствует ваша система, если 
(каждый пункт — это отдельная реализация вашей системы и для каждого пункта нужно привести классификацию):

- данные записываются на все узлы с задержкой до часа (асинхронная запись);

Эта реализация соответствует модели AP (Availability/Partition tolerance), где система обеспечивает доступность и сохраняет работоспособность даже в случае сетевых разделений (partition tolerance). Однако, из-за асинхронной записи с возможной задержкой, система может быть нелинейно согласованной (eventually consistent), что означает, что в разных узлах данные могут быть немного несогласованными в течение какого-то времени.

- при сетевых сбоях система может разделиться на 2 раздельных кластера;

В этом сценарии система принимает модель CP (Consistency/Partition tolerance), где обеспечивается линейная согласованность (strong consistency) данных, даже в случае разделения на два кластера. При таком разделении система может предоставлять доступность только внутри каждого кластера, но не между ними.

- система может не прислать корректный ответ или сбросить соединение.

В этом случае реализация относится к модели EL (Eventual consistency/Loss tolerance) системы. Она может не обеспечить ни согласованность, ни надежность (liveness), так как может не присылать корректные ответы или сбрасывать соединение в случае сетевых проблем.

Согласно PACELC-теореме как бы вы классифицировали эти реализации?

## Задача 3

Могут ли в одной системе сочетаться принципы BASE и ACID? Почему?

В некоторых случаях можно применять аспекты ACID и BASE в разных частях системы для обработки различных типов данных или сценариев использования. Например, в системе можно использовать реляционную базу данных для хранения критически важных, ACID-ориентированных данных, а нереляционные хранилища с принципом BASE для хранения более масштабируемых данных, где некоторая задержка в согласованности приемлема.

Таким образом, принципы ACID и BASE могут использоваться в разных частях системы в соответствии с требованиями к согласованности, доступности, производительности и масштабируемости данных.

## Задача 4

Вам дали задачу написать системное решение, основой которого бы послужили:

- фиксация некоторых значений с временем жизни,
- реакция на истечение таймаута.

Вы слышали о key-value-хранилище, которое имеет механизм Pub/Sub. 
Что это за система? Какие минусы выбора этой системы? 

Key-value-хранилище с механизмом Pub/Sub - это база данных, которая позволяет хранить данные в виде пар ключ-значение, а также поддерживает механизм публикации и подписки на события.

Принцип работы механизма Pub/Sub следующий:

Публикатор (Publisher) публикует событие с определенным тегом.
Подписчик (Subscriber) подписывается на определенные теги событий.
Когда событие с определенным тегом публикуется, все подписчики, подписанные на этот тег, получают уведомление и могут обработать событие.

1.  Ограниченные возможности запросов: Key-value-хранилища, в основном, предоставляют простые операции получения и обновления значения по ключу. Они ограничены в функциональности для выполнения более сложных запросов, таких как фильтрация данных или агрегация.

2. Отсутствие сложных операций: Если потребуется выполнять сложные операции с данными, то key-value-хранилище с ограниченной функциональностью может оказаться недостаточным.

3.  Отказоустойчивость и масштабируемость: В зависимости от конкретной реализации key-value-хранилища с механизмом Pub/Sub, может быть ограничена отказоустойчивость и масштабируемость системы. Некоторые реализации могут не обеспечить репликацию данных, шардинг или горизонтальное масштабирование, что может стать узким местом при работе с большим объемом данных или высокой нагрузкой.

4.  Сложность обработки ошибок: В системах с Pub/Sub механизмом необходимо учитывать возможность потери сообщений или их дублирования. Обработка ошибок, обеспечение доставки сообщений и контроль истечения таймаутов требуют дополнительного программного кода, чтобы гарантировать надежность и целостность сообщений.
5.  Ограниченная консистентность: Некоторые key-value-хранилища могут предоставлять ограниченную консистентность данных из-за асинхронности и репликации.
---

