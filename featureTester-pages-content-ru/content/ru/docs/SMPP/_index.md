---
title : "SMPP"
description : ""
weight : 3

---

Для работы с SMPP протоколом описаны следующие шаги в рамках сценария: 
- Отправка запросов. Формирование запроса на базе предоставленного описания, с проверкой статуса ответа.
- Обработка входящих запросов. Ожидание и сверка поступившего запроса, формирования ответа на базе описания.

### Наименование json-тэгов и структура сообщений

Имена json-тэгов обязательных полей сообщений аналогичны спецификации. Опциональные параметры задаются в ключе “tlv”, имя ключей которого используются лишь для удобства прочтения сценария. Значение тэга берётся из соответствующего ключа.

TLV имеет следующую структуру:
```
{
    "tag": значение в десятичном формате
    "type": тип TLV-значения. Возможные: 
        "int"         - инт, кодируется в 4 байта,т.е. диапазон значений 00:00:00:00 - ff:ff:ff:ff 
        "word"        - инт, кодируется в 2 байта,т.е. 00:00 - ff:ff
        "byte"        - инт, кодируется в 1 байт, т.е. 00 - ff
        "string"      - значение по умолчанию.Добавляется по-умолчанию Null terminator в конец строки
        "raw"         - hex значение.Отправляется без модификаций
    "data": строковое поле для передачи значений. Например,"4294967295" для int, "501a09ff661213" для raw
    "without_null": bool ключ для отключения добавления null-terminator в string type  
}
```

### Отправка запросов:

Регулярное выражение шага - **`send (submit_sm|data_sm)(| via \w+):$`**.

Пример использования
```
    When send submit_sm:
    """
    {
        "message":{
            "source_addr":"102463111",
            "dest_addr":"102463222",
            "short_message":"body",
            "data_coding": 8,
            "validity_period": 3600,
            "registered_delivery": 1,
            "service_type":"sms",
            "source_addr_ton":1,
            "source_addr_npi":1,
            "dest_addr_ton":1,
            "dest_addr_npi":1,
            "esm_class":5,
            "replace_if_present_flag":0,
            "sm_default_msg_id":0,
            "tlv":{
                "DeliverReportPID": {
                    "tag":  5128,
                    "type": "byte",
                    "data": "1"
                }
            }
        },
        "response_status": 0
    }
    """
```

### Обработка входящих запросов:

Регулярное выражение шага - **`handle (deliver_sm|data_sm)(| via \w+):$`**.

Пример использования:
```
    Then handle data_sm:
    """
    {
        "receive":
        {
            "message":{
                "source_addr":"102463222",
                "dest_addr":"102463111",
                "message_payload":"oleg, gde proekt",
                "data_coding": 8,
                "validity_period": 0,
                "registered_delivery": 1,
                "service_type":"sms",
                "source_addr_ton":1,
                "source_addr_npi":1,
                "dest_addr_ton":1,
                "dest_addr_npi":1,
                "esm_class":5,
                "replace_if_present_flag":0,
                "sm_default_msg_id":1
            }
        },
        "send":
        {
            "status":0,
            "message_id":"OIL"
        }
    }
    """
```
