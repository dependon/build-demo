cmake_minimum_required(VERSION 3.0.0)

project(niubi)


set(TARGET_NAME niubi)
set(CMD_NAME niubi)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

file(GLOB_RECURSE  SOURCES  "*.cpp")
file(GLOB_RECURSE SOURCESC "*.c")
file(GLOB_RECURSE HEADERS "*.h")
file(GLOB_RECURSE FORMS "*.ui")
file(GLOB_RECURSE RESOURCES "*.qrc")

include_directories(${CMAKE_INCLUDE_CURRENT_DIR})
include_directories(${CMAKE_CURRENT_BINARY_DIR})

configure_file(${PROJECT_SOURCE_DIR}/config.h.in ${PROJECT_BINARY_DIR}/config.h @ONLY)

set(PROJECT_INCLUDE
    ${PROJECT_SOURCE_DIR}
    )

set(QtModule Core Gui Widgets LinguistTools )

#先查找到这些qt相关的模块以供链接使用
find_package(Qt5 REQUIRED ${QtModule})

find_package(PkgConfig REQUIRED)
pkg_check_modules(3rd_lib REQUIRED
    )

### translations

#file(GLOB TS LIST_DIRECTORIES false translations/${CMD_NAME}*.ts)

#set_source_files_properties(${TS} PROPERTIES OUTPUT_LOCATION ${PROJECT_SOURCE_DIR}/translations)

#qt5_create_translation(QM ${SOURCES}  ${GLOB})

#if (NOT (${CMAKE_BUILD_TYPE} MATCHES "Debug"))
#    qt5_create_translation(QM ${SOURCES} ${TS})
#endif ()

# 设置不删除生成的文件夹内容文件
set_directory_properties(PROPERTIES CLEAN_NO_CUSTOM 1)

add_executable( ${PROJECT_NAME} ${SOURCES} ${SOURCESC} ${HEADERS} ${RESOURCES} ${FORMS} ${QM})

target_include_directories(${PROJECT_NAME} PUBLIC ${3rd_lib_INCLUDE_DIRS} ${PROJECT_INCLUDE} )
target_link_libraries(${PROJECT_NAME} ${3rd_lib_LIBRARIES} pthread )
#将工程与这些模块链接起来
qt5_use_modules(${PROJECT_NAME} ${QtModule})

# Find includes in corresponding build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)


#install

install(TARGETS ${TARGET_NAME} DESTINATION /usr/bin)
#desktop
install(FILES ${PROJECT_SOURCE_DIR}/niubi.desktop
    DESTINATION /usr/share/applications)
#icon
install(FILES ${PROJECT_SOURCE_DIR}/niubi.svg
    DESTINATION /usr/share/icons)

##translations
#install(DIRECTORY ${PROJECT_SOURCE_DIR}/translations
#    DESTINATION ${APPSHAREDIR}
#    FILES_MATCHING PATTERN "*.qm")



