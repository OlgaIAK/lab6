echo '# Лабораторная работа №6: Пакетирование с CPack

**Студент:** Ольга (ИУ8-25)
**Репозиторий:** https://github.com/OlgaIAK/lab06

## Цель работы

Изучить средства пакетирования программного обеспечения на примере CPack.

## Часть 1. Туториал

### 1.1 Настройка окружения

export GITHUB_USERNAME=OlgaIAK
export GITHUB_EMAIL=ovalekseeva07@mail.ru
alias edit=nano
alias gsed=sed

### 1.2 Создание проекта

mkdir -p projects/lab06
cd projects/lab06
git init

Созданы файлы:
- CMakeLists.txt
- print.cpp
- print.h
- LICENSE
- README.md

### 1.3 Настройка версий в CMakeLists.txt

set(PRINT_VERSION_MAJOR 0)
set(PRINT_VERSION_MINOR 1)
set(PRINT_VERSION_PATCH 0)
set(PRINT_VERSION_TWEAK 0)
set(PRINT_VERSION ${PRINT_VERSION_MAJOR}.${PRINT_VERSION_MINOR}.${PRINT_VERSION_PATCH}.${PRINT_VERSION_TWEAK})
set(PRINT_VERSION_STRING "v${PRINT_VERSION}")

### 1.4 Создание файлов для пакетов

cat > DESCRIPTION << EOF
Static C++ library for printing text to console
EOF

cat > ChangeLog.md << EOF
* $(date) OlgaIAK <ovalekseeva07@mail.ru> 0.1.0.0
- Initial RPM release
EOF

### 1.5 Конфигурация CPack (CPackConfig.cmake)

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

### 1.6 Подключение CPack к CMakeLists.txt

echo "include(CPackConfig.cmake)" >> CMakeLists.txt

### 1.7 Создание пакетов

cmake -H. -B_build
cmake --build _build
cd _build
cpack -G "TGZ"

Результат: print-0.1.0.0-Linux.tar.gz

### 1.8 Отправка на GitHub

git add .
git commit -m "Initial commit with print library"
git add .
git commit -m "added cpack config"
git tag v0.1.0.0
git remote add origin https://github.com/OlgaIAK/lab06
git push -u origin master --tags

## Часть 2. Домашнее задание

### 2.1 Цель

Настроить автоматическую сборку пакетов для приложения solver из предыдущей лабораторной работы. Каждый новый релиз должен содержать:
- архивы с исходниками (.tar.gz, .zip)
- бинарные пакеты (.deb, .rpm, .msi, .dmg)

### 2.2 Создание проекта solver

cd ~/OlgaIAK/workspace
mkdir solver && cd solver
git init

Создан файл solver.cpp:

#include <iostream>
#include <cmath>
#include <string>

double solveLinear(double a, double b) {
    if (a == 0) {
        std::cerr << "Error: a cannot be 0" << std::endl;
        return NAN;
    }
    return -b / a;
}

void solveQuadratic(double a, double b, double c) {
    if (a == 0) {
        std::cout << "Linear: x = " << solveLinear(b, c) << std::endl;
        return;
    }
    double d = b*b - 4*a*c;
    if (d > 0) {
        double x1 = (-b + sqrt(d)) / (2*a);
        double x2 = (-b - sqrt(d)) / (2*a);
        std::cout << "Two roots: x1 = " << x1 << ", x2 = " << x2 << std::endl;
    } else if (d == 0) {
        double x = -b / (2*a);
        std::cout << "One root: x = " << x << std::endl;
    } else {
        std::cout << "No real roots" << std::endl;
    }
}

int main(int argc, char* argv[]) {
    if (argc < 4) {
        std::cout << "Usage:\n  solver linear a b\n  solver quadratic a b c\n";
        return 1;
    }
    std::string type = argv[1];
    if (type == "linear" && argc == 4) {
        double a = std::stod(argv[2]), b = std::stod(argv[3]);
        double res = solveLinear(a, b);
        if (!std::isnan(res)) std::cout << "x = " << res << std::endl;
    } else if (type == "quadratic" && argc == 5) {
        double a = std::stod(argv[2]), b = std::stod(argv[3]), c = std::stod(argv[4]);
        solveQuadratic(a, b, c);
    }
    return 0;
}

### 2.3 CMakeLists.txt для solver

cmake_minimum_required(VERSION 3.10)
project(solver VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(solver solver.cpp)
install(TARGETS solver DESTINATION bin)

include(CPackConfig.cmake)

### 2.4 CPackConfig.cmake для solver

include(InstallRequiredSystemLibraries)

set(CPACK_PACKAGE_CONTACT "ovalekseeva07@mail.ru")
set(CPACK_PACKAGE_VERSION_MAJOR ${PROJECT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR ${PROJECT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH ${PROJECT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION "${PROJECT_VERSION}")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Solver for mathematical equations")
set(CPACK_PACKAGE_DESCRIPTION_FILE ${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)

set(CPACK_DEBIAN_PACKAGE_NAME "solver")
set(CPACK_DEBIAN_PACKAGE_DEPENDS "libc6")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER ${CPACK_PACKAGE_CONTACT})

set(CPACK_RPM_PACKAGE_NAME "solver")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_RELEASE 1)

set(CPACK_WIX_UPGRADE_GUID "12345678-1234-1234-1234-123456789abc")
set(CPACK_WIX_PRODUCT_NAME "Solver")

set(CPACK_DMG_VOLUME_NAME "Solver Installer")

include(CPack)

### 2.5 DESCRIPTION

Solver is a command-line application for solving linear and quadratic equations.

### 2.6 GitHub Actions (.github/workflows/release.yml)

name: Create Release Packages

on:
  push:
    tags:
      - '"'"'v*'"'"'

jobs:
  build:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        include:
          - os: ubuntu-latest
            generator: "DEB;RPM;TGZ"
          - os: windows-latest
            generator: "WIX;ZIP"
          - os: macos-latest
            generator: "DragNDrop;TGZ"

    steps:
      - uses: actions/checkout@v3

      - name: Configure CMake
        run: cmake -B build -DCMAKE_BUILD_TYPE=Release

      - name: Build
        run: cmake --build build --config Release

      - name: Package
        run: cd build && cpack -G "${{ matrix.generator }}"

      - name: Upload Release
        uses: softprops/action-gh-release@v1
        with:
          files: |
            build/*.deb
            build/*.rpm
            build/*.dmg
            build/*.msi
            build/*.zip
            build/*.tar.gz
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

### 2.7 Создание релиза

git add .
git commit -m "Initial commit with CI/CD"
git remote add origin https://github.com/OlgaIAK/solver
git push -u origin master
git tag -a v1.0.0 -m "First release of solver"
git push origin v1.0.0

### 2.8 Результаты сборки

| Платформа | Тип пакета | Имя файла |
|-----------|------------|-----------|
| Ubuntu/Debian | DEB | solver-1.0.0-Linux.deb |
| Fedora/RHEL | RPM | solver-1.0.0-Linux.rpm |
| macOS | DMG | solver-1.0.0-Darwin.dmg |
| Windows | MSI | solver-1.0.0-Windows.msi |
| Все платформы | Архив | solver-1.0.0.tar.gz |

## Заключение

В ходе выполнения лабораторной работы изучены:
- CPack - инструмент для создания установочных пакетов
- GitHub Actions - CI/CD для автоматической сборки
- Создание пакетов для Linux (DEB, RPM), macOS (DMG) и Windows (MSI)

## Ссылки

- https://github.com/OlgaIAK/lab06
- https://github.com/OlgaIAK/solver
- https://github.com/OlgaIAK/solver/releases
' > REPORT.md
