# Development Environment Creation Repo

- This is a repo to test building a project w/ CMake, CppUTest, CppCheck, gcovr, and clang-format
- ...A Docker container encapsulating all tools would be ideal, but our MCU doesn't have a mainstream Linux toolchain and a Windows Docker containers require Windows Pro
- ...We can still establish a uniform development environment, it's just that there needs to be a list of installation items and setup instructions in place of a Docker container w/ all tools installed and specified

## `cmake/` Directory

- Generic build space to clone down your source code repo and build w/ CMake to target both Atmel/Microchip's AVR32 MCU and Windows
- Hello World project may be cloned into src/ as an example
  - Run `git clone https://github.com/Mouse-Unit-07/software-hello-world.git .` to clone the Hello World repository into src/ folder
- Provides unit testing, CppCheck, and format checking when a project is built for Windows

- **Dependent on**
  - MSYS2 MINGW64 installation: `C:/msys64/mingw64/bin`
    - gcc 15.0.1
    - g++ 15.0.1
    - gcov 15.0.1
    - CMake 4.0.2
    - Ninja 1.12.1
  - Python 3.9
    - pip 25.1.1 -> gcovr 8.3 (Python wrapper around gcov)
  - CppUTest: https://github.com/cpputest/cpputest.git
  - AVR32 toolchain: `C:/Program Files (x86)/Atmel/Studio/7.0/toolchain/avr32/avr32-gnu-toolchain/bin`
  - AVR32 include library: `C:/Program Files (x86)/Atmel/Studio/7.0/Packs/atmel/UC3L_DFP/1.0.59/include/AT32UC3L0256`
- **Build instructions**
  - AVR32 MCU build
    - Navigate to top level project directory w/ MSYS2 MINGW64 terminal
    - `cmake --preset mcu-build`
    - `cmake --build --preset mcu-build`
  - Windows build
    - Navigate to top level project directory w/ MSYS2 MINGW64 terminal
    - `cmake --preset windows-build`
    - `cmake --build --preset windows-build`
    - `ctest --preset windows-build` to run CppUTest unit tests
  - Running CppCheck
    - Build for Windows w/ commands above
    - Navigate to the Windows build directory `build/windows_build` w/ MSYS2 MINGW64 terminal, then run CMake "build" command to run CppCheck
    - `cmake --build . --target cppcheck`
  - Running clang-format_sources
    - Build for Windows w/ commands above
    - Navigate to the Windows build directory `build/windows_build` w/ MSYS2 MINGW64 terminal, then run CMake "build" command to run clang-format
    - `cmake --build . --target format_sources`
    - Formatted copies of source files will be in build/windows_build/clang-format-output
  - Running gcovr
    - Build for Windows w/ commands above and run ctest
    - Open Windows command prompt where gcovr's been installed through pip
    - Navigate to top level project directory
    - `gcovr -r . --filter=src/`

## `microchip-studio-ide/` directory

- Hello World project that builds using Microchip Studio IDE
- Exists to analyze the compiler/linker flags needed for CMake to build a project using the AVR32 toolchain
- **Dependent on**
  - Microchip Studio installation
