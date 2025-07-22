<p align="center">
  <a href="https://t.me/codyapi"><img src="https://img.shields.io/badge/Cody%20API-2684FF?style=for-the-badge&logo=telegram&logoColor=white" alt="Cody API" /></a>
</p>

<!-- Language Switch -->
<p align="right">
  <a href="#english">🇬🇧 English</a> | <a href="#русский">🇷🇺 Русский</a>
</p>

<a id="english"></a>
# Cody API

**Free, unlimited access to modern language and multimodal models via an OpenAI-compatible endpoint.**

### Contents
- [Key Features](#key-features)
- [1. Get an API Key](#1-get-an-api-key)
- [2. Quick Start](#2-quick-start-python--openai-sdk)
- [3. Endpoints](#3-endpoints)
- [4. Models](#4-models)
- [5. Rate Limits](#5-rate-limits)
- [6. Security & Privacy](#6-security--privacy)
- [7. Support](#7-support)

## Key Features
- ⚡ **Drop-in replacement for the OpenAI SDK** — change two lines of code.
- 🆓 **Zero cost** and no hard quotas.
- 🔒 **Zero-retention** architecture: no request content is persisted.
- 📷 **Multimodal**: text, image generation/editing, Text-to-Speech.
- 🚀 Constantly growing catalog of 400+ SOTA models.

---

## 1. Get an API Key
`POST https://cody.su/api/v1/get_api_key`

```python
import httpx
print(httpx.post("https://cody.su/api/v1/get_api_key").text)  # -> cody-...
```

## 2. Quick Start (Python + OpenAI SDK)

### Text
```python
from openai import OpenAI

client = OpenAI(base_url="https://cody.su/api/v1", api_key="cody-...")

resp = client.chat.completions.create(
    model="gpt-4.1",
    messages=[{"role": "user", "content": "Короткая история про котёнка"}],
)
print(resp.choices[0].message.content)
```

### Streaming
```python
from openai import OpenAI

client = OpenAI(base_url="https://cody.su/api/v1", api_key="cody-...")

completion = client.chat.completions.create(
    model="gpt-4.1",
    messages=[{"role": "user", "content": "Write a short story about two cats named Sonya and Alice"}],
    stream=True,
)

for chunk in completion:
    if chunk.choices and chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
```

### Image
```python
from openai import OpenAI
import base64, pathlib

client = OpenAI(base_url="https://cody.su/api/v1", api_key="cody-...")

img = client.images.generate(
    model="gpt-image-1",
    prompt="A veterinarian listening to a baby otter’s heartbeat, children’s book style",
)
pathlib.Path("otter.png").write_bytes(base64.b64decode(img.data[0].b64_json))
```

### Image Edit
```python
from openai import OpenAI, base64, pathlib

client = OpenAI(base_url="https://cody.su/api/v1", api_key="cody-...")

edited = client.images.edit(
    model="gpt-image-1",
    prompt="Add sunglasses",
    image=open("otter.png", "rb"),
    mask=open("mask.png", "rb"),
)
pathlib.Path("otter_edit.png").write_bytes(base64.b64decode(edited.data[0].b64_json))
```

> **Note:** The `images.generate` and `images.edit` endpoints return images **only** as base64 (`b64_json`).

### Video
```python
import requests, json

headers = {
    "Authorization": "Bearer cody-...",
    "Content-Type": "application/json",
}

payload = {
    "model": "wan-2.1",
    "prompt": "A cinematic shot of a futuristic city with flying cars",
    "ratio": "16:9",
    "quality": "480p",  # only 480p supported
    "duration": 4         # seconds; <= 4
}

resp = requests.post("https://cody.su/api/v1/video/generations", headers=headers, json=payload)
print(resp.json())
```

### Text-to-Speech
```python
from openai import OpenAI
import base64

client = OpenAI(base_url="https://cody.su/api/v1", api_key="cody-...")

audio = client.chat.completions.create(
    model="gpt-4o-mini-audio-preview",
    modalities=["text", "audio"],
    audio={"voice": "alloy", "format": "wav"},
    messages=[{"role": "user", "content": "Привет, мир!"}],
)
open("hello.wav", "wb").write(base64.b64decode(audio.choices[0].message.audio.data))
```

**Available voices**

`alloy`, `echo`, `fable`, `onyx`, `nova`, `shimmer`, `coral`, `verse`, `ballad`, `ash`, `sage`, `amuch`, `dan`

**Formats:** MP3, Opus, AAC, FLAC, WAV, PCM

#### Example Responses

<details>
<summary>chat.completions</summary>

```TXT
<Text answer>
```
</details>

<details>
<summary>images.generate</summary>

```json
{
  "created": 1710002222,
  "data": [
    {"b64_json": "<base64 PNG data>"}
  ]
}
```
</details>

<details>
<summary>images.edit</summary>

```json
{
  "created": 1710002223,
  "data": [
    {
      "b64_json": "<base64 PNG data>"
    }
  ]
}
```
</details>

<details>
<summary>video.generation</summary>

```json
{
  "created": 1677652288,
  "data": [
    {
      "b64_json": "<base64 Video data>"
    }
  ]
}
```
</details>

---

## 3. Endpoints

| Endpoint | Status |
|----------|--------|
| `chat.completions` | ✅ |
| `images.generate`  | ✅ |
| `images.edit`      | ✅ |
| `video.generations` | ✅ |

## 4. Models
```python
from openai import OpenAI
print(OpenAI(base_url="https://cody.su/api/v1", api_key="...").models.list())
```

#### Supported Models  
A concise subset is shown below. See the full catalog in [MODELS.md](./MODELS.md).

## 5. Rate Limits
- 20 requests per minute
- 5 requests per second  
Updates will be announced in our [Telegram channel](https://t.me/codyapi).

## 6. Security & Privacy
- Zero-retention: all data lives only in RAM.
- No request/response logging.

## 7. Support
- **Email:** vvirtr@gmail.com
- **Telegram:** [@vvirtr](https://t.me/vvirtr)
- **Channel:** [t.me/codyapi](https://t.me/codyapi)

---

<a id="русский"></a>
# Cody API

**Бесплатный безлимитный доступ к современным языковым и мультимодальным моделям через совместимый с OpenAI endpoint.**

### Оглавление
- [Ключевые возможности](#ключевые-возможности)
- [1. Получить API-ключ](#1-получить-api-ключ)
- [2. Быстрый старт](#2-быстрый-старт-python--openai-sdk)
- [3. Эндпоинты](#3-эндпоинты)
- [4. Модели](#4-модели)
- [5. Ограничения](#5-ограничения)
- [6. Безопасность и конфиденциальность](#6-безопасность-и-конфиденциальность)
- [7. Поддержка](#7-поддержка)

## Ключевые возможности
- ⚡ **Drop-in замена OpenAI SDK** — достаточно изменить две строки кода.
- 🆓 **Полностью бесплатно**, без жёстких квот.
- 🔒 **Zero-retention** архитектура: содержимое запросов не сохраняется.
- 📷 **Мультимодальность**: текст, генерация/редактирование изображений, TTS.
- 🚀 Каталог из 400+ актуальных SOTA моделей.

---

## 1. Получить API-ключ
`POST https://cody.su/api/v1/get_api_key`

```python
import httpx
print(httpx.post("https://cody.su/api/v1/get_api_key").text)  # -> cody-...
```

## 2. Быстрый старт (Python + OpenAI SDK)

### Текст
```python
from openai import OpenAI

client = OpenAI(base_url="https://cody.su/api/v1", api_key="cody-...")

resp = client.chat.completions.create(
    model="gpt-4.1",
    messages=[{"role": "user", "content": "Короткая история про котёнка"}],
)
print(resp.choices[0].message.content)
```

### Стриминг
```python
from openai import OpenAI

client = OpenAI(base_url="https://cody.su/api/v1", api_key="cody-...")

completion = client.chat.completions.create(
    model="gpt-4.1",
    messages=[{"role": "user", "content": "Привет, напиши историю про 2х кошек: Соню и Алису"}],
    stream=True,
)

for chunk in completion:
    if chunk.choices and chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
```

### Изображение
```python
from openai import OpenAI
import base64, pathlib

client = OpenAI(base_url="https://cody.su/api/v1", api_key="cody-...")

img = client.images.generate(
    model="gpt-image-1",
    prompt="Ветеринар слушает сердце детёныша выдры, стиль детской книги",
)
pathlib.Path("otter.png").write_bytes(base64.b64decode(img.data[0].b64_json))
```

### Редактирование изображения
```python
from openai import OpenAI
import base64, pathlib

client = OpenAI(base_url="https://cody.su/api/v1", api_key="cody-...")

edited = client.images.edit(
    model="gpt-image-1",
    prompt="Добавь солнечные очки",
    image=open("otter.png", "rb"),
    mask=open("mask.png", "rb"),
)
pathlib.Path("otter_edit.png").write_bytes(base64.b64decode(edited.data[0].b64_json))
```
> **Примечание:** эндпоинты `images.generate` и `images.edit` возвращают изображения **только** в Base64 (`b64_json`).

### Видео
```python
import requests, json

headers = {
    "Authorization": "Bearer cody-...",
    "Content-Type": "application/json",
}

payload = {
    "model": "wan-2.1",
    "prompt": "Кинематографичный кадр футуристического города с летающими машинами",
    "ratio": "16:9",
    "quality": "480p",  # поддерживается только 480p
    "duration": 4         # секунд; <= 4
}

resp = requests.post("https://cody.su/api/v1/video/generations", headers=headers, json=payload)
print(resp.json())
```

### Синтез речи
```python
from openai import OpenAI
import base64

client = OpenAI(base_url="https://cody.su/api/v1", api_key="cody-...")

audio = client.chat.completions.create(
    model="gpt-4o-mini-audio-preview",
    modalities=["text", "audio"],
    audio={"voice": "alloy", "format": "wav"},
    messages=[{"role": "user", "content": "Привет, мир!"}],
)
open("hello.wav", "wb").write_bytes(base64.b64decode(audio.choices[0].message.audio.data))
```

**Доступные голоса**

`alloy`, `echo`, `fable`, `onyx`, `nova`, `shimmer`, `coral`, `verse`, `ballad`, `ash`, `sage`, `amuch`, `dan`

**Formats:** MP3, Opus, AAC, FLAC, WAV, PCM

#### Примеры ответов

<details>
<summary>chat.completions</summary>

```TXT
<Text answer>
```
</details>

<details>
<summary>images.generate</summary>

```json
{
  "created": 1710002222,
  "data": [
    {"b64_json": "<base64 PNG data>"}
  ]
}
```
</details>

<details>
<summary>images.edit</summary>

```json
{
  "created": 1710002223,
  "data": [
    {"b64_json": "<base64 PNG data>"}
  ]
}
```
</details>

<details>
<summary>video.generation</summary>

```json
{
  "created": 1677652288,
  "data": [
    {
      "b64_json": "<base64 Video data>"
    }
  ]
}
```
</details>

---

## 3. Эндпоинты

| Endpoint | Статус |
|----------|--------|
| `chat.completions` | ✅ |
| `images.generate`  | ✅ |
| `images.edit`      | ✅ |
| `video.generations` | ✅ |

## 4. Модели
```python
from openai import OpenAI
print(OpenAI(base_url="https://cody.su/api/v1", api_key="...").models.list())
```

#### Поддерживаемые модели  
Полный каталог поддерживаемых моделей см. в [MODELS.md](./MODELS.md).


## 5. Ограничения
- 20 запросов в минуту
- 5 запросов в секунду  
Обновления публикуются в [Telegram-канале](https://t.me/codyapi).

## 6. Безопасность и конфиденциальность
- Zero-retention: данные хранятся только в RAM.
- Запросы/ответы не логируются.

## 7. Поддержка
- **Email:** vvirtr@gmail.com
- **Telegram:** [@vvirtr](https://t.me/vvirtr)
- **Канал:** [t.me/codyapi](https://t.me/codyapi)
