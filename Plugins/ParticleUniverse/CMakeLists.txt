# The project's include directories
include_directories(include)

set (PARTICLEUNIVERSE_PLUGIN_NAME "Plugin_ParticleUniverse")

# The project's sources
file(GLOB PROJECT_SOURCES
	src/*.cpp
	src/*/*.cpp
)

# The project's includes
file(GLOB_RECURSE PROJECT_INCLUDES
        include/*.h
)

set (PARTICLEUNIVERSE_INCLUDE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/include PARENT_SCOPE)
set (PARTICLEUNIVERSE_LIBRARIES ${PARTICLEUNIVERSE_PLUGIN_NAME} PARENT_SCOPE)

add_definitions(-DPARTICLE_UNIVERSE_EXPORTS)

add_library(${PARTICLEUNIVERSE_PLUGIN_NAME} SHARED ${PROJECT_SOURCES})
target_link_libraries(${PARTICLEUNIVERSE_PLUGIN_NAME} ${LIBS})
set_target_properties(${PARTICLEUNIVERSE_PLUGIN_NAME} PROPERTIES VERSION "1.9.0" SOVERSION "1.9.0" PREFIX "")

# Make install target
install(TARGETS ${PARTICLEUNIVERSE_PLUGIN_NAME}
   LIBRARY DESTINATION "lib/ParticleUniverse"
   COMPONENT libraries)

install(FILES ${PROJECT_INCLUDES}
   DESTINATION "include/ParticleUniverse"
   COMPONENT headers)