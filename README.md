<a href="https://github.com/lmnr-ai/index">![GitHub stars](https://img.shields.io/github/stars/lmnr-ai/index?style=social)</a>
<a href="https://www.ycombinator.com/companies/laminar-ai">![Static Badge](https://img.shields.io/badge/Y%20Combinator-S24-orange)</a>
<a href="https://x.com/lmnrai">![X (formerly Twitter) Follow](https://img.shields.io/twitter/follow/lmnrai)</a>
<a href="https://discord.gg/nNFUUDAKub"> ![Static Badge](https://img.shields.io/badge/Join_Discord-464646?&logo=discord&logoColor=5865F2) </a>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./static/logo_dark.png">
  <source media="(prefers-color-scheme: light)" srcset="./static/logo_light.png">
  <img alt="Laminar logo" src="./static/logo_light.png">
</picture>

# Index

Index is a state-of-the-art open-source browser agent that autonomously executes complex web tasks. It turns any website into an accessible API and can be seamlessly integrated with just a few lines of code.

- [x] Powered by reasoning LLMs with vision capabilities.
    - [x] Gemini 2.5 Pro (really fast and accurate)
    - [x] Claude 3.7 Sonnet with extended thinking (reliable and accurate)
    - [x] OpenAI o4-mini (depending on the reasoning effort, provides good balance between speed, cost and accuracy)
    - [x] Gemini 2.5 Flash (really fast, cheap, and good for less complex tasks)
- [x] `pip install lmnr-index` and use it in your project
- [x] `index run` to run the agent in the interactive CLI
- [x] Supports structured output via Pydantic schemas for reliable data extraction.
- [x] Index is also available as a [serverless API.](https://docs.lmnr.ai/index-agent/api/getting-started)
- [x] You can also try out Index via [Chat UI](https://lmnr.ai/chat).
- [x] Supports advanced [browser agent observability](https://docs.lmnr.ai/index-agent/tracing) powered by open-source platform [Laminar](https://github.com/lmnr-ai/lmnr).

prompt: go to ycombinator.com. summarize first 3 companies in the W25 batch and make new spreadsheet in google sheets.

https://github.com/user-attachments/assets/2b46ee20-81b6-4188-92fb-4d97fe0b3d6a

## Documentation

Check out full documentation [here](https://docs.lmnr.ai/index-agent/getting-started)

## Quickstart

### Install dependencies
```bash
pip install lmnr-index 'lmnr[all]'

# Install playwright
playwright install chromium
```

### Setup model API keys

Setup your model API keys in `.env` file in your project root:
```
GEMINI_API_KEY=
ANTHROPIC_API_KEY=
OPENAI_API_KEY=
# Optional, to trace the agent's actions and record browser session
LMNR_PROJECT_API_KEY=
```

### Run Index with code
```python
import asyncio
from index import Agent, GeminiProvider
from pydantic import BaseModel
from lmnr import Laminar
import os

# to trace the agent's actions and record browser session
Laminar.initialize()

# Define Pydantic schema for structured output
class NewsSummary(BaseModel):
    title: str
    summary: str

async def main():

    llm = GeminiProvider(model="gemini-2.5-pro-preview-05-06")
    agent = Agent(llm=llm)

    # Example of getting structured output
    output = await agent.run(
        prompt="Navigate to news.ycombinator.com, find a post about AI, extract its title and provide a concise summary.",
        output_model=NewsSummary
    )
    
    summary = NewsSummary.model_validate(output.result.content)
    print(f"Title: {summary.title}")
    print(f"Summary: {summary.summary}")
    
if __name__ == "__main__":
    asyncio.run(main())
```

### Run Index with CLI

Index CLI features:
- Browser state persistence between sessions
- Follow-up messages with support for "give human control" action
- Real-time streaming updates
- Beautiful terminal UI using Textual

You can run Index CLI with the following command.
```bash
index run
```

Output will look like this:

```
Loaded existing browser state
╭───────────────────── Interactive Mode ─────────────────────╮
│ Index Browser Agent Interactive Mode                       │
│ Type your message and press Enter. The agent will respond. │
│ Press Ctrl+C to exit.                                      │
╰────────────────────────────────────────────────────────────╯

Choose an LLM model:
1. Gemini 2.5 Flash
2. Claude 3.7 Sonnet
3. OpenAI o4-mini
Select model [1/2] (1): 3
Using OpenAI model: o4-mini
Loaded existing browser state

Your message: go to lmnr.ai, summarize pricing page

Agent is working...
Step 1: Opening lmnr.ai
Step 2: Opening Pricing page
Step 3: Scrolling for more pricing details
Step 4: Scrolling back up to view pricing tiers
Step 5: Provided concise summary of the three pricing tiers
```

### Running CLI with a personal Chrome instance

You can use Index with personal Chrome browser instance instead of launching a new browser. Main advantage is that all your existing logged-in sessions will be available.

```bash
# Basic usage with default Chrome path
index run --local-chrome
```

## Use Index via API

The easiest way to use Index in production is with [serverless API](https://docs.lmnr.ai/index-agent/api/getting-started). Index API manages remote browser sessions, agent infrastructure and [browser observability](https://docs.lmnr.ai/index-agent/api/tracing). To get started, create a project API key in [Laminar](https://lmnr.ai).

### Install Laminar
```bash
pip install lmnr
```

### Use Index via API
```python
from lmnr import Laminar, LaminarClient
# you can also set LMNR_PROJECT_API_KEY environment variable

# Initialize tracing
Laminar.initialize(project_api_key="your_api_key")

# Initialize the client
client = LaminarClient(project_api_key="your_api_key")

for chunk in client.agent.run(
    stream=True,
    model_provider="gemini",
    model="gemini-2.5-pro-preview-05-06",
    prompt="Navigate to news.ycombinator.com, find a post about AI, and summarize it"
):
    print(chunk)
    
```


## Browser agent observability

Both code run and API run provide advanced browser observability. To trace Index agent's actions and record browser session you simply need to initialize Laminar tracing before running the agent.

```python
from lmnr import Laminar

Laminar.initialize(project_api_key="your_api_key")
```

Then you will get full observability on the agent's actions synced with the browser session in the Laminar platform. Learn more about browser agent observability in the [documentation](https://docs.lmnr.ai/index-agent/tracing).

<picture>
    <img src="./static/traces.png" alt="Index observability" width="800"/>
</picture>

---

Made with ❤️ by the [Laminar team](https://lmnr.ai)

18.03.26 sync forc

Ось результати аналізу та стратегія трансформації для проекту **Index**, підготовлені у форматі для копіювання в Notion.

---

# 📑 Звіт AI-консультанта: Проект "Index"

**Index** — це сучасний браузерний агент із відкритим кодом, який автономно виконує складні веб-завдання, перетворюючи будь-який веб-сайт на доступний API.

---

## 🧬 Частина 1: "ДНК" Проекту

Логіку коду **Index** можна розбити на такі **атомарні функції**:

*   **Навігація та керування браузером:** Використання Playwright для автоматизації дій у Chromium, включаючи запуск нових сесій або використання існуючих профілів Chrome для доступу до авторизованих сесій.
*   **Інтелектуальне планування (Reasoning):** Використання потужних LLM з візуальними можливостями (Gemini, Claude, OpenAI) для аналізу стану сторінки та прийняття рішень про наступні кроки.
*   **Структуроване витягування даних:** Підтримка схем **Pydantic** для перетворення неструктурованого контенту веб-сайтів у чіткі програмні об'єкти.
*   **Керування станом (Persistence):** Можливість збереження стану браузера між різними сесіями для безперервної роботи.
*   **Спостережуваність (Observability):** Трасування дій агента та запис браузерних сесій через платформу Laminar.
*   **Стрімінгова видача:** Реалізація real-time оновлень та потокової передачі результатів виконання завдань.

### 💎 Головна технічна цінність
Головна цінність Index полягає в **автономному перетворенні будь-якого веб-інтерфейсу на структурований API** за допомогою всього кількох рядків коду. Він дозволяє автоматизувати складні багатокрокові завдання (наприклад, роботу з таблицями, пошук та резюмування), які раніше вимагали ручного написання крихких скриптів для парсингу.

---

## 🚀 Частина 2: "Трансформація" (Інтеграція з Gemini LLM)

Хоча Index вже підтримує Gemini, його "Трансформація" полягає у переході від простого виконання команд до **автономного агентного сервісу** на вашому сайті.

### Як зміниться функціонал?
1.  **Мультимодальна корекція:** Використовуючи **Gemini 2.5 Pro**, агент зможе не лише читати текст, а й "бачити" динамічні зміни в інтерфейсі (спливаючі вікна, капчі) та реагувати на них швидше за людину.
2.  **Автономне самонавчання:** ШІ може аналізувати попередні невдалі спроби через систему трасування Laminar та автоматично коригувати промпти для наступних запусків.
3.  **Генерація інструментів на льоту:** Користувач описує задачу природною мовою, а Gemini створює Pydantic-схему спеціально під це завдання.

### Сценарій сервісу "Web-Action-as-a-Service" (Index + Gemini + ваші ID_{$})

Сценарій створення готового сервісу (наприклад, автоматичне дослідження ринку та наповнення бази даних):
1.  **Прийом запиту (ID_{$1}):** Ваш Python-скрипт **ID_{$1}** приймає від користувача тему дослідження (наприклад: "Знайди топ-3 компанії W25 та збережи у базу").
2.  **Оркестрація (Index + Gemini):** Index запускає агент на базі **Gemini 2.5 Pro**. Він заходить на сайти, знаходить інформацію та формує структурований об'єкт.
3.  **Обробка та збереження (ID_{$2}):** Другий скрипт **ID_{$2}** отримує цей об'єкт, валідує його та автоматично заносить у вашу базу даних або створює Google Sheets.
4.  **Моніторинг (Laminar):** Через вбудовану систему спостережуваності ви бачите кожен крок агента у реальному часі прямо на адмін-панелі вашого сайту.

---

## 📋 План дій для Notion
| Крок | Дія | Результат |
| :--- | :--- | :--- |
| **1** | Встановлення: `pip install lmnr-index` | Готове середовище |
| **2** | Налаштування `GEMINI_API_KEY` у `.env` | Доступ до інтелекту |
| **3** | Створення Pydantic-моделі для вашого сервісу | Структуровані дані |
| **4** | Інтеграція з вашими `ID_{$}` скриптами | Готовий сервіс на сайті |

---

### 💡 Резюме

**Суть:** **Автономне виконання веб-завдань через ШІ**.

**AI-Роль:** **Створення інтелектуальних застосунків через Spark**.
