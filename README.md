# HH Outreach

Скилл для Claude Code. Автоматизирует отправку откликов на вакансии HH.ru с персонализированными сопроводительными письмами для лидогенерации.

## Использование

```bash
/hh-outreach 20                              # отправить 20 откликов
/hh-outreach 20 "контент-стратег"            # с кастомным запросом
/hh-outreach 20 --dry-run                    # только поиск и генерация, без отправки
/hh-outreach 20 --preview                    # показать каждый текст перед отправкой
/hh-outreach 20 --min-salary 80000           # только вакансии от 80К
/hh-outreach 20 --period 3                   # за последние 3 дня
```

## Требования

- Chrome с включённым remote debugging (`chrome://inspect`)
- Залогиненный аккаунт на HH.ru в Chrome
- Claude Code с chrome-cdp скиллом

## Документация

- [SPEC.md](SPEC.md) - полная спецификация
- [Architecture.md](Architecture.md) - архитектура и диаграммы

## Данные

Логи и история хранятся в `~/hh-outreach-data/`:
- `sent.json` - все отправленные отклики
- `log-YYYY-MM-DD.md` - дневные логи
