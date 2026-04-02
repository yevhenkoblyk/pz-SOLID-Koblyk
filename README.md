
# pz-SOLID — Практична реалізація SOLID принципів

## Мета
Рефакторинг «анти-SOLID» коду медіаплеєра з застосуванням усіх 5 принципів SOLID на TypeScript.

---

## Аналіз порушень оригінального коду

| Принцип | Де порушено | Опис порушення |
|---|---|---|
| SRP | `MediaPlayer` | Один клас відповідає за конвертацію, збереження і сповіщення |
| OCP | `convert()` | Новий формат вимагає редагування існуючого методу через `if/else` |
| LSP | `RadioPlayer extends BasePlayer` | `RadioPlayer.record()` і `rewind()` кидають `Error` — порушують контракт базового класу |
| ISP | `IDevice` | Один великий інтерфейс змушує всі класи реалізовувати непотрібні методи |
| DIP | `MediaPlayer` | Клас напряму залежить від конкретних реалізацій, а не від абстракцій |

---

## Застосування принципів після рефакторингу

### SRP — Single Responsibility Principle
Клас `MediaPlayer` розбито на три незалежні класи:
- `MediaConverters.ts` — відповідає лише за конвертацію
- `MediaRepository.ts` — відповідає лише за збереження
- `Notifier.ts` — відповідає лише за сповіщення

### OCP — Open/Closed Principle
Замість `if/else` на формати — окремі класи `Mp3Converter`, `WavConverter`, `OggConverter`, кожен реалізує інтерфейс `IMediaConverter`. Новий формат = новий клас, без редагування існуючого коду.

### LSP — Liskov Substitution Principle
Інтерфейс `IDevice` розбито на `IPlayable`, `IRecordable`, `IRewindable`. `RadioPlayer` реалізує лише `IPlayable` — жодних методів що кидають `Error`.

### ISP — Interface Segregation Principle
Замість одного великого `IDevice` — маленькі специфічні інтерфейси: `IMediaConverter`, `IMediaRepository`, `INotifier`, `IPlayable`, `IRecordable`, `IRewindable`.

### DIP — Dependency Inversion Principle
`MediaService` отримує всі залежності через конструктор у вигляді інтерфейсів — не створює жодного об'єкта через `new`.

---

## Структура проєкту

```
pz-SOLID/
├── src/
│   ├── interfaces/
│   │   ├── IMediaConverter.ts
│   │   ├── IMediaRepository.ts
│   │   ├── INotifier.ts
│   │   └── IPlayable.ts
│   ├── original/
│   │   └── MediaPlayer.ts       # анти-SOLID код
│   └── refactored/
│       ├── MediaConverters.ts
│       ├── MediaRepository.ts
│       ├── MediaService.ts
│       └── Notifier.ts
├── tests/
│   └── refactored.spec.ts
├── jest.config.js
├── tsconfig.json
└── package.json
```

## Запуск тестів

```bash
npx jest
```

### Результат

```
Tests: 9 passed, 9 total
```
```

***

Після цього проект **повністю готовий** до здачі. Всі вимоги виконані:
-  TypeScript
-  Всі 5 принципів SOLID реалізовані
-  Інтерфейси й абстракції
-  9 тестів проходять
