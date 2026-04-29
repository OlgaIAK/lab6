# Лабораторная работа №6: Пакетирование с CPack

**Студент:** Ольга (ИУ8-25)  
**Репозиторий:** https://github.com/OlgaIAK/lab6

## Цель работы
Изучить средства пакетирования программного обеспечения на примере CPack.

## Выполненные действия

1.  **Настроены переменные версии** в `CMakeLists.txt` (major, minor, patch, tweak).
2.  **Созданы файлы описания**:
    *   `DESCRIPTION` — краткое описание пакета.
    *   `ChangeLog.md` — журнал изменений для RPM-пакета.
3.  **Написан конфигурационный файл CPack** (`CPackConfig.cmake`), который определяет:
    *   контактную информацию,
    *   версию пакета,
    *   генераторы пакетов для разных ОС (DEB, RPM, TGZ, WIX).
4.  **Настроен CI (Travis CI)** для автоматической сборки пакетов при создании тега (релиза).

## Файлы конфигурации

### Фрагмент `CMakeLists.txt` с версией
```cmake
set(PRINT_VERSION_MAJOR 0)
set(PRINT_VERSION_MINOR 1)
set(PRINT_VERSION_PATCH 0)
set(PRINT_VERSION_TWEAK 0)
set(PRINT_VERSION "${PRINT_VERSION_MAJOR}.${PRINT_VERSION_MINOR}.${PRINT_VERSION_PATCH}.${PRINT_VERSION_TWEAK}")
