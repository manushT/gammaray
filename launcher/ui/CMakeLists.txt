set(gammaray_launcher_ui_srcs
  promolabel.cpp
  launcherwindow.cpp
  launchpage.cpp
  selftestpage.cpp
  connectpage.cpp
  attachdialog.cpp
  processmodel.cpp
  processfiltermodel.cpp
  probeabimodel.cpp
)
if(NOT WIN32)
  list(APPEND gammaray_launcher_ui_srcs processlist_unix.cpp)
else()
  list(APPEND gammaray_launcher_ui_srcs processlist_win.cpp)
endif()

qt4_wrap_ui(gammaray_launcher_ui_srcs
  attachdialog.ui
  launcherwindow.ui
  launchpage.ui
  selftestpage.ui
  connectpage.ui
)

add_library(gammaray_launcher_ui SHARED ${gammaray_launcher_ui_srcs})
generate_export_header(gammaray_launcher_ui)
set_target_properties(gammaray_launcher_ui PROPERTIES ${GAMMARAY_DEFAULT_LIBRARY_PROPERTIES})

target_include_directories(gammaray_launcher_ui PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>)
target_link_libraries(gammaray_launcher_ui
  LINK_PUBLIC
  gammaray_launcher

  LINK_PRIVATE
  ${QT_QTCORE_LIBRARIES}
  ${QT_QTCONCURRENT_LIBRARIES}
  ${QT_QTGUI_LIBRARIES}
  ${QT_QTNETWORK_LIBRARIES}
  gammaray_ui
)

install(TARGETS gammaray_launcher_ui EXPORT GammaRayTargets ${INSTALL_TARGETS_DEFAULT_ARGS})

ecm_generate_pri_file(BASE_NAME GammaRayLauncherUi
                      LIB_NAME gammaray_launcher_ui
                      DEPS "core gui GammaRayLauncher"
                      FILENAME_VAR PRI_FILENAME
                      INCLUDE_INSTALL_DIR ${INCLUDE_INSTALL_DIR}
)
install(FILES ${PRI_FILENAME} DESTINATION ${ECM_MKSPECS_INSTALL_DIR})
if(MSVC)
  install(FILES "$<TARGET_PDB_FILE_DIR:gammaray_launcher_ui>/$<TARGET_PDB_FILE_NAME:gammaray_launcher_ui>" DESTINATION ${BIN_INSTALL_DIR} CONFIGURATIONS Debug RelWithDebInfo)
endif()
