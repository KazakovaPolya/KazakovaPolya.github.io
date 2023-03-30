---
title : "RAW Configuration"
description : "Настройка RAW-подключения"
weight : 4

---

```json
{"sctp": {
  "clients": {
    "default": "d",
    "list": {
      "d": {
        "address": "127.0.0.1",
        "port": 3838
      }
    }
  },
  "servers": {
    "default": "w",
    "list": {
      "w": {
        "ip": "::",
        "port": 3839
      }
    }
  }}}
```