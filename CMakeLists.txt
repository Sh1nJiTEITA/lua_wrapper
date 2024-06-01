CMAKE_MINIMUM_REQUIRED(VERSION 3.29 FATAL_ERROR)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON CACHE INTERNAL "")
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

set(PROJECT_NAME lua_wrapper)
set(PROJECT_VERSION 0.1)


project(${PROJECT_NAME})

include_directories(${PROJECT_SOURCE_DIR}/include)

add_library(${PROJECT_NAME} STATIC src/lua_wrapper.cpp)

include(GNUInstallDirs)

install(TARGETS ${PROJECT_NAME}
	EXPORT ${PROJECT_NAME}
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})

install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

install(EXPORT ${PROJECT_NAME}
	FILE ${PROJECT_NAME}.cmake
	NAMESPACE ${PROJECT_NAME}::
	DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})

include(CMakePackageConfigHelpers)

write_basic_package_version_file(
	${CMAKE_CURRENT_BINARY_DIR}/lua_wrapper_config_version.cmake
	VERSION patch#${PROJECT_VERSION}
	COMPATIBILITY AnyNewerVersion 
)

configure_package_config_file(
	${CMAKE_CURRENT_SOURCE_DIR}/cmake/lua_wrapper_config.cmake.in
	${CMAKE_CURRENT_BINARY_DIR}/lua_wrapper_config.cmake
	INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
)

install(FILES
	${CMAKE_CURRENT_BINARY_DIR}/lua_wrapper_config.cmake
	${CMAKE_CURRENT_BINARY_DIR}/lua_wrapper_config_version.cmake
	DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/lua_wrapper
)

# Lua-Scripts
file(COPY ${CMAKE_SOURCE_DIR}/lua-scripts
     DESTINATION ${CMAKE_BINARY_DIR}/)
