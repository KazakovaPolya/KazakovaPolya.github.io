---
title : "Mutltipart/related"
description : "Отправка составных сообщений"
weight : 4

---

Тип [Multipart/Related](https://datatracker.ietf.org/doc/html/rfc2387) предназначен для составных объектов, состоящих из нескольких взаимосвязанных частей тела.  

Для работы с multipart/related следует в заголовке Content-Type указать значение "multipart/related". Части сообщения указываются в поле "multipart".

Поле “multipart” содержит следующие ключи:

- “boundary” [опциональный] - содержит строку-разделитель составных частей сообщения
- “parts” [опциональный] - содержит список частей сообщения

Если параметр boundary не указан, то он генерируется автоматически.

Поле “parts” представляет собой список объектов со следующими полями:

- “headers” [обязательный] - содержит список заголовков
- “body”/“hex” [обязательный] - содержит ожидаемую json/xml-структуру или тело raw-сообщения

> Каждая часть обязательно должна содержать заголовок Content-Type и тело body/hex. 

> Часть подразумевается как raw, если в поле "multipart" указан ключ "hex" или заголовок Content-Type запроса не совпадает ни с одним из следующих: application/json, application/xml, text/xml


Пример:
```json
"multipart": {
    "boundary":"MultipartBoundary",
    "parts":[
        {
            "headers": {
               "Content-Type":"application/json"
            },
            "body":{
                "a": {
                    "b": 0
                }
            }
        },
        {
            "headers": {
               "Content-Type":"hex"
            },
            "hex":"010203"
        }
    ]
}
```

>В текущей реализации не предусмотрен сценарий, ожидающий multipart-ответ. Поэтому в полях шагов синхронного запроса ("response"), получения ассинхронного ответа и обработки входящего запроса ("send") ключ "multipart" не ожидается.

## Пример отправки и обработки асинхронного multipart-запроса

```json
  When async send http.POST:
  """
  {
    "endpoint": "/url",
    "request": {
      "headers": {
          "multipart/related"
      },
      "multipart": {
        "boundary": "MultipartBoundary",
        "parts": [
          {
            "headers": {
                "Content-Type": "application/json"
            },
            "body": {
                "imsi": "240000000000001",
                "locationInfo": {
                  "vlr": "79217772323"
                }
            }
          },
          {
            "headers":{
                "Content-Type":"application/hex",
                "Content-ID":"some-raw",
                "Content-TRANSFER-Encoding":"binary"
            },
            "hex":"0000040082000a0c499c9ac030075bcde8008b000a01f00a55c0576e40000800860001000088000"
          }
        ]
      }
    }
  }
  """

  Then handle http request:
  """
  {
      "endpoint": "/url",
      "request":{
          "headers": {
          "multipart/related"
      },
        "multipart": {
          "boundary": "MultipartBoundary",
          "parts": [
            {
              "headers": {
                  "Content-Type": "application/json"
              },
              "body": {
                  "imsi": "240000000000001",
                  "locationInfo": {
                    "vlr": "79217772323"
                  }
              }
            },
            {
              "headers":{
                  "Content-Type":"application/hex",
                  "Content-ID":"some-raw",
                  "Content-TRANSFER-Encoding":"binary"
              },
              "hex":"0000040082000a0c499c9ac030075bcde8008b000a01f00a55c0576e40000800860001000088000"
            }
          ]
        }
      },
      "send":{
          "body":{
              "status": "OK"
          },
          "status_code": 200,
      }
  }
  """

  And receive async http response:
  """
  {
    "body":{
        "status": "OK"
    },
    "status_code":200
  }
  """

```

## Пример отправки синхронного multipart-запроса

```json
  When send http.POST:
  """
  {
    "endpoint": "/url",
    "request": {
      "headers": {
          "multipart/related"
      },
      "multipart": {
        "boundary": "MultipartBoundary",
        "parts": [
          {
            "headers": {
                "Content-Type": "application/json"
            },
            "body": {
                "imsi": "240000000000001",
                "locationInfo": {
                  "vlr": "79217772323"
                }
            }
          },
          {
            "headers":{
                "Content-Type":"application/hex",
                "Content-ID":"some-raw",
                "Content-TRANSFER-Encoding":"binary"
            },
            "hex":"0000040082000a0c499c9ac030075bcde8008b000a01f00a55c0576e40000800860001000088000"
          }
        ]
      }
    },
    "response": {
      "body":{
              "status": "OK"
          },
          "status_code": 200,
    }
  }
  """
