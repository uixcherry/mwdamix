# Forge.MW Plugin Documentation

## Описание

Forge.MW - это комплексный плагин для серверов Unturned, который добавляет систему фракций, кастомный HUD, статистику игроков, систему рангов и множество других функций для создания ролевого игрового процесса.

## Основные возможности

- **Система фракций** с рангами, штабами и госпиталями
- **Кастомный HUD** с отображением здоровья, времени, статуса зон
- **Статистика игроков** и рейтинговая система
- **Система рангов** с автоматическим продвижением
- **NLR (New Life Rule)** с ограничениями после смерти
- **Система объявлений** глобальных и фракционных
- **База данных MySQL** для хранения данных игроков
- **Автоматические награды** за время игры и убийства

---

## Конфигурация

### Основные параметры

```xml
<ConnectionString>server=127.0.0.1;port=3306;uid=root;pwd=12345;database=test</ConnectionString>
<AdminPermissionGroups>
  <Group>admin</Group>
  <Group>moderator</Group>
</AdminPermissionGroups>
```

### Настройка UI эффектов

```xml
<SignupEffectGuid>UI GUID</SignupEffectGuid>
<SignupEffectID>65535</SignupEffectID>
<SignupEffectKey>-32768</SignupEffectKey>

<HUDEffectGuid>UI GUID</HUDEffectGuid>
<HUDEffectID>65535</HUDEffectID>
<HUDEffectKey>-32768</HUDEffectKey>

<StatsEffectGuid>UI GUID</StatsEffectGuid>
<StatsEffectID>65535</StatsEffectID>
<StatsEffectKey>-32768</StatsEffectKey>

<AdvertisementEffectGuid>UI GUID</AdvertisementEffectGuid>
<AdvertisementEffectID>65535</AdvertisementEffectID>
<AdvertisementEffectKey>-32768</AdvertisementEffectKey>
```

### Система наград

```xml
<KillRewardPoints>10</KillRewardPoints>
<TeamKillPenaltyPoints>5</TeamKillPenaltyPoints>
<TimeExperienceInterval>60</TimeExperienceInterval>
<TimeRewardPoints>15</TimeRewardPoints>
<TimeExperienceReward>20</TimeExperienceReward>
<NLRCooldownTime>240</NLRCooldownTime>
```

### Настройка объявлений

```xml
<MaxAdvertisementLength>300</MaxAdvertisementLength>
<DefaultAdvertisementDuration>30</DefaultAdvertisementDuration>
<AdvertisementCooldown>60</AdvertisementCooldown>
```

---

## Система фракций

### Конфигурация фракции

```xml
<Faction Id="1">
  <Name>FactionOne</Name>
  <Description>Описание фракции 1</Description>
  <PermissionGroups>
    <Group>default1</Group>
    <Group>default2</Group>
  </PermissionGroups>
  <SteamGroup>0</SteamGroup>
  <JoinSteamGroup>true</JoinSteamGroup>
  <FactionLocation>
    <x>0</x>
    <y>0</y>
    <z>0</z>
  </FactionLocation>
  <HospitalLocation>
    <x>0</x>
    <y>0</y>
    <z>0</z>
  </HospitalLocation>
  <HospitalRadius>50</HospitalRadius>
  <MaxMembers>50</MaxMembers>
  <Ranks>
    <Rank Order="1">
      <PermissionGroup>group1</PermissionGroup>
      <RequiredPoints>100</RequiredPoints>
      <ExperienceSalary>50</ExperienceSalary>
    </Rank>
  </Ranks>
  <FactionVehicles>
    <VehicleId>1</VehicleId>
    <VehicleId>5</VehicleId>
  </FactionVehicles>
</Faction>
```

### Параметры фракции

- **Id** - Уникальный идентификатор фракции
- **Name** - Название фракции
- **Description** - Описание фракции
- **PermissionGroups** - Группы разрешений, которые получают все члены фракции
- **SteamGroup** - Steam группа фракции (создается автоматически)
- **JoinSteamGroup** - Автоматически добавлять игроков в Steam группу
- **FactionLocation** - Координаты штаба фракции
- **HospitalLocation** - Координаты госпиталя фракции
- **HospitalRadius** - Радиус госпиталя для NLR
- **MaxMembers** - Максимальное количество участников
- **FactionVehicles** - ID транспорта фракции

### Система рангов

- **Order** - Порядковый номер ранга (1 = низший)
- **PermissionGroup** - Группа разрешений для данного ранга
- **RequiredPoints** - Количество очков опыта для получения ранга
- **ExperienceSalary** - Зарплата в Unturned опыте за время игры

---

## Команды

### Команды для игроков

#### `/stats` (Алиасы: `stat`, `rating`)
- **Разрешение:** `forge.stats.view`
- **Описание:** Открывает UI статистики с личными показателями и топом игроков

#### `/hud` (Алиасы: `interface`, `ui`)
- **Разрешение:** `forge.hud`
- **Описание:** Включает/выключает кастомный HUD

#### `/faction accept`
- **Описание:** Принять приглашение во фракцию

#### `/faction deny`
- **Описание:** Отклонить приглашение во фракцию

### Команды объявлений

#### `/ad global <сообщение> [длительность]` (Алиасы: `advertisement`, `news`)
- **Разрешение:** `forge.advertisement.global`
- **Описание:** Создать глобальное объявление для всех игроков
- **Параметры:**
  - `сообщение` - Текст объявления (до 300 символов)
  - `длительность` - Время показа в секундах (5-300, по умолчанию 30)

#### `/ad faction <сообщение> [длительность]`
- **Разрешение:** `forge.advertisement.faction`
- **Описание:** Создать объявление для членов своей фракции
- **Ограничения:** Только лидер или заместитель фракции

#### `/ad cancel`
- **Разрешение:** `forge.advertisement.cancel`
- **Описание:** Отменить текущее объявление

### Административные команды фракций

#### `/faction sethq <factionId>`
- **Разрешение:** `forge.faction.sethq`
- **Описание:** Установить штаб фракции в текущем местоположении

#### `/faction sethospital <factionId> <radius>`
- **Разрешение:** `forge.faction.sethospital`
- **Описание:** Установить госпиталь фракции с указанным радиусом

#### `/faction transfer <playerName> <factionId> [force]`
- **Разрешение:** `forge.faction.transfer`
- **Описание:** Перевести игрока в указанную фракцию
- **Параметры:**
  - `force` - Игнорировать лимит участников фракции

#### `/faction fire <playerName>`
- **Разрешение:** `forge.faction.fire`
- **Описание:** Уволить игрока из фракции

#### `/faction setleader <playerName> <factionId>`
- **Разрешение:** `forge.faction.setleader`
- **Описание:** Назначить игрока лидером фракции

#### `/faction setdeputy <playerName> <factionId>`
- **Разрешение:** `forge.faction.setdeputy`
- **Описание:** Назначить игрока заместителем фракции

#### `/faction promote <playerName>`
- **Разрешение:** `forge.faction.promote`
- **Описание:** Повысить игрока в звании

#### `/faction demote <playerName>`
- **Разрешение:** `forge.faction.demote`
- **Описание:** Понизить игрока в звании

#### `/faction addexp <playerName> <amount>`
- **Разрешение:** `forge.faction.addexp`
- **Описание:** Добавить очки опыта игроку

#### `/faction removeexp <playerName> <amount>`
- **Разрешение:** `forge.faction.removeexp`
- **Описание:** Удалить очки опыта у игрока

### Команды лидеров фракций

#### `/faction invite <playerName>`
- **Разрешение:** `forge.faction.leader`
- **Описание:** Пригласить игрока во фракцию
- **Ограничения:** Только лидер фракции, приглашение действует 3 минуты

---

## Система разрешений

### Основные группы разрешений

- **forge.advertisement.global** - Глобальные объявления
- **forge.advertisement.faction** - Фракционные объявления
- **forge.advertisement.admin** - Все права на объявления
- **forge.advertisement.cancel** - Отмена объявлений
- **forge.faction.admin** - Все права на фракции
- **forge.faction.leader** - Права лидера фракции
- **forge.faction.sethq** - Установка штаба
- **forge.faction.sethospital** - Установка госпиталя
- **forge.faction.transfer** - Перевод игроков
- **forge.faction.fire** - Увольнение игроков
- **forge.faction.setleader** - Назначение лидера
- **forge.faction.setdeputy** - Назначение заместителя
- **forge.faction.promote** - Повышение в звании
- **forge.faction.demote** - Понижение в звании
- **forge.faction.addexp** - Добавление опыта
- **forge.faction.removeexp** - Удаление опыта
- **forge.hud** - Управление HUD
- **forge.stats.view** - Просмотр статистики

---

## База данных

### Требования

- **MySQL 8.0+** рекомендуется
- Автоматическое создание таблиц при первом запуске
- Поддержка повторного подключения при потере соединения

### Структура таблицы `player_data`

| Поле | Тип | Описание |
|------|-----|----------|
| SteamId64 | BIGINT | Steam ID игрока (PRIMARY KEY) |
| Nickname | VARCHAR(64) | Никнейм игрока |
| FactionId | TINYINT | ID фракции (0 = без фракции) |
| Level | TINYINT | Уровень ранга |
| ExperiencePoints | INT UNSIGNED | Очки опыта |
| Kills | INT UNSIGNED | Количество убийств |
| Deaths | INT UNSIGNED | Количество смертей |
| DestroyedVehicles | INT UNSIGNED | Уничтоженные транспортные средства |
| RegistrationDate | DATETIME | Дата регистрации |
| LastUpdated | TIMESTAMP | Последнее обновление |

---

## Система NLR (New Life Rule)

### Принцип работы

- Активируется автоматически при смерти игрока
- Длительность настраивается в конфигурации (`NLRCooldownTime`)
- Игрок возрождается в госпитале своей фракции
- Запрещен вход в транспорт во время действия NLR
- При выходе из радиуса госпиталя игрок телепортируется обратно

### Настройка госпиталей

1. Используйте команду `/faction sethospital <factionId> <radius>`
2. Убедитесь, что координаты госпиталя не равны (0,0,0)
3. Установите подходящий радиус для вашей карты

---

## Кастомный HUD

### Отображаемая информация

- **Здоровье игрока** с прогресс-баром
- **Выносливость, голод, жажда, вирус, кислород**
- **Статус кровотечения и переломов**
- **Информация о времени и дате**
- **Статусы зон** (SafeZone/DeadZone)
- **Информация о звании и прогрессе**
- **Данные о транспорте** (скорость, топливо, здоровье)
- **Информация об оружии** (патроны, режим стрельбы)
- **Статус NLR** с таймером

### Зоны

- Автоматическое определение SafeZone и DeadZone
- Буферная зона для предотвращения мерцания индикаторов
- Обновление каждые 0.5 секунд для оптимизации производительности

---

## Система наград

### Награды за убийства

- **KillRewardPoints** - Очки за убийство врага
- **TeamKillPenaltyPoints** - Штраф за убийство союзника
- Проверка принадлежности к фракции для определения союзников

### Награды за время

- **TimeExperienceInterval** - Интервал начисления очков опыта (секунды)
- **TimeRewardPoints** - Количество очков опыта за интервал
- **TimeExperienceReward** - Интервал начисления Unturned опыта (секунды)
- **ExperienceSalary** - Размер зарплаты в Unturned опыте (настраивается для каждого ранга)

### Награды за уничтожение транспорта

Настраивается в секции `VehicleRewards`:

```xml
<VehicleReward Id="1">
  <PointReward>15</PointReward>
  <ExperienceReward>30</ExperienceReward>
</VehicleReward>
```

- **Id** - ID транспортного средства
- **PointReward** - Очки опыта за уничтожение
- **ExperienceReward** - Unturned опыт за уничтожение

---

## Особенности и ограничения

### Система рангов

- Автоматическое продвижение до предпоследнего ранга
- Ранги "Лидер" и "Заместитель" назначаются только администрацией
- Автоматическое назначение групп разрешений при смене ранга

### Контроль квестов

- Harmony патчи ограничивают создание/удаление/выход из групп квестов
- Доступно только администраторам и игрокам из `AdminPermissionGroups`

### Производительность

- Оптимизированные таймеры обновления для HUD элементов
- Кэширование топа игроков (обновление каждые 5 минут)
- Автоматическая очистка неиспользуемых таймеров

### Безопасность

- Проверка всех пользовательских данных
- Защита от SQL инъекций через параметризованные запросы
- Обработка потери соединения с базой данных

---

## Требования к серверу

- **RocketMod Framework**
- **MySQL 8.0+** (рекомендуется)
- **HarmonyLib** для патчей
- **UI эффекты** для всех интерфейсов (создаются отдельно)

---

## Поддержка

При возникновении проблем проверьте:

1. **Подключение к базе данных** - правильность строки подключения
2. **Разрешения игроков** - корректность настройки групп
3. **UI эффекты** - наличие всех необходимых GUID'ов
4. **Логи сервера** - плагин подробно логирует все действия и ошибки
