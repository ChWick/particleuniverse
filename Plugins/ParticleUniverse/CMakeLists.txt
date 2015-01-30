# The project's include directories
include_directories(include)

set (PARTICLEUNIVERSE_PLUGIN_NAME "Plugin_ParticleUniverse")

# The project's sources
file(GLOB PROJECT_SOURCES
	src/*.cpp
	src/*/*.cpp
)

# The project's includes
file(GLOB PROJECT_INCLUDES
        include/*.h
)
file(GLOB PROJECT_INCLUDES_EXTERNS
        include/Externs/*.h
)
file(GLOB PROJECT_INCLUDES_PARTICLE_AFFECTORS
        include/ParticleAffectors/*.h
)
file(GLOB PROJECT_INCLUDES_PARTICLE_BEHAVIOURS
        include/ParticleBehaviours/*.h
)
file(GLOB PROJECT_INCLUDES_PARTICLE_EMITTERS
        include/ParticleEmitters/*.h
)
file(GLOB PROJECT_INCLUDES_PARTICLE_EVENT_HANDLERS
        include/ParticleEventHandlers/*.h
)
file(GLOB PROJECT_INCLUDES_PARTICLE_OBSERVERS
        include/ParticleObservers/*.h
)
file(GLOB PROJECT_INCLUDES_PARTICLE_RENDERERS
        include/ParticleRenderers/*.h
)

set (PARTICLEUNIVERSE_INCLUDE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/include PARENT_SCOPE)
set (PARTICLEUNIVERSE_LIBRARIES ${PARTICLEUNIVERSE_PLUGIN_NAME} PARENT_SCOPE)

add_definitions(-DPARTICLE_UNIVERSE_EXPORTS)

add_library(${PARTICLEUNIVERSE_PLUGIN_NAME} SHARED ${PROJECT_SOURCES})
target_link_libraries(${PARTICLEUNIVERSE_PLUGIN_NAME} ${LIBS})
set_target_properties(${PARTICLEUNIVERSE_PLUGIN_NAME} PROPERTIES VERSION "1.9.0" SOVERSION "1.9.0" PREFIX "")

# Make install target
# framework on mac

if (APPLE)
  set_target_properties(${PARTICLEUNIVERSE_PLUGIN_NAME} PROPERTIES FRAMEWORK TRUE)
  set_target_properties(${PARTICLEUNIVERSE_PLUGIN_NAME} PROPERTIES OUTPUT_NAME ${PARTICLEUNIVERSE_PLUGIN_NAME})
  set_target_properties(${PARTICLEUNIVERSE_PLUGIN_NAME} PROPERTIES BUILD_WITH_INSTALL_RPATH 1
    INSTALL_NAME_DIR "@executable_path/../Frameworks")

  install(TARGETS ${PARTICLEUNIVERSE_PLUGIN_NAME}
     FRAMEWORK DESTINATION "Framework")
else()

  # classic install

  install(TARGETS ${PARTICLEUNIVERSE_PLUGIN_NAME}
     LIBRARY DESTINATION "lib/ParticleUniverse"
     COMPONENT libraries)

  install(FILES ${PROJECT_INCLUDES}
     DESTINATION "include/ParticleUniverse"
     COMPONENT headers)
  install(FILES ${PROJECT_INCLUDES_EXTERNS}
     DESTINATION "include/ParticleUniverse/Externs"
     COMPONENT headers)
  install(FILES ${PROJECT_INCLUDES_PARTICLE_AFFECTORS}
     DESTINATION "include/ParticleUniverse/ParticleAffectors"
     COMPONENT headers)
  install(FILES ${PROJECT_INCLUDES_PARTICLE_BEHAVIOURS}
     DESTINATION "include/ParticleUniverse/ParticleBehaviours"
     COMPONENT headers)
  install(FILES ${PROJECT_INCLUDES_PARTICLE_EMITTERS}
     DESTINATION "include/ParticleUniverse/ParticleEmitters"
     COMPONENT headers)
  install(FILES ${PROJECT_INCLUDES_PARTICLE_EVENT_HANDLERS}
     DESTINATION "include/ParticleUniverse/ParticleEventHandlers"
     COMPONENT headers)
  install(FILES ${PROJECT_INCLUDES_PARTICLE_OBSERVERS}
     DESTINATION "include/ParticleUniverse/ParticleObservers"
     COMPONENT headers)
  install(FILES ${PROJECT_INCLUDES_PARTICLE_RENDERERS}
     DESTINATION "include/ParticleUniverse/ParticleRenderers"
     COMPONENT headers)

  if (UNIX)
    if (APPLE)
    else()
      # on linux
      if (OGRE_FOUND)
        # copy on install aswell in the ogre directory to keep plugins together
        if (CMAKE_BUILD_TYPE EQUAL "Debug")
  	install(TARGETS ${PARTICLEUNIVERSE_PLUGIN_NAME}
  	  LIBRARY DESTINATION ${OGRE_PLUGIN_DIR_DBG}
  	  COMPONENT libraries)
        else()
  	install(TARGETS ${PARTICLEUNIVERSE_PLUGIN_NAME}
  	  LIBRARY DESTINATION ${OGRE_PLUGIN_DIR_REL}
  	  COMPONENT libraries)
        endif()
      endif()
    endif()
  endif()
endif()
