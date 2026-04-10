# HH Outreach

<p align="center">
  <img src="docs/hero-banner.png" alt="HH Outreach — AI-powered outbound lead farming via HH.ru" width="900">
</p>

<p align="center">
  <em>В порыве ночного безумия я собрал скилл для Claude Code, который за меня фармит лидов на hh.</em><br>
  <strong>Outbound sales engine, замаскированный под job search.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Claude_Code-000?style=flat&logo=anthropic&logoColor=white" alt="Claude Code">
  <img src="https://img.shields.io/badge/Playwright-2EAD33?style=flat&logo=playwright&logoColor=white" alt="Playwright">
  <img src="https://img.shields.io/badge/Node.js-339933?style=flat&logo=node.js&logoColor=white" alt="Node.js">
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/HH.ru-D6001C?style=flat&logo=headhunter&logoColor=white" alt="HH.ru">
</p>

---

## Что это такое

Это скилл для Claude Code, который каждый день ходит в API hh.ru, собирает свежие вакансии, генерит на каждую персональный пассивно-агрессивный отклик с моим Calendly внутри и отправляет через Playwright. Полностью автономно. Пока я сплю.

Я использую его не для поиска работы. Я использую его как фарминг лидов на AI-автоматизацию бизнеса. Каждый HR, который разместил вакансию на 300к, - это человек с подтверждённым бюджетом и конкретной болью. В сопроводительном я продаю ему парт-тайм консалтинг: «не нанимайте штатника, возьмите меня, я закрою это машиной за меньшие деньги».

## Результаты за 8 дней работы

- **472** отправленных отклика
- **37** назначенных собеседований
- **14** звонков с рекрутерами и фаундерами
- **3** закрытых продажи

Это не лучший response rate на свете, но это лучший SDR, которого я когда-либо нанимал. И он работает 24/7, без зарплаты, без выгорания и без понедельничной прокрастинации.

## Как работает

```
HH API (поиск за 24ч)
    ↓
Фильтр + дедупликация против sent.json
    ↓
Релевантная выборка топ-100
    ↓
Для каждой вакансии:
    ├─ Генерация персонального текста (компания, должность, зп внутри)
    ├─ Playwright: navigate → click Откликнуться → cover letter
    ├─ nativeInputValueSetter для React textarea
    ├─ Submit + verify «Резюме доставлено»
    └─ Log в sent.json с message_preview
    ↓
Throttle 5-10 секунд
    ↓
Повтор до лимита (обычно ~200/день, лимит HH)
```

## Установка и запуск

```bash
# Клонировать как скилл в Claude Code
git clone https://github.com/TimmyZinin/hh-outreach.git ~/.claude/skills/hh-outreach

# Поставить Playwright-core для batch-скрипта
mkdir -p ~/hh-outreach-data
cd ~/hh-outreach-data
npm init -y
npm install playwright-core

# Запустить интерактивно через Claude Code
/hh-outreach 20                              # отправить 20 откликов
/hh-outreach 20 "контент-стратег"            # с кастомным запросом
/hh-outreach 20 --dry-run                    # поиск и генерация без отправки
/hh-outreach 20 --preview                    # показать текст перед каждой отправкой
/hh-outreach 20 --min-salary 80000           # только вакансии от 80К
/hh-outreach 20 --period 3                   # за последние 3 дня
```

## Шаблон сопроводительного (Bold 2026)

Скилл генерит персонализированный текст под каждую вакансию. Вот пример, что уходит живым компаниям:

```
Коллеги из {company}, слушайте.

Я даже календарь посмотрел на всякий случай - и да, на дворе 2026 год,
а вы мне тут платите {salary} за работу человека на позиции {title}.
Давайте я вам помогу этой ерундой прекратить заниматься?

Вместо того чтобы кормить 10 таких ребят, платите мне - я AI-инженер
по процессам. Я вам соберу систему из Claude, GPT и автоматизаций,
которая закроет вашу вакансию за меня. В частности, {title} у вас
закроется моими руками за пару дней.

Вы уже поняли, что вам пишет мой агент, да? Я так и работаю. Я плачу
моим агентам копейки, а они мне делают работу целого отдела.

Вот мой сайт: timzinin.com
Вот мой календарь: https://calendly.com/timzinin

С любовью,
Тим Зинин
```

## Требования

- Chrome с включённым remote debugging (`chrome://inspect`)
- Залогиненный аккаунт на HH.ru
- Claude Code с chrome-cdp скиллом (для интерактивного режима)
- Node.js 20+ и Playwright-core (для batch-режима)
- Python 3.10+ (для поиска вакансий через HH API)

## Документация

- [SPEC.md](SPEC.md) - полная спецификация скилла
- [Architecture.md](Architecture.md) - архитектура и диаграммы
- [BACKLOG.md](BACKLOG.md) - бэклог улучшений

## Данные

Всё логируется в `~/hh-outreach-data/`:
- `sent.json` - все отправленные отклики с превью сообщений
- `vacancies-batch-YYYY-MM-DD.json` - выборки по датам
- `batch-log-YYYY-MM-DD.md` - дневные логи
- `batch-send.mjs` - standalone Playwright скрипт для batch-рассылок

## Предупреждение

Это инструмент для осознанного outbound-маркетинга. Не используйте его для спама. HH.ru видит твою активность и может забанить аккаунт при слишком агрессивной отправке. Рекомендую:
- Throttle минимум 5 секунд между откликами
- Лимит 200 откликов/день (естественный HH-лимит)
- Персонализация обязательна - generic тексты не работают и бесят людей
- Календарь внутри сопроводительного = конверсия в звонки

## Автор

**Тим Зинин** - AI-инженер по процессам. Строю AI-системы на Claude Code, GPT, n8n и Python, которые закрывают работу целых отделов. Парт-тайм, удалёнка, ИП.

- Сайт: [timzinin.com](https://timzinin.com)
- Календарь: [calendly.com/timzinin](https://calendly.com/timzinin)
- GitHub: [@TimmyZinin](https://github.com/TimmyZinin)

## Лицензия

MIT. Забирай, адаптируй, используй. Если этот скилл помог тебе закрыть продажу - напиши мне, будет приятно.
