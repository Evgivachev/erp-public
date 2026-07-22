# Справочник MCP-инструментов revit-lighting

Связка: Claude → MCP-сервер `RevitLightingMcpServer` (net8, stdio) → gRPC
`localhost:{порт}` → плагин RevitMcp внутри Revit (net48).

Порт настраивать не нужно: плагин при старте Revit занимает первый свободный
порт из диапазона **5005–5010** и пишет discovery-файл
`%LocalAppData%\ElectricityRevitPlugin\instances\{pid}.json`, по которому
MCP-сервер сам находит открытые экземпляры Revit. По умолчанию запросы идут в
самый свежий запущенный Revit; переключение — `ListRevitInstances` /
`SelectRevitInstance` (без перезапуска чего-либо).

## Светильники (LightingTools)

| Инструмент | Описание |
|---|---|
| `GetAllFixtures` | Все светильники проекта с координатами и электрическими параметрами |
| `GetFixtureById` | Светильник по ElementId |
| `GetSelectedFixtures` | Светильники, выделенные в UI Revit |
| `GetFixturesByLevel` | Светильники на уровне |
| `GetFixturesByRoom` | Светильники в помещении |
| `GetAvailableLightingFamilyTypes` | Доступные типоразмеры семейств светильников |
| `CreateFixture` | Создать светильник (семейство, тип, точка, уровень) |
| `CreateFixturesBatch` | Пакетное создание по typeId и массиву точек |
| `MoveFixture` / `MoveFixturesBatch` | Перемещение (точечное / пакетное) |
| `DeleteFixture` | Удалить светильник |
| `CalculateFixtureLayout` | Расчёт равномерной расстановки в полигоне с вырезами (CVT / Lloyd relaxation) |

## Выключатели (LightingTools)

| Инструмент | Описание |
|---|---|
| `GetAllSwitches` | Все выключатели с управляемыми светильниками |
| `GetSwitchById` | Выключатель по ElementId |
| `GetAvailableSwitchFamilyTypes` | Доступные типоразмеры семейств выключателей |
| `CreateSwitch` | Создать выключатель (с опциональной привязкой к стене) |
| `SetSwitchControlledFixtures` | Связать выключатель со светильниками (many-to-many) |
| `DeleteSwitch` | Удалить выключатель |

## Пространства MEP (SpaceTools)

| Инструмент | Описание |
|---|---|
| `FindSpaceByNumber` | Поиск пространства по номеру |
| `FindSpaceByName` | Поиск по имени (точное/частичное совпадение) |
| `GetAllSpaces` | Все пространства (с фильтром по уровню), площади и объёмы |
| `GetSpaceBoundary` | Контур границ пространства (полигон + bounding box) |

## Архитектура (ArchitectureTools)

| Инструмент | Описание |
|---|---|
| `GetLevels` / `CreateLevel` | Уровни проекта |
| `GetCeilingInfo` | Потолок по пространству или точке (в т.ч. в связанных АР-моделях). Для высоты установки светильников бери **`bottomElevationFeet`** — отметку нижней грани из геометрии в координатах хоста; `offsetFromLevelFeet` для связанных моделей — в системе уровней АР-файла, по нему высоту не считать |
| `GetLinkedFiles` | Связанные Revit-файлы |
| `CreateWalls` / `GetAvailableWallTypes` | Создание стен (тестовая инфраструктура) |
| `CreateSpace` / `SetSpaceParameters` | Создание/редактирование пространств |
| `CreateCeiling` | Создание потолка (тестовая инфраструктура) |
| `DeleteElements` | Пакетное удаление элементов |

## Элементы и параметры (ElementTools)

| Инструмент | Описание |
|---|---|
| `GetElementInfo` / `GetElementInfoBatch` | Информация об элементе(ах) |
| `GetElementParameters` | Все параметры элемента (только чтение) |
| `FindElementParameters` | Поиск параметров по имени |

## Выделение (SelectionTools)

| Инструмент | Описание |
|---|---|
| `GetCurrentSelection` / `SetSelection` / `ClearSelection` | Управление выделением в UI |
| `GetElementsOnCurrentView` | Элементы на активном виде |

## Визуальный контроль (ViewportTools)

| Инструмент | Описание |
|---|---|
| `CaptureCurrentViewport` / `CaptureViewByName` | Скриншот вида (Base64) для vision-анализа |
| `GetCurrentViewInfo` / `GetAvailableViews` / `GetDrawingAreaExtents` | Информация о видах |

## Электрические цепи (CircuitTools)

| Инструмент | Описание |
|---|---|
| `GetPanels` | ElementId всех распределительных щитов (только ID; параметры — через `GetElementInfo`) |
| `SearchPanels` | Поиск щитов по имени/уровню/близости к точке; сводка: цепи, нагрузка (ВА), напряжение |
| `GetElectricalConnections` | Топология подключений (batch): для щита — цепи с нагрузками и питающая линия; для цепи — состав и щит; для светильника — его цепи и свободные коннекторы |
| `RecommendCircuit` | Ранжирование цепей щита для группы светильников: баллы 0..1, разбор по критериям, вариант «новая цепь» с номиналом 10/16/20/25 А |
| `CreateCircuit` | Создать цепь (ElectricalSystem) из элементов, опционально сразу привязать к щиту |
| `SetCircuitPanel` | Привязать цепь к щиту / сменить базовое устройство |
| `AddElementsToCircuit` | Добавить элементы в существующую цепь |

Параметры `RecommendCircuit` (0/null — значение по умолчанию): `maxUtilization`
(0.8), `maxFixturesPerCircuit` (20), `maxDistanceMeters` (15),
`allowedLoadClassificationsJson` (["освещение","lighting"]),
`ignoreLevelMismatch`.

## Экземпляры Revit (InstanceTools)

| Инструмент | Описание |
|---|---|
| `ListRevitInstances` | Открытые экземпляры Revit из discovery-файлов (pid, порт, документ) |
| `SelectRevitInstance` | Переключить MCP-сервер на другой экземпляр Revit |

## Авторизация (AuthTools)

| Инструмент | Описание |
|---|---|
| `StartLogin` | Начать вход (device flow) |
| `GetLoginStatus` | Статус авторизации |

## Служебные (HealthTools)

| Инструмент | Описание |
|---|---|
| `CheckHealth` / `Ping` | Проверка соединения MCP → gRPC → Revit; версия Revit, документ, аптайм |

## Устранение проблем

| Симптом | Причина и действие |
|---|---|
| `CheckHealth` — ошибка соединения | Revit не запущен, или плагин ElectricityRevitPlugin не установлен/не загрузился. Запустить Revit, открыть проект |
| Инструменты не появились в Claude | MCP-сервер не подключён: проверить путь к exe/лаунчеру в конфиге клиента, перезапустить Claude Desktop |
| Запросы уходят «не в тот» Revit | Открыто несколько экземпляров: `ListRevitInstances` → `SelectRevitInstance` |
| Порты 5005–5010 заняты | Закрыть лишние процессы Revit; проверить файлы `%LocalAppData%\ElectricityRevitPlugin\instances\` |
| Ответ «нет активного документа» | В Revit не открыт проект — открыть .rvt |
| Номиналы всех цепей одинаковые (20 А) | Дефолт шаблона в свойстве Rating — уточнить номиналы у пользователя или передать лимит параметром |
