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

- Tendermint (можем легко наладить связь с другими DAO, Rust)

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

--- 

## Системы поощрения
- при регистрации даётся утили токен на тренировку
- можно "привести друга" и тогда тебе тоже дадут токен
- повышение уровня в зависимости от посещений
- глобальный рейтинг
- награды самым активным

## Примеры контрактов

Вознаграждаем пригласившего:
```Rust
/// Contract for rewarding person who invited somebody
pub fn reward_invite(
    deps: DepsMut,
    info: MessageInfo,
    invitee: String,
) -> StdResult<Response> {
    // Check if registration is complete
    let registered_users = REGISTERED_USERS.load(deps.storage)?;
    if !registered_users.contains(&invitee) {
        return Err(StdError::generic_err("Invitee not registered"));
    }

    let user_address = info.sender.to_string();
    let mut user_info = USERS.may_load(deps.storage, user_address.clone())?.unwrap_or(TokenInfo {
        governance_tokens: Uint128::zero(),
        utility_tokens: Uint128::zero(),
    });

    user_info.utility_tokens += Uint128::from(1u128); // Get tokens for invitation
    USERS.save(deps.storage, user_address, &user_info)?;

    Ok(Response::new().add_attribute("action", "reward_invite"))
}
```

Добавляем админа. Логика такая, что после голосования будет вызвана эта функция (только в случае успеха голосования)

```Rust
pub fn add_administrator(
    deps: DepsMut,
    info: MessageInfo,
    new_admin: String,
) -> StdResult<Response> {
    let sender_address = info.sender.to_string();

    // Check if sender is administrator
    let mut administrators = ADMINISTRATORS.load(deps.storage)?;
    if !administrators.contains(&sender_address) {
        return Err(StdError::generic_err("Only administrators can add new administrators"));
    }

    // Check if admin is Professional of Coach
    let role = ROLES.load(deps.storage, new_admin.clone())?;
    if role != Role::Professional && role != Role::Coach {
        return Err(StdError::generic_err("New administrator must be a Professional or a Coach"));
    }

    if !administrators.contains(&new_admin) {
        administrators.push(new_admin.clone());
        ADMINISTRATORS.save(deps.storage, &administrators)?;
    }

    Ok(Response::new()
        .add_attribute("action", "add_administrator")
        .add_attribute("new_admin", new_admin))
}
```
