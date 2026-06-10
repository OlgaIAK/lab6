echo '# Лабораторная работа №6: Пакетирование с CPack

**Студент:** Ольга (ИУ8-25)  
**Репозиторий:** https://github.com/OlgaIAK/lab06

## Цель работы

Изучить средства пакетирования программного обеспечения на примере CPack.

## Часть 1. Туториал

### 1.1 Настройка окружения

export GITHUB_USERNAME=OlgaIAK
export GITHUB_EMAIL=ovalekseeva07@mail.ru

### 1.2 Создание проекта

Создан проект с библиотекой print. Настроены переменные версии в CMakeLists.txt.

### 1.3 Конфигурация CPack

Создан файл CPackConfig.cmake с настройками для DEB и RPM пакетов.

### 1.4 Создание пакетов

Создан архив print-0.1.0.0-Linux.tar.gz

## Часть 2. Домашнее задание

### 2.1 Цель

Настроить автоматическую сборку пакетов для приложения solver.

### 2.2 Проект solver

Создано приложение для решения линейных и квадратных уравнений.

### 2.3 Настройка CPack

Настроены пакеты: DEB (Ubuntu), RPM (Fedora), WIX (Windows), DragNDrop (macOS).

### 2.4 GitHub Actions

Создан файл .github/workflows/release.yml. При создании тега v1.0.0 автоматически собираются пакеты для всех платформ.

### 2.5 Результаты

- Ubuntu/Debian: .deb
- Fedora/RHEL: .rpm
- macOS: .dmg
- Windows: .msi

## Ссылки

- https://github.com/OlgaIAK/lab06
- https://github.com/OlgaIAK/solver' > REPORT.md
