cmake_minimum_required(VERSION 3.1)

if(MSVC AND NOT "${MSVC_VERSION}" LESS 1400)
	add_definitions( "/MP" )
endif()

project(glew)

add_definitions(
	#-DGLEW_STATIC
	-DVC_EXTRALEAN
	-DGLEW_BUILD
	-DWIN32
	-DWIN32_MEAN_AND_LEAN
	-D_CRT_SECURE_NO_WARNINGS
	#-DGLEW_NO_GLU
)

set(GLEW_SOURCE
	src/glew.c
)

set(GLEW_HEADERS
	include/GL/glew.h
	include/GL/glxew.h
	include/GL/wglew.h
)

add_library( glew SHARED
	${GLEW_SOURCE}
	${GLEW_INCLUDE}
)

include_directories(
	include/
)

target_link_libraries(glew
	${OPENGL_LIBRARY}
	${EXTRA_LIBS}
)

#OPENGL
find_package(OpenGL REQUIRED)
target_link_libraries (glew ${OPENGL_LIBRARY})

install(TARGETS glew
        RUNTIME DESTINATION bin
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib )

install(FILES ${GLEW_HEADERS} DESTINATION include/GL/)
