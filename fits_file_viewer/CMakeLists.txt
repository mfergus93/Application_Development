cmake_minimum_required(VERSION 3.5)

project(FITS_Viewer_Application VERSION 0.1 LANGUAGES CXX)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets)
list(APPEND CMAKE_MODULE_PATH "c:/Users/Matt/OpenCV/build/x64/vc16")
find_package( OpenCV REQUIRED )

set(PROJECT_SOURCES
        main.cpp
        mainwindow.cpp
        mainwindow.h
        mainwindow.ui
)

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(FITS_Viewer_Application
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
    )
# Define target properties for Android with Qt 6 as:
#    set_property(TARGET FITS_Viewer_Application APPEND PROPERTY QT_ANDROID_PACKAGE_SOURCE_DIR
#                 ${CMAKE_CURRENT_SOURCE_DIR}/android)
# For more information, see https://doc.qt.io/qt-6/qt-add-executable.html#target-creation
else()
    if(ANDROID)
        add_library(FITS_Viewer_Application SHARED
            ${PROJECT_SOURCES}
        )
# Define properties for Android with Qt 5 after find_package() calls as:
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
    else()
        add_executable(FITS_Viewer_Application
            ${PROJECT_SOURCES}
        )
    endif()
endif()


include_directories(PRIVATE "C:/Users/Matt/opencv/build")
target_link_libraries(${PROJECT_NAME} PRIVATE ${OpenCV_LIBS} )

target_include_directories(${PROJECT_NAME} PRIVATE "C:/Users/Matt/zlib/include")
target_link_libraries(${PROJECT_NAME} PRIVATE "C:/Users/Matt/zlib/lib/zlib.lib")

target_include_directories(${PROJECT_NAME} PRIVATE "C:/Program Files (x86)/CFITSIO/include")
target_link_libraries(${PROJECT_NAME} PRIVATE "C:/Program Files (x86)/CFITSIO/lib/cfitsio.lib")

target_link_libraries(${PROJECT_NAME} PRIVATE Qt${QT_VERSION_MAJOR}::Widgets)


#target_link_libraries(FITS_Viewer_Application PRIVATE Qt${QT_VERSION_MAJOR}::Widgets ${OpenCV_LIBS} ${CFITSIO_LIBRARIES} ${ZLIB_LIBRARIES})


# Qt for iOS sets MACOSX_BUNDLE_GUI_IDENTIFIER automatically since Qt 6.1.
# If you are developing for iOS or macOS you should consider setting an
# explicit, fixed bundle identifier manually though.
if(${QT_VERSION} VERSION_LESS 6.1.0)
  set(BUNDLE_ID_OPTION MACOSX_BUNDLE_GUI_IDENTIFIER com.example.FITS_Viewer_Application)
endif()
set_target_properties(FITS_Viewer_Application PROPERTIES
    ${BUNDLE_ID_OPTION}
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

include(GNUInstallDirs)
install(TARGETS FITS_Viewer_Application
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

if(QT_VERSION_MAJOR EQUAL 6)
    qt_finalize_executable(FITS_Viewer_Application)
endif()
