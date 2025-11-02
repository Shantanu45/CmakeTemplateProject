set(PROJECT_NAME Main)

set(UNGROUPED_FILES
	main.cpp
)

source_group("" FILES ${UNGROUPED_FILES})


set(ALL_FILES
	${UNGROUPED_FILES}
)

add_executable(${PROJECT_NAME}
	${ALL_FILES}
)

setup_msvc_options(${PROJECT_NAME})

set(INCLUDES 
"${PROJECT_SOURCE_DIR}/src"
"${CMAKE_BINARY_DIR}/_deps/googletest-src/googletest/include"
)

target_include_directories(${PROJECT_NAME} PRIVATE ${INCLUDES})

set(ADDITIONAL_LIBRARY_DEPENDENCIES
	spdlog::spdlog
)

target_link_libraries(${PROJECT_NAME} PRIVATE "${ADDITIONAL_LIBRARY_DEPENDENCIES}")


target_compile_options(spdlog PRIVATE /EHsc)