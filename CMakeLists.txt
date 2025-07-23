cmake_minimum_required(VERSION 3.16)

project(Group_Project VERSION 0.1 LANGUAGES CXX)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Modified to include Multimedia
find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets Multimedia)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets Multimedia)

set(PROJECT_SOURCES
        main.cpp
        mainwindow.cpp
        mainwindow.h
        mainwindow.ui
)

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(Group_Project
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
        apollo.h apollo.cpp apollo.ui
        questionbank.h questionbank.cpp
        level.h level.cpp
        customizewidget.h customizewidget.cpp
        global.h
        quizpage.h quizpage.cpp quizpage.ui
        score.h score.cpp score.ui
        zombiestoryline.h zombiestoryline.cpp zombiestoryline.ui
        difficultylevels.h difficultylevels.cpp difficultylevels.ui
        zombiequestionbank.h zombiequestionbank.cpp
        easylevel.h easylevel.cpp easylevel.ui
        mediumlevel.h mediumlevel.cpp mediumlevel.ui
        hardlevel.h hardlevel.cpp hardlevel.ui
        viewscorespage.h viewscorespage.cpp viewscorespage.ui
        resources.qrc
        backgroundsetter.h backgroundsetter.cpp
    )
else()
    if(ANDROID)
        add_library(Group_Project SHARED
            ${PROJECT_SOURCES}
        )
    else()
        add_executable(Group_Project
            ${PROJECT_SOURCES}
        )
    endif()
endif()

# Modified to include Multimedia
target_link_libraries(Group_Project PRIVATE Qt${QT_VERSION_MAJOR}::Widgets Qt${QT_VERSION_MAJOR}::Multimedia)

if(${QT_VERSION} VERSION_LESS 6.1.0)
  set(BUNDLE_ID_OPTION MACOSX_BUNDLE_GUI_IDENTIFIER com.example.Group_Project)
endif()
set_target_properties(Group_Project PROPERTIES
    ${BUNDLE_ID_OPTION}
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

include(GNUInstallDirs)
install(TARGETS Group_Project
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

if(QT_VERSION_MAJOR EQUAL 6)
    qt_finalize_executable(Group_Project)
endif()
