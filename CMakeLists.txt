cmake_minimum_required(VERSION 3.15)
project(AB_count)

find_package(PkgConfig REQUIRED)
pkg_check_modules(ALPACA REQUIRED alpaca)
pkg_check_modules(BANDICOOT REQUIRED bandicoot)

add_library(abcount SHARED ab_count.h ab_count.cpp)
target_compile_options(abcount PUBLIC ${ALPACA_CFLAGS_OTHER} ${BANDICOOT_CFLAGS_OTHER})
target_include_directories(abcount PUBLIC ${CMAKE_CURRENT_SOURCE_DIR} ${ALPACA_INCLUDE_DIRS} ${BANDICOOT_INCLUDE_DIRS})

set_target_properties(abcount PROPERTIES SOVERSION 1.0.0)
set_target_properties(abcount PROPERTIES PUBLIC_HEADER ab_count.h)

configure_file(abcount.pc.in abcount.pc @ONLY)

add_executable(abcountDemo main.cpp)
target_include_directories(abcountDemo PUBLIC ${ALPACA_INCLUDE_DIRS} ${BANDICOOT_INCLUDE_DIRS})
target_link_libraries(abcountDemo LINK_PUBLIC abcount ${ALPACA_LIBRARIES} ${BANDICOOT_LIBRARIES})

include(GNUInstallDirs)
install(TARGETS abcount abcountDemo
	RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
install(FILES ${CMAKE_BINARY_DIR}/abcount.pc DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/pkgconfig)
