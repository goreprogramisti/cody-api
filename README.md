# cody-api
Бесплатное безлимитное api llm моделей

> Drop-in замена OpenAI SDK: поменяйте **две строки** — и пользуйтесь 15 моделями без лимитов и оплаты.

# Получить API-ключ
Чтобы получить api ключ, вам нужно отослать post запрос на https://cody.su/api/v1/get_api_key
```python
import httpx

client = httpx.Client()

response = client.post("https://cody.su/api/v1/get_api_key")
response.raise_for_status()

print(response.text)
```

# Быстрый старт (Python / OpenAI SDK)
## Генерация текста
```python
from openai import OpenAI

client = OpenAI(
    base_url="https://cody.su/api/v1",
    api_key="cody-...", 
)

completion = client.chat.completions.create(
    model="gpt-4.1",
    messages=[
        {"role": "user", "content": "Привет, напиши историю про 2х кошек, с именем Соня и Алиса"}
    ]
)

print(completion.choices[0].message.content)
```
>Привет! Вот тебе история о двух кошках — Соня и Алиса.
>
>---
>
>Жили-были две кошки в уютной старинной усадьбе — Соня и Алиса. Соня была серой с бледно-голубыми глазами, почти как ночное небо, а Алиса — рыжая с блестящими зелёными глазами, как изумруды. Они были неразлучны, как два лучших друга, и часто придумывали всякие приключения.
>
>Одним солнечным утром Соня решила подняться на чердак, чтобы разглядеть старые книги и тряпки. Алиса поспешила за ней, чуть ли не напевая отпускную песню. Там они нашли старинный сундук, покрытый пылью, который, похоже, долгое время никто не открывал.
>
>— Интересно, что внутри? — прошептала Соня, вытягивая лапки.
>
>Алиса улыбнулась и дернула ручку замка. Вскоре крышка распахнулась, и перед ними появился целый ворох старых фотографий, ювелирных украшений и темных, загадочных карт. Соня подумала: «Может, тут спрятан клад?» — и лицо у неё засияло от предвкушения.
>
>Но вдруг, среди всего этого добротного хаоса, они нашли маленькую старую книгу — дневник хозяйки усадьбы. В его страницах была записана история о путешествиях, любви и том, как однажды она нашла волшебную лесную тропинку, которая велела её в сказочный мир.
>
>— Представляешь, Алиса, — сказала Соня, — может, и мы однажды откроем свою магическую тропинку, которая приведет нас к приключениям!
>
>Алиса нежно тронула её за ухо и промурлыкала:
>
>— Мне бы очень хотелось, чтобы наша жизнь была чуть-чуть волшебнее. А пока будем исследовать наш дом как настоящие искатели сокровищ.
>
>И так, две кошки в своих приключениях продолжали открывать новые грани старой усадьбы, словно каждый день был новым сказочным приключением.
>
>---
>
>Если хочешь более длинную или насыщенную деталями историю — скажи, добавлю!

### Также доступен стриминговый ответ:
```python
from openai import OpenAI

client = OpenAI(
    base_url="https://cody.su/api/v1",
    api_key="cody-...", 
)

completion = client.chat.completions.create(
    model="gpt-4.1",
    messages=[
        {"role": "user", "content": "Привет, напиши историю про 2х кошек, с именем Соня и Алиса"}
    ],
    stream=True
)

for chunk in completion:
    if chunk.choices and chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
```

## Создание изображений
```python
from openai import OpenAI
import base64

client = OpenAI(
    base_url="https://cody.su/api/v1",
    api_key="cody-...",
)

prompt = """
Рисунок из детской книги, на котором ветеринар с помощью стетоскопа слушает сердцебиение детеныша выдры.
"""

result = client.images.generate(
    model="gpt-image-1",
    prompt=prompt
)

image_base64 = result.data[0].b64_json
image_bytes = base64.b64decode(image_base64)

with open("otter.png", "wb") as f:
    f.write(image_bytes)
```
><img src="https://github.com/user-attachments/assets/f5a8f75e-1c28-46fc-bbb1-769a816fb210" width="40%">

## Изменение изображений
```python
from openai import OpenAI
import base64

client = OpenAI(
    base_url="https://cody.su/api/v1",
    api_key="cody-...",
)

prompt = """
Добавь эффект свечения на лого.
"""

result = client.images.edit(
    model="gpt-image-1",
    image=[
        open("image.png", "rb"),
        #open("image1.png", "rb"),
        #open("image2.png", "rb"),
    ],
    prompt=prompt
)

image_base64 = result.data[0].b64_json
image_bytes = base64.b64decode(image_base64)

with open("cody.png", "wb") as f:
    f.write(image_bytes)
```
><img src="https://github.com/user-attachments/assets/f41bcf25-1d7f-4126-93b0-ba8725027e5f" width="40%"> <img src="https://github.com/user-attachments/assets/48ad1841-8eeb-4070-a082-c19cbcfea40c" width="40%">

## Генерация аудио
```python
from openai import OpenAI
import base64

client = OpenAI(
    base_url="https://cody.su/api/v1",
    api_key="cody-...",
)

completion = client.chat.completions.create(
    model="gpt-4o-audio-preview",
    modalities=["text", "audio"],
    audio={"voice": "alloy", "format": "wav"},
    messages=[
        {
            "role": "user",
            "content": "Придумай рекламный текст про бесплатное безлимитное api llm моделей: Cody API"
        }
    ]
)

wav_bytes = base64.b64decode(completion.choices[0].message.audio.data)
with open("file.wav", "wb") as f:
    f.write(wav_bytes)
```
>https://github.com/user-attachments/assets/749f85a5-91e0-43de-acd8-73bb6b3a6074

# Поддерживаемые модели
```json
[
  "o3",
  "grok-3-mini",
  "deepseek-r1",
  "gpt-4.1",
  "deepseek-v3",
  "gpt-4.1-mini",
  "gpt-4.1-nano",
  "gpt-4o-audio-preview",
  "gpt-4o-mini-search",
  "phi-4-mini",
  "mistral-small-3.1",
  "qwen2.5-coder",
  "llama-4-scout",
  "flux",
  "gpt-image-1",
]
```

# Таблица совместимости функций
| Endpoint  | Статус | Комментарий |
| ------------- | ------------- | ------------- |
| chat.completions  | ✅ Полная поддержка  | Поддержка streaming, file upload, gpt-4o-audio-preview  |
| images.generate  | ✅ Полная поддержка  | Поддержка gpt-image-1, size  |
| images.edit  | ✅ Полная поддержка  | Поддержка gpt-image-1, size  |

# Интеграция в IDE / плагины
На данный момент **тестово поддерживается Cline**, нужно вставить наш url в поле base_url и указать модель из списка

# Ограничения
**Сейчас rate limit = ∞**. Если ситуация изменится, мы заблаговременно предупредим всех участников в нашем телеграм канале: https://t.me/codyapi.

# Безопасность и конфиденциальность
Мы проектировали Cody API по принципу **zero-retention**: содержимое ваших запросов (промпты, ответы, вложенные файлы) **не пишется на диск и удаляется сразу после генерации результата**.
>**Никакого логирования контента.**
>
>**Сервис обрабатывает промпт в оперативной памяти и мгновенно стирает его после того, как LLM вернула ответ.**

# Поддержка и контакты
>**email**: vvirtr@gmail.com
>
>**telegram**: https://t.me/vvirtr
>
>**telegram-channel**: https://t.me/codyapi
