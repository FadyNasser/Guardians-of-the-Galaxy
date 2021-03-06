# CMake entry point
cmake_minimum_required (VERSION 2.6)
project (Graphics_Lab4)

find_package(OpenGL REQUIRED)

# Compile external dependencies 
add_subdirectory (external)

# On Visual 2005 and above, this module can set the debug working directory
list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/external/rpavlik-cmake-modules-1c73e35")
include(CreateLaunchers)
include(MSVCMultipleProcessCompile) # /MP

if(INCLUDE_DISTRIB)
	add_subdirectory(distrib)
endif(INCLUDE_DISTRIB)



include_directories(
	external/AntTweakBar-1.16/include/
	external/glfw-2.7.6/include/
	external/glm-0.9.4.0/
	external/glew-1.9.0/include/
	external/assimp-3.0.1270/include/
	external/bullet-2.81-rev2613/src/
	.
)

set(ALL_LIBS
	${OPENGL_LIBRARY}
	GLFW_276
	GLEW_190
)

add_definitions(
	-DTW_STATIC
	-DTW_NO_LIB_PRAGMA
	-DTW_NO_DIRECT3D
	-DGLEW_STATIC
	-D_CRT_SECURE_NO_WARNINGS
)


# Model Loading
add_executable(GuardiansGame
        GuardiansGame/GuardiansGame.cpp
        GuardiansGame/ObjectModel.cpp
        GuardiansGame/ObjectModel.h
        GuardiansGame/GameHeader.h
        GuardiansGame/Buffers.h
        GuardiansGame/Buffers.cpp
        GuardiansGame/Spaceship.h
        GuardiansGame/Spaceship.cpp
	common/shader.cpp
	common/shader.hpp
	common/controls.cpp
	common/controls.hpp
	common/texture.cpp
	common/texture.hpp
	common/objloader.cpp
	common/objloader.hpp
	common/text2D.cpp
	common/text2D.hpp
)
target_link_libraries(GuardiansGame
	${ALL_LIBS}
)
# Xcode and Visual working directories
set_target_properties(GuardiansGame PROPERTIES XCODE_ATTRIBUTE_CONFIGURATION_BUILD_DIR "${CMAKE_CURRENT_SOURCE_DIR}/GuardiansGame/")
create_target_launcher(GuardiansGame WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/GuardiansGame/")




SOURCE_GROUP(common REGULAR_EXPRESSION ".*/common/.*" )


if (NOT ${CMAKE_GENERATOR} MATCHES "Xcode" )


add_custom_command(
   TARGET GuardiansGame POST_BUILD
   COMMAND ${CMAKE_COMMAND} -E copy "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_CFG_INTDIR}/GuardiansGame${CMAKE_EXECUTABLE_SUFFIX}" "${CMAKE_CURRENT_SOURCE_DIR}/GuardiansGame/"
)

elseif (${CMAKE_GENERATOR} MATCHES "Xcode" )

endif (NOT ${CMAKE_GENERATOR} MATCHES "Xcode" )



