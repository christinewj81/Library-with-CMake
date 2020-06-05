set(FILES "${CMAKE_CURRENT_SOURCE_DIR}/Hello.h")
set(SUBDIRECTORIES "")
set(RELATIVE_PATH "include/foo")

#add all subdirectories
foreach(VAR ${SUBDIRECTORIES})
	add_subdirectory(${VAR})
endforeach()

if(FILES)
	#add source files to target
	target_sources("${PROJECT_NAME}" PRIVATE ${FILES})
	
	#install our source files
	install(FILES ${FILES} DESTINATION ${RELATIVE_PATH})
endif()