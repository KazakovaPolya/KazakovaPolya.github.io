---
title : "История версий"
description : "История версий"
weight : 3
--- 
#### 1.30.3.198 (2022-11-22)
[Скачать версию 1.30.3.198](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/198/artifact/featureTester1.30.3.198.tar.gz)

[MOBILE_TEST-55](https://youtrack.protei.ru/issue/MOBILE_TEST-55) - Basic **Task**, Заказчик: **НТЦ Протей**
- Добавил в лог вывод конфигурации.
- Дефолтный таймаут для МАР изменён на 30 сек

#### 1.30.2.197 (2022-11-16)
[Скачать версию 1.30.2.197](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/197/artifact/featureTester1.30.2.197.tar.gz)

[Mobile_TestingApps-144](https://youtrack.protei.ru/issue/Mobile_TestingApps-144) Расширить логирование
- Basic **Freq**, Заказчик: **НТЦ Протей**
- diameter. В серверный хендлер добавил лог

#### 1.30.1.195 (2022-11-08)
[Скачать версию 1.30.1.195](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/195/artifact/featureTester1.30.1.195.tar.gz)

Рефакторинг кода
[Mobile_TestingApps-144](https://youtrack.protei.ru/issue/Mobile_TestingApps-144) Расширить логирование
- Basic **Freq**, Заказчик: **НТЦ Протей**
- Дополнены логи для diameter

#### 1.30.0.184 (2022-10-11)
[Скачать версию 1.30.0.184](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/184/artifact/featureTester1.30.0.184.tar.gz)

[Mobile_TestingApps-144](https://youtrack.protei.ru/issue/Mobile_TestingApps-144) Расширить логирование
- Basic **Freq**, Заказчик: **НТЦ Протей**
- AppendHandler добавлен дебаг-лог

[Mobile_TestingApps-148](https://youtrack.protei.ru/issue/Mobile_TestingApps-148) smpp. Не обрабатывается входящее сообщение
- Basic **Bug**, Заказчик: **НТЦ Протей**
- Переписана inflight обработка входящих сообщений

#### 1.29.1.169 (2022-08-23)
[Скачать версию 1.29.1.169](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/169/artifact/featureTester1.29.1.169.tar.gz)

[Mobile_TestingApps-143](https://youtrack.protei.ru/issue/Mobile_TestingApps-143) SMPP.Некорректное формирование сообщения для отправки
- Basic **Bug**, Заказчик: **НТЦ Протей**
- Обновление go-smpp до версии с фиксом падения message_payload в пустой TLV-словарь

#### 1.29.0.168 (2022-08-22)
[Скачать версию 1.29.0.168](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/168/artifact/featureTester1.29.0.168.tar.gz)

[Mobile_TestingApps-143](https://youtrack.protei.ru/issue/Mobile_TestingApps-143) SMPP.Некорректное формирование сообщения для отправки
- Basic **Bug**, Заказчик: **НТЦ Протей**
- Исправлено переиспользование структуры из общего словаря

#### 1.28.1.166 (2022-08-03)
[Скачать версию 1.28.1.166](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/166/artifact/featureTester1.28.1.166.tar.gz)

[Mobile_TestingApps-141](https://youtrack.protei.ru/issue/Mobile_TestingApps-141) MAP.Добавить поддержку  DeleteSubscriberData
- Basic **Freq**, Заказчик: **НТЦ Протей**
- Обновлена либа go-models до 1.12.0

#### 1.28.0.165 (2022-07-29)
[Скачать версию 1.28.0.165](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/165/artifact/featureTester1.28.0.165.tar.gz)

[Mobile_TestingApps-140](https://youtrack.protei.ru/issue/Mobile_TestingApps-140) SCTP. Работа с raw пакетами
- Basic **Freq**, Заказчик: **НТЦ Протей**
- Добавлен функционал работы с raw.SCTP пакетами

#### 1.27.1.163 (2022-05-12)
[Скачать версию 1.27.1.163](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/163/artifact/featureTester1.27.1.163.tar.gz)

[Mobile_TestingApps-131](https://youtrack.protei.ru/issue/Mobile_TestingApps-131) Служебные сообщения для NRF(5g)
- Basic **Freq**, Заказчик: **НТЦ Протей**
- Исправлена пропущенная проверка NRF

#### 1.27.1.162 (2022-05-12)
[Скачать версию 1.27.1.162](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/162/artifact/featureTester1.27.1.162.tar.gz)

[Mobile_TestingApps-131](https://youtrack.protei.ru/issue/Mobile_TestingApps-131) Служебные сообщения для NRF(5g)
- Basic **Freq**, Заказчик: **НТЦ Протей**
- Исправлена пропущенная проверка NRF

#### 1.27.0.161 (2022-05-06)
[Скачать версию 1.27.0.161](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/161/artifact/featureTester1.27.0.161.tar.gz)

[Mobile_TestingApps-131](https://youtrack.protei.ru/issue/Mobile_TestingApps-131) Служебные сообщения для NRF(5g)
- Basic **Freq**, Заказчик: **НТЦ Протей**
- Добавил функционал "фоновой" работы с NRF

#### 1.26.3.159 (2022-04-25)
[Скачать версию 1.26.3.159](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/159/artifact/featureTester1.26.3.159.tar.gz)

[Mobile_TestingApps-129](https://youtrack.protei.ru/issue/Mobile_TestingApps-129) Лочится логика при тестах MI
- Basic **Bug**, Заказчик: **НТЦ Протей**
- Добавил удаление связки при таймауте ожидания ответа от сценарной логики

#### 1.26.2.158 (2022-04-21)
[Скачать версию 1.26.2.158](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/158/artifact/featureTester1.26.2.158.tar.gz)

[Mobile_TestingApps-133](https://youtrack.protei.ru/issue/Mobile_TestingApps-133) DRA. used nonexistent field's name S
- Basic **Freq**, Заказчик: **НТЦ Протей**
- Вместо проверки пришедшего запроса в null передавалась diameter структура ответа

#### 1.26.1.157 (2022-04-19)
[Скачать версию 1.26.1.157](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/157/artifact/featureTester1.26.1.157.tar.gz)

[Mobile_TestingApps-132](https://youtrack.protei.ru/issue/Mobile_TestingApps-132) runtime error в http async шаге
- Basic **Bug**, Заказчик: **НТЦ Протей**
- Исправил пропущенное создание sync.WaitGroup из-за которых валились runtime.error

#### 1.26.0.156 (2022-04-18)
[Скачать версию 1.26.0.156](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/156/artifact/featureTester1.26.0.156.tar.gz)

[Mobile_TestingApps-112](https://youtrack.protei.ru/issue/Mobile_TestingApps-112) HTTP. Использование нескольких клиентов и серверов
- Basic **Freq**, Заказчик: **НТЦ Протей**
- Добавлена возможность настройки и использования нескольких клиентов и серверов

[Mobile_TestingApps-106](https://youtrack.protei.ru/issue/Mobile_TestingApps-106) Поддержать работу HTTPS запрос/ответами
- Basic **Freq**, Заказчик: **НТЦ Протей**
- Реализована возможность использования сертификатов

#### 1.25.2.155 (2022-02-14)
[Скачать версию 1.25.2.155](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/155/artifact/featureTester1.25.2.155.tar.gz)

[Mobile_TestingApps-119](https://youtrack.protei.ru/issue/Mobile_TestingApps-119) Изменение типа значения AVP EdrxCycleLengthValue
- Basic **Freq**, Заказчик: **НТЦ Протей**
- Добавлен новый тип кастомного анмаршелинга, позволяющего преобразовывать вводимые int значения и отправлять их в hex представления дополняя лидирующими нулями

#### 1.25.1.154 (2022-02-11)
[Скачать версию 1.25.1.154](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/154/artifact/featureTester1.25.1.154.tar.gz)

[Mobile_TestingApps-119](https://youtrack.protei.ru/issue/Mobile_TestingApps-119) Изменение типа значения AVP EdrxCycleLengthValue
- Basic **Freq**, Заказчик: **НТЦ Протей**
- Актуализация версии библиотеки go-diameter до v1.10.1

#### 1.25.0.152 (2022-02-10)
[Скачать версию 1.25.0.152](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/152/artifact/featureTester1.25.0.152.tar.gz)

[Mobile_TestingApps-117](https://youtrack.protei.ru/issue/Mobile_TestingApps-117) Добавление T6a интерфейса
- Basic **Freq**, Заказчик: **НТЦ Протей**
- Актуализация версии библиотеки go-diameter

#### 1.24.3.151 (2022-02-10)
[Скачать версию 1.24.3.151](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/151/artifact/featureTester1.24.3.151.tar.gz)

[Mobile_TestingApps-107](https://youtrack.protei.ru/issue/Mobile_TestingApps-107) HTTP. Не работает выставление query в шагах на отправку
- Basic **Bug**, Заказчик: **НТЦ Протей**
- Добавил пропущенное выставление query параметров при отправке запросов
- Исправил сравнение headers в handle шаге

#### 1.24.3.150 (2021-11-26)
[Скачать версию 1.24.3.150](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/150/artifact/featureTester1.24.3.150.tar.gz)

[Mobile_TestingApps-107](https://youtrack.protei.ru/issue/Mobile_TestingApps-107) HTTP. Не работает выставление query в шагах на отправку
- Basic **Bug**, Заказчик: **НТЦ Протей**
- Добавил пропущенное выставление query параметров при отправке запросов
- Исправил сравнение headers в handle шаге

#### 1.24.2.148 (2021-11-15)
[Скачать версию 1.24.2.148](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/148/artifact/featureTester1.24.2.148.tar.gz)

[Mobile_TestingApps-101](https://youtrack.protei.ru/issue/Mobile_TestingApps-101) Добавить проверку наличия/отсутствия тегов в xml структуре
- Basic **Freq**, Заказчик: **НТЦ Протей**
- Добавлены ключи null, not_null.Указываются значение ключей как список строк, при необходимости проверить вложенный ключ,
- следует указать его через точку.Например, для проверки ключа 'c' тела {"a":1,"b":{"c":2}} следует указать  "not_null":["b.c"]

#### 1.24.1.147 (2021-11-08)
[Скачать версию 1.24.1.147](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/147/artifact/featureTester1.24.1.147.tar.gz)

[Mobile_TestingApps-100](https://youtrack.protei.ru/issue/Mobile_TestingApps-100) HTTP. Добавлена обработка заголовка text/xml
- Basic **Task**, Заказчик: **НТЦ Протей**

#### 1.24.0.144 (2021-11-03)
[Скачать версию 1.24.0.144](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/144/artifact/featureTester1.24.0.144.tar.gz)

[Mobile_TestingApps-97](https://youtrack.protei.ru/issue/Mobile_TestingApps-97) Ошибка "request with same type already wait response"
- Basic **Bug**, Заказчик: **НТЦ Протей**
- Добавлена доп. вложенность с привязкой к идентификатору запроса map[requestType][identity]*responseCtx{}

[Mobile_TestingApps-93](https://youtrack.protei.ru/issue/Mobile_TestingApps-93) Добавить endpoint-ы RegisterSS/ActivateSS/EraseSS/DeactivateSS для SigTester
- Basic **Freq**, Заказчик: **НТЦ Протей**
- Выставляем значение OpCode, исходя из типа МАР-запроса. Словарик OpCode только для сообщений сидящих на одном эндпоинте

#### 1.23.0.142 (2021-11-02)
[Скачать версию 1.23.0.142](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/142/artifact/featureTester1.23.0.142.tar.gz)

[Mobile_TestingApps-92](https://youtrack.protei.ru/issue/Mobile_TestingApps-92) HTTP. Добавить поддержку XML
- Basic **Freq**, Заказчик: **НТЦ Протей**
- Работа завязана на указание заголовка Content-Type = "application/xml". XML тело запроса должно указываться, как строковое значение(в кавычки)



v1.23.0         Merge branch 'XML'

v1.22.2         fix units after migrate to full naming

v1.22.1         Mobile_TestingApps-93 add new endpoints

v1.22.0         Mobile_TestingApps-90 MAP. Отправки разных типов сообщений не дожидаясь их ответа.

v1.21.0         Mobile_TestingApps-89 Требуется сравнение значений SessionId AVP
                Если значение sessionId был указан в сценарии для сообщений, то будет выполняться проверка с полученным.
                Если значение не было указано - будет как и раньше выставляться полученное от удалённой стороны или сгенерированное ft

v1.20.0         Mobile_TestingApps-25. При явном указании SessionID использовать его при отправке, не генеря свой на фабрике.

v1.19.2         refactor http steps

v1.19.1         Mobile_TestingApps-75 add slice to handle

v1.19.0         add name of field to null checvk

v1.18.1         upd go-diam version

v1.18.0         Mobile_TestingApps-80. SMSC2SMSC feature

v1.17.10        upd diameter lib version

v1.17.9         Mobile_TestingApps-80. Fixes

v1.17.8         Mobile_TestingApps-81. Fix missed addition header value to caseCtx

v1.17.7         Mobile_TestingApps-79. Fixed getting a MAP object in multiple entries of key 'MAP' on incoming indication

v1.17.6         Mobile_TestingApps-78. update diam version.fix unmarshalling error

v1.17.5         set to nil asynchError after processing

v1.17.4         add timeout of sending map request

v1.17.3         Mobile_TestingApps-77. Fix wrong path join

v1.17.2         Mobile_TestingApps-77 fix query parameter append

v1.17.1         change optionality of body check

v1.17.0         Mobile_TestingApps-69. Migrate to godog v0.11

v1.16.2         Mobile_TestingApps-51. Unit tests

v1.16.1         Mobile_TestingApps-72. Fix optionality of body
                extend units

v1.16.0         Mobile_TestingApps-70 Add query and endpoint for handle step
                Mobile_TestingApps-71 support all methods for handle step

v1.15.0         Mobile_TestingApps-65.Add support of status_code for http requests

v1.14.0         Migration to go-diameter v1.8.1

v1.13.0         Mobile_TestingApps-63 Analize http headers for response

v1.12.0         Mobile_TestingApps-62. Set http-method type in scenario step defenition

v1.11.2         fix unit-tests

v1.11.1         Mobile_TestingApps-59. Slice comparing

v1.11.0         Mobile_TestingApps-48. Http2 server part
                diameter multihouming config

v1.10.0         Mobile_TestingApps-50: Extend logging
                Add SMPP-connection(s) pool by name

v1.9.0          Mobile_TestingApps-49. Add work with http-headers

v1.8.6          Mobile_TestingApps-44. update version of diameter

v1.8.5          fix superfluous response.WriteHeader call

v1.8.4          Mobile_TestingApps-47.update go-diameter version.
                Add supporting Zh, SGdGdd interfaces

v1.8.3          Mobile_TestingApps-46. Add slice comparing

v1.8.2          Upd go.mod. go-models v1.7

v1.8.1          Fix bug of identifier in api.HandleRequest

v1.8.0          Mobile_TestingApps-14. Migrate to new CompareError struct.
                Show different values instead of full message

v1.7.0          Mobile_TestingApps-42. Migrate to map[string]interface json comparing

v1.6.3          change type comparing
                add indication search in server-side

v1.6.2          Extend answer message for ServeHTTP
                Add unit-test for ServeHTTP
                Change names of json-tags to sigtran-like style
                Fix panic on nil values
                Update unit-test to new model of input

v1.6.1          Mobile_TestingApps-33. MAP. Add "send" feature to handle indication

v1.6.0          Mobile_TestingApps-33. Add support of MAP protocol via Sigtester

v1.5.0          Mobile_TestingApps-39. PEA for answers with ResultCode 3xxx

v1.4.0          Mobile_TestingApps-36. Deleting from bound after reading

v1.3.2          Update go-models version

v1.3.1          Update go.mod

v1.3.0          Mobile_TestingApps-27. Get/Set diameter connection by name

v1.2.0          rebase master to reworked

v1.1.0          feature(API model) - extend IMS description

v1.0.0          Mobile_DiamTester-19. Migration to diameter v1









