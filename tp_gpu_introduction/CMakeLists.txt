cmake_minimum_required(VERSION 3.10)

project(openGL_CPE LANGUAGES C CXX )

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

find_package(OpenGL REQUIRED)
if(NOT ${OPENGL_FOUND})
    message("OPENGL not found")
endif()

file(GLOB PROJECT_SOURCE_FILES "*.cpp")
file(GLOB PROJECT_HEADER_FILES "*.h")

message("${PROJECT_SOURCE_FILES}")
message("${PROJECT_HEADER_FILES}")


find_package(GLUT)
if(NOT ${GLUT_FOUND})

  message(WARNING "Glut not found, manual compilation of Glut added to the project")

  option(FREEGLUT_BUILD_SHARED_LIBS "Build FreeGLUT shared library." OFF)
  option(FREEGLUT_BUILD_DEMOS "Build FreeGLUT demos." OFF )
  option(FREEGLUT_PRINT_ERRORS "Lib prints errors to stderr" OFF)
  option(FREEGLUT_PRINT_WARNINGS "Lib prints warnings to stderr" OFF)
  option(INSTALL_PDB "Also install .pdb files" OFF)

  add_subdirectory(extern/freeglut)
  include_directories(${CMAKE_SOURCE_DIR}/extern/freeglut/include)
  set(USED_GLUT_LIBRARY freeglut_static)
else()
  include_directories(${GLUT_INCLUDE_DIRS})
  set(USED_GLUT_LIBRARY ${GLUT_LIBRARIES})
endif()

find_package(GLEW)
if(NOT ${GLEW_FOUND})
  option(glew-cmake_BUILD_SHARED "Build the shared glew library" OFF)
  option(ONLY_LIBS "Do not build executables" ON)

  add_subdirectory(extern/glew)
  include_directories(${PROJECT_PATH}/extern/glew/include)
  set(USED_GLEW_LIBRARY libglew_static)
else()
  include_directories(${GLEW_INCLUDE_DIRS})
  set(USED_GLEW_LIBRARY ${GLEW_LIBRARIES})
endif()

include_directories(${OPENGL_INCLUDE_DIRS})

#set(SIMULATION_LINK_LIBRARIES freeglut_static ${OPENGL_INCLUDE_DIRS} libglew_static)
#set(SIMULATION_DEPENDENCIES freeglut glew)

add_executable(opengl ${PROJECT_SOURCE_FILES} ${PROJECT_HEADER_FILES})
target_link_libraries(opengl ${OPENGL_LIBRARIES} ${USED_GLUT_LIBRARY} ${USED_GLEW_LIBRARY})



