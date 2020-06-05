add_library("${PROJECT_NAME}" "")
add_library(Foo::foo ALIAS "${PROJECT_NAME}")

#private--build requirement(compile), interface--usage requirement(link), public--private+interface
target_include_directories("${PROJECT_NAME}" PUBLIC	
	"$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
	"$<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>" #foo_export
	"$<INSTALL_INTERFACE:include>"	#${CMAKE_INSTALL_PREFIX}/include	
)

if(NOT BUILD_SHARED_LIBS)
	set(STATIC_POSTFIX "-s")
	set_target_properties("${PROJECT_NAME}" PROPERTIES
	COMPLETE_FLAGS -DFOO_STATIC_DEFINE)
endif()

#rename target Foo
set_target_properties("${PROJECT_NAME}" PROPERTIES
	DEBUG_POSTFIX "${STATIC_POSTFIX}-d"
	RELEASE_POSTFIX "${STATIC_POSTFIX}"
	MINSIZEREL_POSTFIX "${STATIC_POSTFIX}-mr"
	RELEASEWITHDEBINFO_POSTFIX "${STATIC_POSTFIX}-rd"
)

include("GenerateExportHeader")
generate_export_header("${PROJECT_NAME}")
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/foo.export.h" DESTINATION "include")

add_subdirectory("include")
add_subdirectory("src")