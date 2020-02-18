cmake_minimum_required(VERSION 3.12)

project(rec02)

LINK_DIRECTORIES( ${CMAKE_BINARY_DIR}/lib )
LINK_DIRECTORIES( ${CMAKE_BINARY_DIR}/bin )

set (CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# For multi-config builds (e.g. msvc)
foreach( OUTPUTCONFIG ${CMAKE_CONFIGURATION_TYPES} )
    string( TOUPPER ${OUTPUTCONFIG} OUTPUTCONFIG )
    set( CMAKE_RUNTIME_OUTPUT_DIRECTORY_${OUTPUTCONFIG} ${CMAKE_BINARY_DIR}/bin )
    set( CMAKE_LIBRARY_OUTPUT_DIRECTORY_${OUTPUTCONFIG} ${CMAKE_BINARY_DIR}/lib )
    set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY_${OUTPUTCONFIG} ${CMAKE_BINARY_DIR}/lib )
endforeach( OUTPUTCONFIG CMAKE_CONFIGURATION_TYPES )

find_package(OpenGL REQUIRED )
if (OpenGL_FOUND)
    include_directories(${OPENGL_INCLUDE_DIRS})
    link_libraries(${OpenGL_LIBRARIES})
endif()	

find_package(GLEW REQUIRED)
if (GLEW_FOUND)
    include_directories(${GLEW_INCLUDE_DIRS})
    link_libraries(${GLEW_LIBRARIES})
endif()

find_package(GLUT REQUIRED)
if (GLUT_FOUND)
	include_directories( ${GLUT_INCLUDE_DIRS} )
	link_libraries(${GLUT_LIBRARY})
endif()

find_package(GLFW REQUIRED)
include_directories(${GLFW_INCLUDE_DIR})
link_libraries(${GLFW_LIBRARIES})

find_package(GLAD REQUIRED)
include_directories(${GLAD_INCLUDE_PATH})
link_libraries(${GLAD_LIBRARIES})

set(COMMON_LIBS optimized ${OpenGL_LIBRARIES} ${GLUT_LIBRARY} ${GLEW_LIBRARIES} ${GLFW_LIBRARIES} ${GLAD_LIBRARIES})
set(COMMON_LIBS ${COMMON_LIBS} ${EXTRA_LIBS})

find_package(OpenMP)
if (OPENMP_FOUND)
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif()

SET(APP_SRCS1
  source/Question1.cpp
)
SET(APP_SRCS2
  source/Question2.cpp
)
SET(APP_COMMON
  common/loadShaders.cpp
)

SET(APP_HDRS
	include/mat.h
	include/vec.h
	include/loadShaders.h
)

SET(APP_SHADERS1
	shader/triangles1.vert
	shader/triangles1.frag
	shader/triangles2.vert
	shader/triangles2.frag
	shader/triangles1q2.vert
	shader/triangles1q2.frag
	shader/triangles2q2.vert
	shader/triangles2q2.frag
	shader/triangles3.vert
	shader/triangles3.frag
)

SOURCE_GROUP("Common Files" FILES
  ${APP_COMMON}
)

SOURCE_GROUP("Shader Files" FILES
 ${APP_SHADERS1}
)

add_executable(Question1  ${APP_SRCS1} ${APP_COMMON}  ${APP_HDRS}  ${APP_SHADERS1})
target_link_libraries(Question1  ${COMMON_LIBS})

add_executable(Question2  ${APP_SRCS2} ${APP_COMMON}  ${APP_HDRS}  ${APP_SHADERS1})
target_link_libraries(Question2  ${COMMON_LIBS})

include_directories( include )

ADD_CUSTOM_TARGET(debug ${CMAKE_COMMAND} -DCMAKE_BUILD_TYPE:STRING=Debug ${project_binary_dir})
ADD_CUSTOM_TARGET(release ${CMAKE_COMMAND} -DCMAKE_BUILD_TYPE:STRING=Release ${project_binary_dir})






