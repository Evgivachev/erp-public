# ElectricityRevitPlugin

[![Revit](https://img.shields.io/badge/Revit-2023%20%7C%202024-orange.svg)](https://www.autodesk.com/products/revit)
[![.NET Framework](https://img.shields.io/badge/.NET%20Framework-4.8-blue.svg)](https://dotnet.microsoft.com/download/dotnet-framework)
[![GitHub release](https://img.shields.io/github/v/release/Evgivachev/erp-public)](https://github.com/Evgivachev/erp-public/releases)

Плагин для Autodesk Revit для инженеров-электриков: расстановка светильников,
маркировка и расчёт электрических цепей, работа со щитами, схемы, спецификации
и ещё десятки инструментов на ленте Revit.

Плагин можно использовать двумя способами — они не исключают друг друга:

- **Как обычный плагин** — кнопки на вкладке **ERP** в Revit, привычный клик мышью.
- **В диалоге с ИИ** — через Claude и протокол MCP плагин даёт ИИ-ассистенту
  доступ к модели: попросите словами «расставь светильники в кабинете 305» или
  «подключи выделенные светильники к ЩО-1.1» — и Claude сделает это сам,
  спрашивая подтверждение перед изменением модели.

## Быстрый старт

1. Скачайте установщик под свою версию Revit на странице
   [Releases](../../releases) (MSI или ZIP).
2. Установите плагин — подробности в [docs/installation.md](docs/installation.md).
3. Откройте Revit — на ленте появится вкладка **ERP** с готовыми инструментами.

## Дальше

| Документ | О чём |
|---|---|
| [docs/installation.md](docs/installation.md) | Установка, системные требования, автообновление, удаление |
| [docs/ribbon-commands.md](docs/ribbon-commands.md) | Обычная работа: что умеет каждая группа кнопок на ленте ERP |
| [docs/mcp-setup.md](docs/mcp-setup.md) | Подключение плагина к Claude Desktop / Claude Code через MCP |
| [docs/ai-assistant-usage.md](docs/ai-assistant-usage.md) | Проектирование освещения в диалоге с ИИ: методика, примеры запросов, правила безопасности |

## Поддержка

Нашли ошибку или есть предложение — заведите issue в этом репозитории:
[Issues](../../issues).

## Лицензия

MIT.
