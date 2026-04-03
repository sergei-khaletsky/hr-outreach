# HH Outreach - Бэклог и спринты

## Бэклог

| ID | Задача | Приоритет | Спринт | Статус |
|----|--------|-----------|--------|--------|
| HH-01 | Создать SKILL.md с полной спецификацией | crit | 1 | backlog |
| HH-02 | Создать ~/hh-outreach-data/ и sent.json с 64 записями | crit | 1 | backlog |
| HH-03 | CDP healthcheck: функция проверки доступности | high | 1 | backlog |
| HH-04 | SEARCH: поиск вакансий через CDP eval с фильтрацией | crit | 1 | backlog |
| HH-05 | SEARCH: дедупликация по sent.json | high | 1 | backlog |
| HH-06 | SEARCH: пагинация (страницы 0-N) | med | 1 | backlog |
| HH-07 | READ: извлечение описания вакансии через CDP eval | crit | 1 | backlog |
| HH-08 | READ: парсинг зарплаты (от/до, gross/net, $/руб) | high | 1 | backlog |
| HH-09 | READ: проверка "Вы откликнулись" | high | 1 | backlog |
| HH-10 | Тест Спринт 1: dry-run на 5 вакансий | crit | 1 | backlog |
| HH-11 | GENERATE: модульный шаблон (6 блоков) | crit | 2 | backlog |
| HH-12 | GENERATE: 5 вариантов OPENING | high | 2 | backlog |
| HH-13 | GENERATE: 7 модулей WHAT_I_DO (по типу вакансии) | high | 2 | backlog |
| HH-14 | GENERATE: нормализация зарплаты для блока OFFER | high | 2 | backlog |
| HH-15 | GENERATE: anti-AI проверка текста | med | 2 | backlog |
| HH-16 | Тест Спринт 2: генерация 10 текстов, ручная проверка качества | crit | 2 | backlog |
| HH-17 | SEND: DOM-based клики (eval по textContent, НЕ clickxy) | crit | 3 | backlog |
| HH-18 | SEND: обработка popup "вакансия в другой стране" | high | 3 | backlog |
| HH-19 | SEND: polling textarea (500ms, timeout 5s) | high | 3 | backlog |
| HH-20 | SEND: polling "Резюме доставлено" (500ms, timeout 5s) | high | 3 | backlog |
| HH-21 | SEND: обработка обязательного сопроводительного | med | 3 | backlog |
| HH-22 | SEND: обработка выбора резюме (если несколько) | med | 3 | backlog |
| HH-23 | Тест Спринт 3: отправка 3 откликов в preview режиме | crit | 3 | backlog |
| HH-24 | LOG: atomic write sent.json (tmp + rename + backup) | crit | 4 | backlog |
| HH-25 | LOG: reason codes в sent.json и дневном логе | high | 4 | backlog |
| HH-26 | LOG: дневной лог log-YYYY-MM-DD.md | high | 4 | backlog |
| HH-27 | THROTTLE: пауза 5-10 сек (random) между откликами | crit | 4 | backlog |
| HH-28 | THROTTLE: лимит 30 откликов за сессию | high | 4 | backlog |
| HH-29 | THROTTLE: предупреждение при >150 в sent.json | med | 4 | backlog |
| HH-30 | REPORT: итоговый отчёт (sent/skip/failed + топ-3) | high | 4 | backlog |
| HH-31 | REPORT: обновление stats в sent.json | med | 4 | backlog |
| HH-32 | Тест Спринт 4: полный цикл 10 откликов | crit | 4 | backlog |
| HH-33 | ERROR: graceful stop при CDP unavailable | high | 5 | backlog |
| HH-34 | ERROR: частичный отчёт при crash | high | 5 | backlog |
| HH-35 | ERROR: recovery после partial write sent.json | med | 5 | backlog |
| HH-36 | ARGS: парсинг --query, --period, --min-salary, --any-format | high | 5 | backlog |
| HH-37 | ARGS: парсинг --dry-run, --preview, --no-filter | high | 5 | backlog |
| HH-38 | Wiki: обновить с результатами тестирования | med | 5 | backlog |
| HH-39 | Codex review финальный (target: 10/10) | crit | 5 | backlog |
| HH-40 | Тест Спринт 5: полный цикл 20 откликов автономно | crit | 5 | backlog |

## Спринт 1: Фундамент (поиск + чтение)

**Цель:** скилл умеет найти вакансии и прочитать их описание через CDP

**Deliverables:**
- SKILL.md создан и зарегистрирован
- sent.json инициализирован с историей 64 откликов
- CDP healthcheck работает
- Поиск вакансий с фильтрацией и дедупликацией
- Чтение описания вакансии с парсингом зарплаты
- dry-run тест на 5 вакансий

**Задачи:** HH-01 .. HH-10

**Критерий готовности:** `/hh-outreach 5 --dry-run` находит 5 вакансий, читает описания, показывает данные (ID, компания, должность, зарплата, описание 200 символов). Не отправляет ничего.

## Спринт 2: Генерация текста

**Цель:** скилл генерит персонализированные сопроводительные письма

**Deliverables:**
- Модульный шаблон (6 блоков)
- 5 вариантов OPENING
- 7 модулей WHAT_I_DO
- Нормализация зарплаты
- Anti-AI проверка
- 10 тестовых текстов проверены вручную

**Задачи:** HH-11 .. HH-16

**Критерий готовности:** `/hh-outreach 10 --dry-run` генерит 10 кастомных текстов. Тим проверяет качество - все 10 должны быть уникальными и персонализированными. Ни один не должен выглядеть как шаблон.

## Спринт 3: Отправка

**Цель:** скилл умеет отправлять отклики через CDP

**Deliverables:**
- DOM-based клики (НЕ clickxy)
- Обработка всех popup/модалок HH
- Polling вместо фиксированных задержек
- Preview режим работает
- 3 тестовых отклика отправлены

**Задачи:** HH-17 .. HH-23

**Критерий готовности:** `/hh-outreach 3 --preview` показывает 3 текста, после одобрения отправляет. Все 3 получают статус SENT. Ни одного clickxy в коде.

## Спринт 4: Логирование и безопасность

**Цель:** надёжное хранение данных, throttling, отчёты

**Deliverables:**
- Atomic write sent.json
- Reason codes
- Дневные логи
- Throttling 5-10 сек
- Лимит 30 за сессию
- Итоговый отчёт
- 10 тестовых откликов в полном цикле

**Задачи:** HH-24 .. HH-32

**Критерий готовности:** `/hh-outreach 10` отправляет 10 откликов с паузами, логирует в sent.json (atomic) и дневной лог. Отчёт показывает sent/skip/failed с reason codes. sent.json не корраптится при прерывании.

## Спринт 5: Hardening и финализация

**Цель:** обработка ошибок, аргументы CLI, финальный Codex review

**Deliverables:**
- Graceful stop при CDP ошибках
- Частичный отчёт при crash
- Recovery после partial write
- Все CLI аргументы работают
- Wiki обновлена
- Codex review 10/10
- 20 тестовых откликов автономно

**Задачи:** HH-33 .. HH-40

**Критерий готовности:** `/hh-outreach 20` работает полностью автономно. Codex review >= 9/10. Wiki актуальна. Все edge cases обработаны.

## Timeline

| Спринт | Задач | Оценка |
|--------|-------|--------|
| 1: Фундамент | 10 | ~2 часа |
| 2: Генерация | 6 | ~1.5 часа |
| 3: Отправка | 7 | ~2 часа |
| 4: Логирование | 9 | ~1.5 часа |
| 5: Hardening | 8 | ~2 часа |
| **Итого** | **40** | **~9 часов** |
