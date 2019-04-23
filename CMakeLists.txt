cmake_minimum_required(VERSION 3.1)

project(glew)

find_package(OpenGL REQUIRED)

set(GLEW_SOURCE
    src/glew.c
)

set(GLEW_HEADERS
    include/GL/eglew.h
    include/GL/glew.h
    include/GL/glxew.h
    include/GL/wglew.h
)

add_library(${PROJECT_NAME} SHARED
    ${GLEW_SOURCE}
    ${GLEW_INCLUDE}
)
add_library(::${PROJECT_NAME} ALIAS ${PROJECT_NAME})

target_compile_definitions(${PROJECT_NAME} PRIVATE
    #-DGLEW_STATIC
    -DVC_EXTRALEAN
    -DGLEW_BUILD
    -DWIN32
    -DWIN32_MEAN_AND_LEAN
    -D_CRT_SECURE_NO_WARNINGS
    #-DGLEW_NO_GLU
)

target_include_directories(${PROJECT_NAME}
    PUBLIC $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/gen>
    PUBLIC $<INSTALL_INTERFACE:include>
)

target_link_libraries(${PROJECT_NAME} PRIVATE ${OPENGL_LIBRARY} ${EXTRA_LIBS})

configure_file(${PROJECT_NAME}-config.cmake.in ${PROJECT_NAME}-config.cmake @ONLY)
install(FILES ${CMAKE_BINARY_DIR}/${PROJECT_NAME}-config.cmake DESTINATION lib/cmake/${PROJECT_NAME})
install(TARGETS ${PROJECT_NAME} EXPORT ${PROJECT_NAME}-targets DESTINATION lib)
install(DIRECTORY include DESTINATION .)
install(EXPORT ${PROJECT_NAME}-targets NAMESPACE :: DESTINATION lib/cmake/${PROJECT_NAME})
