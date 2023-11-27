cmake_minimum_required(VERSION 3.27)
project(ExampleMacOS C)

set(CMAKE_C_STANDARD 11)

set(CMAKE_MACOSX_BUNDLE TRUE)

set(MAIN_STORYBOARD "${CMAKE_CURRENT_SOURCE_DIR}/Resources/Base.lproj/Main.storyboard")

set(APP_RESOURCES "$<TARGET_FILE_DIR:${PROJECT_NAME}>/../Resources")

add_executable(${PROJECT_NAME}
               src/main.m
               src/AppDelegate.h
               src/AppDelegate.m
               src/ViewController.h
               src/ViewController.m)

set_target_properties(${PROJECT_NAME} PROPERTIES
                      MACOSX_BUNDLE_INFO_PLIST ${CMAKE_CURRENT_SOURCE_DIR}/plist.in)

target_link_libraries(${PROJECT_NAME} PRIVATE
                      "-framework CoreData"
                      "-framework Security"
                      "-framework Foundation"
                      "-framework CoreFoundation"
                      "-framework AppKit"
                      "-framework Carbon"
                      "-framework CoreServices"
                      "-framework SystemConfiguration"
                      "-framework ServiceManagement"
                      "-framework NetworkExtension"
                      "-framework ApplicationServices"
                      "-lobjc")

add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
                   COMMAND ibtool --compile ${APP_RESOURCES}/Main.storyboardc ${MAIN_STORYBOARD}
                   DEPENDS ${MAIN_STORYBOARD}
                   COMMENT "Compiling MainMenu.nib")
