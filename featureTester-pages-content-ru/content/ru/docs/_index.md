---
title: "Документация"
description: "FeatureTester - приложение для тестирования по сценариям в Gherkin-описании"
weight: 2
menu:
    main:
        weight: 20
---

FeatureTester (далее FT) - приложение для проведения автоматических функциональных тестов по сценариям в [Gherkin-описании](./gherkin).
FT используется для проверки функциональности тестируемых приложений без учета их внутренней реализации, подключаясь по всем доступным протоколам к тестируемому приложению и эмулируя работу внешних приложений.


![FeatureTester](/featureTester/images/ft_scheme.png)

Для описания подаваемых на вход сценариев используется язык [Gherkin](./gherkin).

FeatureTester может взаимодействовать с приложением, используя следующие протоколы:
- [Diameter](https://ru.wikipedia.org/wiki/DIAMETER)
- [HTTP](https://ru.wikipedia.org/wiki/HTTP)
- [MAP](https://ru.wikipedia.org/wiki/Mobile_Application_Part)
- [SMPP](https://ru.wikipedia.org/wiki/SMPP)

![Протоколы, используемые FeatureTeser](/featureTester/images/protocols.png)

>Возможно использовать одновременно несколько протоколов в рамках одного сценария
> FeatureTester является инициирующем приложением, поэтому нельзя начинать сценарии с ожиданием входящего запроса

____
## Запуск тестов FeatureTester

Структура директории тестируемого приложения **`app`** обычно выглядит следующим образом:

```sh
app
│
└───features                // сценарии для тестирования
│   └───functionalityName1  // базовый функционал
│   │   │   featureName1.feature
│   │   │   ...
│    ...
│
└───testdata                // тестовые данные
│
└───config                  // конфигурационные файлы
    └───app                 // файлы конфигурации тестируемого приложения
    └─── ...
    └───conf.json

```

Сценарии для тестирования находятся в директории features. Подробную инструкцию по написанию функциональным тестам можно найти [здесь](https://git.protei.ru/MobileDevelop/functional-tests/-/blob/master/README.md).


Для запуска тестов желательно использовать последнюю версию [FeatureTester](https://jenkins.protei.ru/job/team4/job/Testing/job/Applications/job/FeaturesTester/lastSuccessfulBuild/). 

Получение исполняемого файла.

```sh
tar xvfz ./FeatureTester/*.tar.gz
tester=$(find . -executable -type f | grep featureTester)
```

Тестер принимает следующие ключи:
- **`-config`** &emsp;&emsp;&nbsp; # Путь до конфигурационного файла
- **`-features`** &emsp; # Путь до директории со сценариями
- **`-format`** [опциональный] &emsp;&emsp;&nbsp; # Формат вывода отчета

Параметр `-format` может принимать следующие значения:
- pretty (форматированный вывод в консоль)
- cucumber:report.json (вывод Cucumber)
- events
- junit
- progress

Примеры запуска:
```sh
$tester -config=path_to_conf_file/conf.json -features=path_to/features -format=pretty
```


```sh
$tester -config=path_to_conf_file/conf.json -features=path_to/features -format=cucumber:report.json
```
