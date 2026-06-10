echo '# Лабораторная работа №6: Пакетирование с CPack

**Студент:** Ольга (ИУ8-25)
**Репозиторий:** https://github.com/OlgaIAK/lab06

## Цель работы

Изучить средства пакетирования программного обеспечения на примере CPack.

## Часть 1. Туториал

### 1.1 Настройка окружения
```
export GITHUB_USERNAME=OlgaIAK
export GITHUB_EMAIL=ovalekseeva07@mail.ru
alias edit=nano
alias gsed=sed
```
### 1.2 Создание проекта
```
mkdir -p projects/lab06
cd projects/lab06
git init
```
### 1.3 Настройка версий в CMakeLists.txt
```
set(PRINT_VERSION_MAJOR 0)
set(PRINT_VERSION_MINOR 1)
set(PRINT_VERSION_PATCH 0)
set(PRINT_VERSION_TWEAK 0)
set(PRINT_VERSION ${PRINT_VERSION_MAJOR}.${PRINT_VERSION_MINOR}.${PRINT_VERSION_PATCH}.${PRINT_VERSION_TWEAK})
set(PRINT_VERSION_STRING "v${PRINT_VERSION}")
```
### 1.4 Создание файлов для пакетов

Файл DESCRIPTION:
Static C++ library for printing text to console

Файл ChangeLog.md:
* Wed Jun 10 2026 OlgaIAK <ovalekseeva07@mail.ru> 0.1.0.0
- Initial RPM release

### 1.5 Конфигурация CPack (CPackConfig.cmake)
```
include(InstallRequiredSystemLibraries)

set(CPACK_PACKAGE_CONTACT ovalekseeva07@mail.ru)
set(CPACK_PACKAGE_VERSION_MAJOR ${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR ${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH ${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK ${PRINT_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION ${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_FILE ${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "static C++ library for printing")

set(CPACK_RESOURCE_FILE_LICENSE ${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README ${CMAKE_CURRENT_SOURCE_DIR}/README.md)

set(CPACK_RPM_PACKAGE_NAME "print-devel")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "print")
set(CPACK_RPM_CHANGELOG_FILE ${CMAKE_CURRENT_SOURCE_DIR}/ChangeLog.md)
set(CPACK_RPM_PACKAGE_RELEASE 1)

set(CPACK_DEBIAN_PACKAGE_NAME "libprint-dev")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)

include(CPack)
```
### 1.6 Создание пакетов
```
cmake -H. -B_build
cmake --build _build
cd _build
cpack -G "TGZ"

Результат: архив print-0.1.0.0-Linux.tar.gz
```
### 1.7 Отправка на GitHub
```
git add .
git commit -m "added cpack config"
git tag v0.1.0.0
git remote add origin https://github.com/OlgaIAK/lab06
git push -u origin master --tags
```
## Часть 2. Домашнее задание

### 2.1 Цель

Настроить автоматическую сборку пакетов для приложения solver.

### 2.2 Проект solver

Создано приложение для решения линейных и квадратных уравнений.

### 2.3 Настройка CPack

Настроены пакеты:
- DEB для Ubuntu/Debian
- RPM для Fedora/RHEL
- WIX для Windows
- DragNDrop для macOS

### 2.4 GitHub Actions

Создан файл .github/workflows/release.yml.
При создании тега v1.0.0 автоматически собираются пакеты для всех платформ.

### 2.5 Создание релиза
```
git tag -a v1.0.0 -m "First release"
git push origin v1.0.0
```
### 2.6 Результаты

| Платформа | Тип пакета |
|-----------|------------|
| Ubuntu/Debian | .deb |
| Fedora/RHEL | .rpm |
| macOS | .dmg |
| Windows | .msi |

## Заключение

В ходе работы изучены CPack и GitHub Actions для автоматической сборки пакетов.

## Ссылки

- https://github.com/OlgaIAK/lab06
- https://github.com/OlgaIAK/solver
' > REPORT.md
