# Подробное описание VAD/BCD нотаций (ver3)

Исходный промпт https://github.com/bpmbpm/jsDOTsmartDesign/issues/1

В `ver3VAD` рассматриваются девять нотаций, которые объединяют общим понятием "верхнеуровневая модель ценности". В этом файле даётся метамодель каждой нотации в формате `source \ target`: строки - исходный тип объекта, столбцы - целевой тип, ячейка - допустимый предикат.

Краткая характеристика каждой нотации, ссылки на исходные определения и сопоставление с колонками нашей таблицы. См. [папку notation](https://github.com/bpmbpm/jsDOTsmartDesign/blob/main/ver3VAD/notation/)

| Нотация              | Файл JS                          | Файл онтологии                | Тип                | События |
| -------------------- | -------------------------------- | ----------------------------- | ------------------ | ------- |
| rdf-grapher ver9d    | `js/ver9d-vad.js`                | `notation/ver9d-vad.trig`     | VAD (опорная)      | Нет     |
| ARIS VAD             | `js/aris-vad.js`                 | `notation/aris-vad.trig`      | VAD                | Технические |
| SILA Union VAD       | `js/sila-vad.js`                 | `notation/sila-vad.trig`      | VAD                | Нет     |
| Business Studio VAD  | `js/businessstudio-vad.js`       | `notation/businessstudio-vad.trig` | VAD             | Нет     |
| BCD (обобщённая)     | `js/bcd-vad.js`                  | `notation/bcd-vad.trig`       | BCD                | Нет     |
| OSP BCD              | `js/osp-bcd.js`                  | `notation/osp-bcd.trig`       | BCD                | Нет     |
| Comindware BCD       | `js/comindware-bcd.js`           | `notation/comindware-bcd.trig` | BCD               | Нет     |
| Stormbpmn BCD (BPMN) | `js/stormbpmn-bcd.js`            | `notation/stormbpmn-bcd.trig` | Псевдо-BCD на BPMN | Да (BPMN events) |
| ArchiMate VAD-like   | `js/archimate-vad.js`            | `notation/archimate-vad.trig` | Архитектурная VAD | Да (Business event) |

---

## 1. rdf-grapher ver9d (опорная)

Источник: <https://github.com/bpmbpm/rdf-grapher/tree/main/ver9d>. Минимальная VAD-онтология: два класса и две связи.

### Типы объектов

| Тип            | Назначение                                     |
| -------------- | ---------------------------------------------- |
| TypeProcess    | Процесс верхнего уровня (cds-шеврон)           |
| ExecutorGroup  | Группа исполнителей (эллипс)                   |

### Матрица source \ target

| source \ target | TypeProcess | ExecutorGroup |
| --------------- | ----------- | ------------- |
| TypeProcess     | hasNext     | hasExecutor   |
| ExecutorGroup   | -           | -             |

### Предикаты

| Предикат      | source       | target          | Описание                       |
| ------------- | ------------ | --------------- | ------------------------------ |
| hasNext       | TypeProcess  | TypeProcess     | Горизонтальный порядок процессов |
| hasExecutor   | TypeProcess  | ExecutorGroup   | Связь процесса с группой исполнителей |

---

## 2. ARIS VAD

Источник: <https://docs.aris.com/10.0.27.0/yay-method-reference/en/#/home/494393/en/1>

### Типы объектов

| Тип                          | Назначение                            |
| ---------------------------- | ------------------------------------- |
| Function                     | Работа верхнего уровня                |
| Event                        | Технический маркер состояния          |
| Information object           | Документ, данные                      |
| Organizational unit / Role   | Исполнитель                           |
| Application system           | ИС                                    |
| Comment                      | Пояснение                             |

### Матрица source \ target

| source \ target | Function | Information object | Role | Application system | Comment |
| --------------- | -------- | ------------------ | ---- | ------------------ | ------- |
| Function        | precedes | creates            | -    | -                  | hasComment |
| Information object | isInputFor | - | - | - | hasComment |
| Role            | performs | -                  | -    | -                  | hasComment |
| Application system | supports | -               | -    | -                  | hasComment |
| Comment         | annotates | annotates         | annotates | annotates    | -       |

---

## 3. SILA Union VAD

Источник: <https://bpm3.ru/articles/modelirovanie-protsessov-v-notatsii-vad-v-sila-union/>

### Типы объектов

| Тип                  | Назначение                          |
| -------------------- | ----------------------------------- |
| Operation            | Операция/процесс                    |
| Document             | Входной или выходной документ       |
| Role                 | Исполнитель                         |
| Information system   | ИС                                  |
| Comment              | Пояснение                           |

### Матрица source \ target

| source \ target | Operation | Document | Role | Information system | Comment |
| --------------- | --------- | -------- | ---- | ------------------ | ------- |
| Operation       | precedes  | producesDocument | - | - | hasComment |
| Document        | isUsedBy  | -        | -    | -                  | hasComment |
| Role            | performs  | -        | -    | -                  | hasComment |
| Information system | supports | -      | -    | -                  | hasComment |
| Comment         | annotates | annotates | annotates | annotates | - |

---

## 4. Business Studio VAD

Источник: <https://www.businessstudio.ru/> ("Создание модели верхнего уровня").

### Типы объектов

| Тип                | Назначение                              |
| ------------------ | --------------------------------------- |
| VAD function       | Работа верхнего уровня (cds-шеврон)     |
| VAD function group | Группа функций                          |
| Term / Document    | Вход или выход                          |
| Org unit           | Ответственное подразделение             |
| Information system | ИС                                      |
| Comment            | Пояснение                               |

### Матрица source \ target

| source \ target | VAD function | VAD function group | Term | Org unit | Information system | Comment |
| --------------- | ------------ | ------------------ | ---- | -------- | ------------------ | ------- |
| VAD function    | precedes     | composesInto       | produces | - | - | hasComment |
| Term            | isInputFor   | -                  | -    | -        | -                  | hasComment |
| Org unit        | responsibleFor | -                | -    | -        | -                  | hasComment |
| Information system | supports  | -                  | -    | -        | -                  | hasComment |

---

## 5. BCD (обобщённая)

Источник: общая теория BCD (TOGAF, Open Group); в нашем коде используется как родительская абстракция для osp-bcd и comindware-bcd.

### Типы объектов

| Тип                | Назначение                          |
| ------------------ | ----------------------------------- |
| Business capability | Способность                        |
| Resource           | Ресурс на входе                     |
| Product            | Продукт на выходе                   |
| Owner              | Ответственный                       |
| Comment            | Пояснение                           |

### Матрица source \ target

| source \ target | Business capability | Resource | Product | Owner | Comment |
| --------------- | ------------------- | -------- | ------- | ----- | ------- |
| Business capability | dependsOn       | consumes | produces | - | hasComment |
| Owner           | owns                | -        | -       | -     | hasComment |
| Comment         | annotates           | annotates | annotates | annotates | - |

---

## 6. OSP BCD

Источник: <https://www.osp.ru/os/2026/02/13060742> (изложение - см. `source/osp-bcd-description.md`).

### Типы объектов

| Тип               | Назначение                         |
| ----------------- | ---------------------------------- |
| Capability        | Способность                        |
| Capability group  | Группа способностей (домен)        |
| Resource          | Ресурс/продукт (один класс)        |

### Матрица source \ target

| source \ target | Capability | Capability group | Resource |
| --------------- | ---------- | ---------------- | -------- |
| Capability      | -          | isPartOf         | consumes / produces |
| Resource        | -          | -                | -        |

**Особенность:** связь "следующая способность" отсутствует - это формальный признак BCD.

---

## 7. Comindware BCD

Источник: <https://kb.comindware.ru/> (изложение - см. `source/comindware-bcd-description.md`).

### Типы объектов

| Тип               | Назначение                                |
| ----------------- | ----------------------------------------- |
| Capability        | Способность с атрибутами Owner и Comment  |
| Capability level  | Уровень L1/L2/L3 (атрибут)                |

### Матрица source \ target

| source \ target | Capability     | Capability level |
| --------------- | -------------- | ---------------- |
| Capability      | decomposesInto | atLevel          |

**Особенность:** ресурсы и продукты вне ядра нотации; ответственный - атрибут блока.

---

## 8. Stormbpmn BCD через BPMN

Источник: <https://helpdesk.stormbpmn.com/>

### Типы объектов

| Тип             | Назначение                       |
| --------------- | -------------------------------- |
| BPMN event      | Событие                          |
| BPMN task       | Работа/способность               |
| Data object     | Вход или выход                   |
| Lane            | Зона ответственности             |
| Text annotation | Комментарий                      |

### Матрица source \ target

| source \ target | BPMN event | BPMN task | Data object | Lane | Text annotation |
| --------------- | ---------- | --------- | ----------- | ---- | --------------- |
| BPMN event      | -          | sequenceFlow | -        | -    | association     |
| BPMN task       | sequenceFlow | sequenceFlow | dataOutputAssociation | assignedLane | association |
| Data object     | -          | dataInputAssociation | -  | -    | association     |
| Lane            | -          | contains  | -           | -    | association     |
| Text annotation | association | association | association | association | -   |

---

## 9. ArchiMate VAD-like

Источник: <https://pubs.opengroup.org/architecture/archimate3-doc/>

### Типы объектов

| Тип                    | Назначение                          |
| ---------------------- | ----------------------------------- |
| Business event         | Событие                             |
| Business process       | Работа                              |
| Business role          | Роль                                |
| Business object        | Бизнес-объект                       |
| Application component  | Прикладной компонент                |
| Meaning                | Пояснение                           |

### Матрица source \ target

| source \ target | Business event | Business process | Business object | Business role | Application component | Meaning |
| --------------- | -------------- | ---------------- | --------------- | ------------- | --------------------- | ------- |
| Business event  | -              | triggering       | -               | -             | -                     | association |
| Business process | triggering    | triggering       | access          | -             | serving               | association |
| Business object | -              | access           | -               | -             | -                     | association |
| Business role   | -              | assignment       | -               | -             | -                     | association |
| Application component | -        | serving          | -               | -             | -                     | association |
| Meaning         | association    | association      | association     | association   | association           | -       |

---

## Соответствие колонкам нашей таблицы (свод)

| Колонка `ver3VAD` | ver9d        | ARIS         | SILA       | Business Studio | OSP BCD     | Comindware BCD | Stormbpmn BCD | ArchiMate     |
| ----------------- | ------------ | ------------ | ---------- | --------------- | ----------- | -------------- | ------------- | ------------- |
| type=event        | технический  | Event        | -          | -               | -           | -              | BPMN event    | Business event |
| type=activity     | TypeProcess  | Function     | Operation  | VAD function    | Capability  | Capability     | BPMN task     | Business process |
| name              | подпись      | подпись      | подпись    | подпись         | подпись     | подпись        | подпись       | подпись       |
| input             | -            | Information object (in) | Document (in) | Term (in) | Resource    | -              | Data object (in) | Business object (in) |
| output            | -            | Information object (out) | Document (out) | Term (out) | Resource (Product) | - | Data object (out) | Business object (out) |
| role              | ExecutorGroup | Role        | Role       | Org unit        | Capability group | Owner (атрибут) | Lane | Business role |
| system            | -            | Application system | IS    | Information system | -        | -              | -             | Application component |
| comment           | -            | Comment      | Comment    | Comment         | Comment     | Comment (атрибут) | Text annotation | Meaning |
