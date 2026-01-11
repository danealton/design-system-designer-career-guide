# Design Tokens (Дизайн-токены)

> Независимые от платформы переменные, хранящие значения дизайна. Фундамент консистентности дизайн-системы.

## Содержание

1. [Что такое Design Tokens](#что-такое-design-tokens)
2. [Типы токенов](#типы-токенов)
3. [Структура и организация](#структура-и-организация)
4. [Семантические vs прямые токены](#семантические-vs-прямые-токены)
5. [Работа с токенами в Figma](#работа-с-токенами-в-figma)
6. [Design Tokens Pipeline](#design-tokens-pipeline)
7. [Инструменты](#инструменты)
8. [Best Practices](#best-practices)

---

## Что такое Design Tokens

**Design Tokens** (дизайн-токены) — это абстрактные значения дизайна, хранящие конкретные свойства (цвета, размеры, отступы, тени и т.д.) в формате, понятном как дизайнерам, так и разработчикам.

### Зачем нужны токены?

**Проблема без токенов:**

- Дизайнер меняет цвет кнопки в 10 местах
- Разработчик использует хардкод значений (`#FF5733`, `16px`, `font-weight: 600`)
- При смене темы приходится менять код во многих местах
- Нет единого источника истины (Single Source of Truth)

**Решение с токенами:**

- Одно место хранения значения
- Автоматическая синхронизация между дизайном и кодом
- Легкая смена темы через изменение токенов
- Консистентность на всех платформах (Web, iOS, Android)

### Пример

Вместо:

```css
.button-primary {
  background-color: #007AFF;
  padding: 12px 24px;
  border-radius: 8px;
}
```

С токенами:

```css
.button-primary {
  background-color: var(--color-brand-primary);
  padding: var(--spacing-md) var(--spacing-lg);
  border-radius: var(--radius-md);
}
```

---

## Типы токенов

### 1. Цвета (Colors)

**Базовая палитра:**

- Brand colors (брендовые цвета)
- Semantic colors (семантические: success, error, warning, info)
- Neutral colors (нейтральные: gray scale)
- Surface colors (цвета поверхностей: background, overlay)

**Пример структуры:**

```text
colors/
  ├── brand/
  │   ├── primary
  │   ├── secondary
  │   └── accent
  ├── semantic/
  │   ├── success
  │   ├── error
  │   ├── warning
  │   └── info
  └── neutral/
      ├── gray-50
      ├── gray-100
      └── ...
```

### 2. Типографика (Typography)

**Параметры:**

- Font families (семейства шрифтов)
- Font sizes (размеры)
- Font weights (насыщенность)
- Line heights (высота строки)
- Letter spacing (межбуквенное расстояние)

**Пример:**

```text
typography/
  ├── font-family/
  │   ├── primary: 'Inter', sans-serif
  │   └── secondary: 'Roboto Mono', monospace
  ├── size/
  │   ├── xs: 12px
  │   ├── sm: 14px
  │   ├── base: 16px
  │   └── lg: 18px
  └── weight/
      ├── regular: 400
      ├── medium: 500
      └── bold: 700
```

### 3. Spacing (Отступы)

**Базовая система:**

- Обычно основана на **8pt grid** (кратность 8 пикселям)
- Названия: xs, sm, md, lg, xl, 2xl и т.д.

**Пример:**

```text
spacing/
  ├── xs: 4px
  ├── sm: 8px
  ├── md: 16px
  ├── lg: 24px
  ├── xl: 32px
  └── 2xl: 48px
```

### 4. Shadows (Тени)

**Уровни глубины:**

- Обычно от 1 до 5+ уровней
- Определяют визуальную иерархию

**Пример:**

```text
shadows/
  ├── sm: 0 1px 2px rgba(0, 0, 0, 0.05)
  ├── md: 0 4px 6px rgba(0, 0, 0, 0.1)
  └── lg: 0 10px 15px rgba(0, 0, 0, 0.1)
```

### 5. Borders (Границы)

**Параметры:**

- Width (толщина)
- Radius (скругление)
- Style (стиль: solid, dashed)

**Пример:**

```text
borders/
  ├── width/
  │   ├── thin: 1px
  │   └── thick: 2px
  └── radius/
      ├── sm: 4px
      ├── md: 8px
      └── lg: 12px
```

### 6. Другие типы

- **Opacity** (прозрачность)
- **Duration** (длительность анимаций)
- **Easing** (кривые анимаций)
- **Z-index** (порядок наложения)
- **Breakpoints** (точки перелома для responsive)

---

## Структура и организация

### Плоская структура vs иерархическая

**❌ Плоская структура (не рекомендуется):**

```text
primary-color
secondary-color
button-padding
card-padding
text-size-large
text-size-small
```

**✅ Иерархическая структура (рекомендуется):**

```text
colors.brand.primary
colors.brand.secondary
spacing.button.padding
spacing.card.padding
typography.size.large
typography.size.small
```

### Рекомендуемая организация

```text
tokens/
  ├── color/
  │   ├── brand.json
  │   ├── semantic.json
  │   └── neutral.json
  ├── spacing/
  │   └── base.json
  ├── typography/
  │   ├── font-family.json
  │   ├── size.json
  │   └── weight.json
  ├── shadows.json
  ├── borders.json
  └── theme/
      ├── light.json
      └── dark.json
```

---

## Семантические vs прямые токены

### Прямые токены (Primitive/Base Tokens)

Хранят конкретные значения напрямую.

**Пример:**

```json
{
  "color-blue-500": "#3B82F6",
  "spacing-16": "16px",
  "font-size-16": "16px"
}
```

**Проблема:** Сложно менять значение во всей системе, нет семантики.

### Семантические токены (Semantic Tokens)

Ссылаются на прямые токены или другие семантические токены, несут смысловую нагрузку.

**Пример:**

```json
{
  "color-background-primary": {
    "value": "{color-blue-500}",
    "type": "color"
  },
  "color-text-primary": {
    "value": "{color-gray-900}",
    "type": "color"
  },
  "spacing-button-padding": {
    "value": "{spacing-16}",
    "type": "dimension"
  },
  "typography-body": {
    "font-size": {
      "value": "{font-size-16}",
      "type": "dimension"
    },
    "line-height": {
      "value": "1.5",
      "type": "number"
    }
  }
}
```

> **Примечание:** Формат может варьироваться в зависимости от инструмента. Некоторые используют упрощенный синтаксис без полей `value` и `type`.

### Двухуровневая система (рекомендуется)

#### Уровень 1: Примитивы (Primitives)

```json
{
  "color": {
    "blue": {
      "50": "#EFF6FF",
      "500": "#3B82F6",
      "900": "#1E3A8A"
    },
    "gray": {
      "50": "#F9FAFB",
      "900": "#111827"
    }
  },
  "spacing": {
    "4": "4px",
    "16": "16px"
  }
}
```

#### Уровень 2: Семантика (Semantic)

```json
{
  "color": {
    "background": {
      "primary": {
        "value": "{color.blue.50}",
        "type": "color"
      },
      "secondary": {
        "value": "{color.gray.50}",
        "type": "color"
      }
    },
    "text": {
      "primary": {
        "value": "{color.gray.900}",
        "type": "color"
      },
      "brand": {
        "value": "{color.blue.500}",
        "type": "color"
      }
    }
  },
  "spacing": {
    "button": {
      "padding": {
        "value": "{spacing.16}",
        "type": "dimension"
      },
      "gap": {
        "value": "{spacing.4}",
        "type": "dimension"
      }
    }
  }
}
```

> **Примечание:** Синтаксис ссылок на другие токены (`{token.name}`) может отличаться в зависимости от инструмента. В Style Dictionary используется формат с полями `value` и `type`. В Token Studio синтаксис может быть упрощенным. Уточняйте документацию выбранного инструмента.

**Преимущества:**

- ✅ Легко менять значения (изменяем примитив — меняется везде)
- ✅ Семантика понятна разработчикам
- ✅ Легко создавать темы (меняем привязки)

---

## Работа с токенами в Figma

### Variables (Figma Variables)

В июне 2023 года Figma представил **Variables** (Переменные) — встроенную функциональность для работы с design tokens прямо в инструменте.

#### Создание Variables

1. **Открыть панель Variables:**
   - `Shift + Cmd/Ctrl + ;` или через меню

2. **Создать коллекцию (Collection):**
   - Название: "Primitives" или "Base Tokens"
   - Режим: "Local" (локально) или "Published" (публиковать)

3. **Добавить переменные:**
   - **Color variables:** для цветов
   - **Number variables:** для spacing, sizes
   - **String variables:** для текста

#### Организация в Figma

**Рекомендуемая структура:**

```text
Variables/
  ├── Primitives/
  │   ├── Colors/
  │   │   ├── Blue / 500
  │   │   ├── Gray / 900
  │   │   └── ...
  │   └── Spacing/
  │       ├── 4
  │       ├── 16
  │       └── ...
  └── Semantic/
      ├── Colors/
      │   ├── Background / Primary
      │   └── Text / Primary
      └── Components/
          └── Button / Padding
```

#### Использование Variables

1. **В свойствах слоя:**
   - Выбрать цвет → нажать "Variables"
   - Выбрать нужную переменную

2. **В компонентах:**
   - Применить Variable к компонентам
   - При изменении Variable обновятся все использования

3. **Режимы (Modes) для тем:**
   - Создать режим "Light" и "Dark"
   - Назначить разные значения переменным в каждом режиме
   - Переключаться между темами

#### Альтернативы Variables (для старых версий Figma)

**Styles (устаревший подход):**

- Color Styles
- Text Styles
- Effect Styles

**Ограничения:**

- Нет числовых значений для spacing
- Нет связей между стилями
- Сложнее управлять темами

---

## Design Tokens Pipeline

**Design Tokens Pipeline** — автоматизированный процесс преобразования токенов из дизайна (Figma) в код для разных платформ.

### Зачем нужен pipeline?

**Без pipeline:**

- Дизайнер меняет токены в Figma
- Разработчик вручную обновляет код
- Риск ошибок и рассинхронизации
- Медленный процесс

**С pipeline:**

- Автоматическая синхронизация
- Консистентность между дизайном и кодом
- Поддержка нескольких платформ
- Быстрое обновление

### Компоненты pipeline

```text
┌─────────────┐
│   Figma     │ (Источник токенов)
│  Variables  │
└──────┬──────┘
       │
       │ (Экспорт/API)
       ▼
┌─────────────┐
│  Tokens     │ (JSON/YAML формат)
│  Format     │
└──────┬──────┘
       │
       │ (Преобразование)
       ▼
┌─────────────┐
│  Platform   │  ┌──────────┐  ┌──────────┐  ┌──────────┐
│  Outputs    │  │   CSS    │  │   iOS    │  │ Android  │
└─────────────┘  └──────────┘  └──────────┘  └──────────┘
```

### Процесс работы

1. **Создание токенов в Figma**
   - Дизайнер создает Variables в Figma

2. **Экспорт токенов**
   - Через Figma API или плагины
   - Экспорт в JSON/YAML формат

3. **Трансформация**
   - Использование инструментов (Style Dictionary, Theo)
   - Преобразование в форматы платформ:
     - **Web:** CSS variables, SCSS, LESS
     - **iOS:** Swift, Objective-C
     - **Android:** Kotlin, XML

4. **Интеграция**
   - Автоматический коммит в репозиторий
   - Обновление библиотек компонентов
   - Деплой в production

### Пример pipeline

**Figma → JSON:**

```json
{
  "color": {
    "brand": {
      "primary": {
        "value": "#3B82F6",
        "type": "color"
      }
    }
  }
}
```

**JSON → CSS:**

```css
:root {
  --color-brand-primary: #3B82F6;
}
```

**JSON → iOS (Swift):**

```swift
extension UIColor {
  static let brandPrimary = UIColor(
    red: 0x3B / 255.0,
    green: 0x82 / 255.0,
    blue: 0xF6 / 255.0,
    alpha: 1.0
  )
}
```

> **Примечание:** В реальных проектах часто используют кастомный extension с инициализатором `init(hex:)` или библиотеки для работы с цветами. Также можно использовать `UIColor(red:green:blue:alpha:)` с конвертацией hex-значения.

---

## Инструменты

### 1. Style Dictionary

**Что это:** Инструмент для трансформации дизайн-токенов в форматы платформ.

**Возможности:**

- Множество форматов вывода (CSS, iOS, Android, JSON и др.)
- Кастомные трансформеры
- Поддержка темизации
- Интеграция с CI/CD

**Ссылка:** <https://amzn.github.io/style-dictionary/>

**Пример конфигурации:**

```json
{
  "source": ["tokens/**/*.json"],
  "platforms": {
    "css": {
      "transformGroup": "css",
      "buildPath": "build/css/",
      "files": [{
        "destination": "variables.css",
        "format": "css/variables"
      }]
    }
  }
}
```

### 2. Token Studio (ранее Figma Tokens)

**Что это:** Плагин Figma для управления дизайн-токенами, который эволюционировал из проекта Figma Tokens.

**Возможности:**

- Визуальный редактор токенов в Figma
- Синхронизация токенов между Figma и кодом
- Поддержка темизации (Light/Dark режимы)
- Экспорт в различные форматы (JSON, CSS, SCSS, iOS, Android)
- Интеграция с Style Dictionary
- Работа с коллекциями и режимами

**Ссылка:** <https://tokens.studio/> (основной сайт), <https://github.com/tokens-studio/figma-plugin> (репозиторий плагина)

> **Примечание:** Token Studio — это преемник проекта Figma Tokens. Старый репозиторий <https://github.com/six7/figma-tokens> больше не поддерживается активной разработкой.

### 3. Theo (Salesforce)

**Что это:** Утилита для трансформации дизайн-токенов, разработанная командой Salesforce.

**Особенности:**

- Используется в Salesforce Lightning Design System
- Поддержка форматов: CSS, SCSS, JSON, Android, iOS
- Простая конфигурация через YAML/JSON

**Ссылка:** <https://github.com/salesforce-ux/theo>

> **Примечание:** Проект активно поддерживается, но Style Dictionary (Amazon) стал более популярным решением в сообществе.

### 4. Figma API

**Что это:** RESTful API для программного доступа к файлам и данным Figma.

**Применение:**

- Автоматический экспорт Variables из Figma
- Синхронизация токенов в pipeline
- Создание кастомных интеграций и скриптов
- Извлечение данных о компонентах и стилях

**Ссылка:** <https://www.figma.com/developers/api>

> **Примечание:** Для доступа к API требуется токен доступа. Variables доступны через API начиная с обновлений 2023 года.

---

## Best Practices

### 1. Используйте иерархическую структуру

**✅ Хорошо:**

```text
colors.brand.primary
spacing.button.padding
typography.heading.large
```

**❌ Плохо:**

```text
primaryColor
buttonPadding
headingLarge
```

### 2. Разделяйте примитивы и семантику

- **Примитивы:** базовые значения (цвета палитры, базовые размеры)
- **Семантика:** значения с контекстом (background-primary, text-secondary)

### 3. Используйте осмысленные имена

**✅ Хорошо:**

- `color-background-primary`
- `spacing-button-padding-horizontal`
- `typography-body-size`

**❌ Плохо:**

- `color-1`
- `spacing-a`
- `text-size`

### 4. Применяйте систему масштабирования

- Используйте **8pt grid** для spacing
- Типографика: модульная шкала (1.25x, 1.5x)
- Согласованные пропорции

### 5. Документируйте токены

- Описание назначения токена
- Примеры использования
- Когда использовать vs не использовать

### 6. Версионируйте токены

- Поддерживайте обратную совместимость
- Используйте deprecation для устаревших токенов
- Документируйте breaking changes

### 7. Тестируйте токены

- Проверка контрастности (accessibility)
- Проверка на разных платформах
- Визуальное тестирование

### 8. Автоматизируйте синхронизацию

- Настройте pipeline для автоматической синхронизации
- Используйте CI/CD для проверок
- Уведомляйте команду об изменениях

### 9. Поддерживайте консистентность

- Единый стиль именования
- Единая структура для всех типов токенов
- Code review для новых токенов

### 10. Начните с малого

- Начните с основных токенов (цвета, spacing)
- Постепенно расширяйте систему
- Регулярно рефакторьте и улучшайте

---

## Примеры из известных дизайн-систем

### Material Design 3

- Комплексная система дизайн-токенов для всех значений
- Поддержка динамической цветовой палитры (Material You)
- Поддержка тем (Light/Dark) и кастомизация
- Мультиплатформенность (Web, Android, iOS, Flutter)
- Открытая документация токенов

**Ссылка:** <https://m3.material.io/design-tokens>

### Ant Design

- Структурированные токены по категориям (цвета, типографика, spacing и др.)
- Кастомизация через токены (Less variables, CSS-in-JS)
- Поддержка различных тем и режимов
- Экспорт токенов в различные форматы

**Ссылка:** <https://ant.design/docs/spec/colors>

### Carbon Design System (IBM)

- Детальная система токенов с иерархической структурой
- Поддержка брендинга и темизации через токены
- Инструменты для работы с токенами (интеграция с Style Dictionary)
- Экспорт для React, Vue, Angular, Web Components

**Ссылка:** <https://carbondesignsystem.com/guidelines/color/usage>

---

## Связанные темы

- [Accessibility](accessibility.md) - токены для доступности (контрастность)
- [Theming](GLOSSARY.md#theming-темизация) - использование токенов для тем
- [8pt Grid](GLOSSARY.md#8pt-grid-8pt-сетка) - система spacing
- [Design Tokens Pipeline](GLOSSARY.md#design-tokens-pipeline) - автоматизация

---

## Полезные ресурсы

- [Design Tokens Community Group](https://www.designtokens.org/) - стандарты и спецификации
- [Style Dictionary Docs](https://amzn.github.io/style-dictionary/)
- [Figma Variables Documentation](https://help.figma.com/hc/en-us/articles/15339657135383)
- [Token Studio Plugin](https://tokens.studio/)

---

**Последнее обновление:** Январь 2026
