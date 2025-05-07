cmake_minimum_required(VERSION 3.25)

option(THREADPOOL_INSTALL "install thread pool(usefull for externalProject_add" OFF)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)
project(BSThreadPool LANGUAGES CXX)

include(GNUInstallDirs)

set(CURRENT_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/include)
cmake_path(NORMAL_PATH CURRENT_SOURCE_DIR)

set(EXPORT_HEADERS
    ${CURRENT_SOURCE_DIR}/BS_thread_pool.hpp)

add_library(ThreadPool INTERFACE)
target_sources(ThreadPool PUBLIC FILE_SET HEADERS BASE_DIRS ${CURRENT_SOURCE_DIR} FILES ${EXPORT_HEADERS})

# Install Config
if(THREADPOOL_INSTALL)

    install(TARGETS ThreadPool
            EXPORT ThreadPoolTargets
            DESTINATION ${CMAKE_INSTALL_LIBDIR}
            FILE_SET HEADERS
            DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/BS)
    
    include(CMakePackageConfigHelpers)
    set(THREADPOOL_CONFIG_DIR ${CMAKE_INSTALL_LIBDIR}/cmake/BS)

    # generate the config file that includes the exports
    configure_package_config_file(${CMAKE_CURRENT_SOURCE_DIR}/ThreadPoolConfig.cmake.in
            "${CMAKE_CURRENT_BINARY_DIR}/ThreadPoolConfig.cmake"
            INSTALL_DESTINATION ${THREADPOOL_CONFIG_DIR}
            NO_CHECK_REQUIRED_COMPONENTS_MACRO
    )

    install(FILES "${CMAKE_CURRENT_BINARY_DIR}/ThreadPoolConfig.cmake"
            DESTINATION ${THREADPOOL_CONFIG_DIR})

    install(EXPORT ThreadPoolTargets
            FILE ThreadPoolTargets.cmake
            DESTINATION ${THREADPOOL_CONFIG_DIR})

endif()
