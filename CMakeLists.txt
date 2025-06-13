cmake_minimum_required(VERSION 3.10)
project(GravityGUI)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)

if(NOT DEFINED SFML_DIR)
    set(SFML_DIR "$ENV{SFML_DIR}/lib/cmake/SFML")
endif()

find_package(SFML 2.6 REQUIRED COMPONENTS graphics window system audio)

# Ищем SFML перед добавлением исполняемого файла
find_package(SFML 2.6 REQUIRED COMPONENTS graphics window system audio)

# Добавляем исполняемый файл
add_executable(${PROJECT_NAME} 
    src/main.cpp
    src/GravityModel.h
    src/GravityModel.cpp
    src/Render.h
    src/Render.cpp
    src/Grid.h
    src/Grid.cpp
    src/Cell.h
    src/Cell.cpp
    src/GProfile.h
    src/GProfile.cpp)

# Подключаем SFML
if(SFML_FOUND)
    target_include_directories(${PROJECT_NAME} PRIVATE ${SFML_INCLUDE_DIR})
    target_link_libraries(${PROJECT_NAME} PRIVATE 
        sfml-graphics 
        sfml-window 
        sfml-system 
        sfml-audio)
endif()

# Настройка выходных каталогов
set_target_properties(${PROJECT_NAME} PROPERTIES 
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin
    RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR}/bin/Debug
    RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR}/bin/Release)

# Копирование ресурсов (более правильный способ)
add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
    ${CMAKE_SOURCE_DIR}/resources
    $<TARGET_FILE_DIR:${PROJECT_NAME}>/resources)
