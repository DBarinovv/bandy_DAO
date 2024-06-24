# BANDY DAO

## Описание
Хоккей с мячом, также известный как хоккей с мячом или русский хоккей с мячом - это вид спорта с богатой историей и уникальным игровым процессом. Несмотря на свою культурную значимость и давние традиции, в последние годы хоккей с мячом совсем перестал популяризироваться (как по мне - начинания с зимней олимпиады в России, где мы, как страна организатор, добавили вид спорта, но это был не "Русский хоккей"). Как бывший профессиональный игрок с почти десятилетним опытом, я страстно желаю возродить и популяризировать хоккей с мячом

### Цели и задачи
- Поддержка местных и международных турниров
- Обучение новых игроков
- Развитие инфраструктуры для занятий bandy
- Привлечение внимания к спорту

---

## Архитектура

### Выбор блокчейна

- Tendermint (можем легко наладить связь с другими DAO)

### Frontend and backend

**React** + **Node.js** – очень популярно, легко писать, а также обеспечивает высокую производительность и асинхронную модель обработки запросов

### Виды токенов

- **Governance Tokens** - получают пропорционально активности
- **Utility Tokens** - доступ к инфраструктуре, поощрительные токены

---

## Роли участников
- Новичок - получает доступ к базовой инфрастуктуре и голосованию за тренеров
- Любитель - получает полный доступ к голосованиям
- Профессионал - получает возможность создавать голосование
- Тренер - по сути профессионал, но другого вида
- Администратор - занимается закупкой, бронь и т.д. то есть в том же числе получает доступ к `createICO`

---

## Схема управления
### Методы
- `createProposal` (вручить экипировку активному участнику, повысить чей-то статус и т.д.)
- `vote` (голосовать могут только те, которые контактировали)
- `addAdministrator` (функция, которая вызывается если голосвание на повышение было успешно)
- `removeAdministrator` (такая же логика как выше. Удалить администратора только можно явным большинством, например 70%)
- `createICO` (создаём сбор средств на что-то. ранзные суммы разные роли могут)
