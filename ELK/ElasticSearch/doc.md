### ElasticSearch

- Elasticsearch в більшості випадків використовується для "full-text search", "log analytics", "application monitoring", та інші випадки використання які вимагають швидкий та ефективний пошуковий механізм
- Elasticsearch був спроектований дозволяючи "scale horizontally across multiple nodes in a cluster", що означає що можна горизонтально масштабувати nodes в межах кластера. Ця розприділена архітектура дозволяє включати високу доступність, відмовостійкість та масштабованість
- <b>Full-Text Search.</b> Elasticsearch відмінно підходить для повнотекстового пошуку, підтримуючи такі функції, як: нечітка відповідність (fuzzy matching), відповідність фрази (phrase matching), визначення коренів (stemming) і оцінка релевантності (relevance scoring). Він індексує текстові дані в режимі реального часу, роблячи пошукові операції швидкими та ефективними.
- <b>Real-Time Analytics.</b> Elasticsearch надає можливості аналітики в реальному часі, дозволяючи користувачам аналізувати та візуалізувати великі обсяги даних майже в реальному часі. Він підтримує агрегації, фільтрацію та складні запити для отримання інформації з даних.
- <b>Schema-Free JSON Documents.</b> Elasticsearch зберігає дані у форматі JSON і не потребує попередньо визначеної схеми. Ця гнучкість дозволяє користувачам індексувати та шукати документи з різними структурами та полями.
- <b>RESTful API.</b> Elasticsearch надає RESTful API, що полегшує взаємодію з пошуковою системою за допомогою запитів HTTP. Цей API підтримує операції CRUD, пошукові запити, керування індексами та адміністративні завдання
- <b>Powerful Query DSL.</b> Elasticsearch надає потужну Query DSL (Domain-Specific Language) для створення складних пошукових запитів. Query DSL дозволяє користувачам створювати запити за допомогою синтаксису на основі JSON для визначення умов, фільтрів, агрегацій тощо.
- <b>Scalability and Resilience.</b> Elasticsearch має високу масштабованість і стійкість (scalable and resilient), що дозволяє обробляти великі обсяги даних і одночасні запити (concurrent requests). Він підтримує такі функції, як: sharding, replication, and automatic failover to ensure data availability and reliability.

---

### Структура ElasticSearch

- як згадувалось вище, ElasticSearch зберігає дані за принципом «без схеми». Це означає, що немає необхідності визначати структуру даних наперед, як це відбувається з реляційними базами даних
- можна зробити аналогію Sql баз-даних із ElasticSearch щоб краще розуміти структуру
    - `Indexes`
      - `index` - колекція `documents` що має подібні характеристики.  
      - `index` - можна сказати, що це таблиця в relational databases 
      - кожний `index` - повинен мати унікальну назву
      - можна створювати декілька `index` в межах одного `cluster`
    - `Documents`
      - `document` - основий юніт із інформацією. Це аналог рядків в relational databases
      - кожний `document` зберігається як JSON-object та представляє один запис даних. `Documents` в межах одного `index` можуть мати різну структуру та `fields`
      - `Documents` - містить `field ID` який повинен бути унікальним. Ми можемо самостійно вказувати ID, або Elastic авматотично згенерує ID
    - `Fields`
      - `fields` - це key-value пара що і є контентом нашого `document`. Вони представляють атрибути які будуть індексуватись
      - вони можуть містити різні типи даних: strings, numbers, dates, booleans, arrays, and nested objects
      - Elastic підтримує динамічний mapping, мається на увазі що він автоматично визначає тип даних та індексує `fields`. Також, ми можемо явно вказувати mapping для `fields` тим самим контролюючи як поля індексуються та аналізуються
      - `fields` - можуть бути `indexed (searchable)`, `stored (retrievable)` або обидва. В залежності від наших вимог
    - `Mapping`
      - `mapping` - визначає як `fields` в межах `document` будуть індексуватись та зберігатись в Elastic. Він визначає тип даних, `analyzer` та інші `properties` для кожного `field`
      - існує два типи `mapping`: explicit та dynamic
        - `explicit` - це коли ми явно вказуємо структуру
        - `dynamic` - коли Elastic самостійно визначає структуру
      - `mapping` - може також визначати кастомний `analyzer`, `tokenizers` та `filters` щоб контролювати як `fields` аналізуються та індексуються
    - `Shard`
      - `shard` - це базова одиниця в horizontal scalability в Elastic. Це підмножина даних індексу та містить частину даних індекса
      - коли ми створюємо `index`, Elastic автоматично ділить його в декілька шардів щоб розприділити дані по різним `nodes` в `cluster`
      - `shards` - дозволяють Elasticsearch масштабувати та обробляти великі обсяги даних шляхом розподілу робочого навантаження між кількома `nodes`.
    - `Replica`
      - `replica` - це копія `shard`. Elastic дозволяє налаштувати одну або більше `replicas` для кожного `shard` щоб надати redundancy та high availability
      - `replicas`  служать резервними копіями основних `shards`. Якщо `node` яка містить основний `shard` зафейлиться, то Elastic візьме одну із `replica shards` і зробить її основною
    - `Cluster`
      - `cluster` - це колекція одного або більше `nodes` що спільно зберігають indexed дані в Elasticsearch
      - `nodes` в межах одного `cluster` спілкуються між собою для розподілу даних, координації операцій та підтримуванні `cluster health` 
      - `clusters` - highly available and fault-tolerant, що дозволяє 
      - Elasticsearch clusters are highly available and fault-tolerant, що забезпечує роботу навіть у разі збою `node` в цьому `cluster` 
    - `Node`
      - `node` - це один інстанс Elasticsearch що працює на фізичній чи віртуальній машині
      - `nodes` - відповідальні за: збереження даних, executing search queries, та координацію із іншими `nodes` в межах `cluster`
      - кожній `node` можна вказати спеціальну роль, такі як: master-eligible nodes, data nodes, or coordinating nodes базуючись на вимогах кластера

- В підсумку, Elasticsearch зберігає дані в `index`, які містять `documents` що є відображенням даних. Кожний `documents` складається із `fields` що зберігають власне дані

---

Інші документи варто прочитати в такому порядку:

- `data-types.md`
- `mapping.md`
- `analyzer.md`
- `inverted-index.md`
- `extra.md`

---

### Links

- article - https://blog.avenuecode.com/elasticsearch
- tokenizer, analyzer, token-filters - https://mallikarjuna91.medium.com/what-is-tokenizer-analyzer-and-filter-in-elasticsearch-317d4ec69ecc
- inverted index - https://medium.com/@sujathamudadla1213/what-is-the-inverted-index-in-elastic-search-f04df6f0c806#:~:text=Inverted%20Index%20Creation%3A%20The%20inverted,of%20documents%20for%20each%20term.

### TODO 

- read this article - https://mallikarjuna91.medium.com/elasticsearch-building-a-simple-e-commerce-search-with-elasticsearch-part-1-7ba2f75e684c
- read this article - https://codecurated.com/blog/introduction-to-analyzer-in-elasticsearch/