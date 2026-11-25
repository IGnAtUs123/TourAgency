# Туристическое агентство

Информационная система для подбора, бронирования и управления турами,
а также взаимодействия между клиентами и туристическим агентством.

## Назначение

Система предназначена для клиентов (просмотр каталога туров, оформление
бронирования, управление своими заявками) и для сотрудников агентства
(создание и редактирование туров, обработка заявок на бронирование).
Результат использования — сокращение времени обработки обращений и
прозрачный контроль статусов бронирования.

## Команда

| Участник       | Роли                                                                                    |
| -------------- | --------------------------------------------------------------------------------------- |
| Холодняк Игнат | Руководитель проекта, Разработчик, Архитектор, Специалист по развёртыванию, Тестировщик |

## Трекер задач

Jira: `https://elobaka-tours.atlassian.net/jira/software/projects/TA/boards/3?filter=&groupBy=none&atlOrigin=eyJpIjoiYTgyNjg2YzQyYzExNDc0OGEzZmVlZjdmMjZjZTZjYzQiLCJwIjoiaiJ9`

## Технологический стек

- Frontend: React.js
- Backend: Express.js (Node.js)
- СУБД: PostgreSQL
- Контейнеризация: Docker, Docker Compose
- Репозиторий: GitHub
- IDE: Visual Studio Code

## Структура проекта

```
travel-agency-system/
├── backend/            # серверная часть (REST API)
├── frontend/           # клиентская часть (React)
├── docs/               # проектная документация и диаграммы
│   ├── bpmn/            # диаграммы процессов As-Is / To-Be
│   ├── requirements/    # функциональные и нефункциональные требования
│   ├── architecture/    # контекстная диаграмма и диаграмма компонентов (C4)
│   ├── data/            # ER-диаграмма базы данных
│   └── adr/             # журнал архитектурных решений (ADR)
├── docker-compose.yml
├── .gitignore
└── README.md
```

## Архитектура системы (кратко)

Система построена как **монолитное** Express.js-приложение с модульной
внутренней структурой и единой базой данных PostgreSQL (обоснование — см.
`docs/adr/001_architecture_style.md`). Клиентская часть — одностраничное
приложение на React.js, взаимодействующее с backend через REST API по HTTPS.
Внешние интеграции: сервис email-уведомлений и файловое хранилище изображений
туров (см. контекстную диаграмму).

Подробности архитектуры и бизнес-процессов — в документации ниже.

## Документация проекта (docs/)

| Артефакт                             | Файл                                                                                 |
| ------------------------------------ | ------------------------------------------------------------------------------------ |
| Диаграмма текущего процесса (As-Is)  | [`docs/bpmn/as_is.png`](docs/bpmn/as_is.png)                                         |
| Диаграмма целевого процесса (To-Be)  | [`docs/bpmn/to_be.png`](docs/bpmn/to_be.png)                                         |
| Функциональные требования            | [`docs/requirements/functional.md`](docs/requirements/functional.md)                 |
| Нефункциональные требования          | [`docs/requirements/non_functional.md`](docs/requirements/non_functional.md)         |
| Контекстная диаграмма (C4, Level 1)  | [`docs/architecture/context_diagram.png`](docs/architecture/context_diagram.png)     |
| Диаграмма компонентов (C4, Level 2)  | [`docs/architecture/container_diagram.png`](docs/architecture/container_diagram.png) |
| ER-диаграмма базы данных             | [`docs/data/er_diagram.png`](docs/data/er_diagram.png)                               |
| Журнал архитектурных решений (ADR)   | [`docs/adr/001_architecture_style.md`](docs/adr/001_architecture_style.md)           |
| Отчёт о практической работе №2 (PDF) | `docs/report/Отчет_о_практической_работе_2.pdf`                                      |

## Запуск и тестирование

### Запуск backend

    cd backend
    npm install
    npm start

Сервер доступен на http://localhost:3000.

### Запуск frontend

    cd frontend
    npm install
    npm run dev

### Тест-кейсы health-check

| №   | Действие                               | Ожидаемый результат                         |
| --- | -------------------------------------- | ------------------------------------------- |
| 1   | GET http://localhost:3000/api/health   | HTTP 200, в теле JSON с полем status = "ok" |
| 2   | GET http://localhost:3000/api/unknown  | HTTP 404                                    |
| 3   | Остановить backend, повторить запрос 1 | Соединение отклонено, ошибка подключения    |

Проверка выполняется в Postman или командой `curl`.
