cmake_minimum_required(VERSION 3.28)
project(feedback-module VERSION 0.1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_AUTOMOC ON)

# --- Dependencies ---
# Qt6 Core (required for all Basecamp modules); RemoteObjects is pulled in by the
# Logos C++ SDK's IPC and must be linked explicitly for the darwin linker.
find_package(Qt6 REQUIRED COMPONENTS Core RemoteObjects)

# libsodium — this module's crypto_box_seal
find_package(PkgConfig REQUIRED)
pkg_check_modules(sodium IMPORTED_TARGET libsodium)

add_subdirectory(core)
