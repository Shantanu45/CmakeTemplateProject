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

use_props(${PROJECT_NAME} "${CMAKE_CONFIGURATION_TYPES}" "${DEFAULT_CXX_PROPS}")

if(MSVC)
	if("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "x64")
		target_compile_options(${PROJECT_NAME} PRIVATE
			$<$<OR:$<CONFIG:Debug>,$<CONFIG:DebugHeadless>>:
				/Od
			>
			$<$<OR:$<CONFIG:Release>,$<CONFIG:ReleaseHeadless>>:
				/Oi;
				/Gy
			>
			/permissive-;
			/sdl;
			/W3;
			${DEFAULT_CXX_DEBUG_INFORMATION_FORMAT};
			${DEFAULT_CXX_EXCEPTION_HANDLING}
		)
	endif()
	if("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "x64")
		target_link_options(${PROJECT_NAME} PRIVATE
			$<$<OR:$<CONFIG:Release>,$<CONFIG:ReleaseHeadless>>:
				/OPT:REF;
				/OPT:ICF
			>
			/DEBUG;
			/SUBSYSTEM:CONSOLE
		)
	endif()
endif()

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