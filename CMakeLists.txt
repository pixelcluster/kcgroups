cmake_minimum_required(VERSION 3.10)

project(ForegroundBooster)

find_package(ECM 6.0 REQUIRED NO_MODULE)
set(CMAKE_MODULE_PATH ${ECM_MODULE_PATH})

include(KDEInstallDirs)
include(KDECompilerSettings)
include(KDECMakeSettings)
include(ECMConfiguredInstall)

find_package(KF6 REQUIRED COMPONENTS Config DBusAddons)
find_package(KF5CGroups REQUIRED)
find_package(LibTaskManager CONFIG REQUIRED)

if(BUILD_TESTING)
    add_subdirectory(tests)
else()
    add_compile_definitions(QT_NO_DEBUG_OUTPUT)
endif()

add_executable(foreground_booster foregroundbooster.cpp foregroundbooster.h booster_main.cpp)
kconfig_add_kcfg_files(foreground_booster boostersettings.kcfgc)
target_link_libraries(
    foreground_booster
    Qt6::DBus
    PW::LibTaskManager
    KF6::ConfigCore
    KF6::ConfigGui
    KF6::DBusAddons
    KF5::CGroups)
install(TARGETS foreground_booster ${KDE_INSTALL_TARGETS_DEFAULT_ARGS})

configure_file(org.kde.foreground-booster.desktop.in ${CMAKE_CURRENT_BINARY_DIR}/org.kde.foreground-booster.desktop @ONLY)
install(FILES  ${CMAKE_CURRENT_BINARY_DIR}/org.kde.foreground-booster.desktop DESTINATION ${KDE_INSTALL_APPDIR})
install(FILES  ${CMAKE_CURRENT_BINARY_DIR}/org.kde.foreground-booster.desktop DESTINATION ${KDE_INSTALL_AUTOSTARTDIR})


ecm_install_configured_files(INPUT plasma-foreground-booster.service.in DESTINATION ${KDE_INSTALL_SYSTEMDUSERUNITDIR})
