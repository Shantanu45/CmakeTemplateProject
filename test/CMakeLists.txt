set(TEST_PROJECT_NAME Test)

set(UNGROUPED_FILES
	"main.cpp"
)

source_group("" FILES ${UNGROUPED_FILES})

set(TESTS
	"SampleTest.cpp"
)

source_group("tests" FILES ${TESTS})

include(FetchContent)
FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/52eb8108c5bdec04579160ae17225d66034bd723.zip
  DOWNLOAD_EXTRACT_TIMESTAMP TRUE
)
# For Windows: Prevent overriding the parent project's compiler/linker settings
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

if(MSVC)
  set_target_properties(gtest         PROPERTIES FOLDER "ThirdParty/GTest")
  set_target_properties(gtest_main    PROPERTIES FOLDER "ThirdParty/GTest")
  set_target_properties(gmock         PROPERTIES FOLDER "ThirdParty/GTest")
  set_target_properties(gmock_main    PROPERTIES FOLDER "ThirdParty/GTest")
endif()


set(ALL_FILES
	${UNGROUPED_FILES}
	${TESTS}
)

add_executable(${TEST_PROJECT_NAME}
	${ALL_FILES}
)

setup_msvc_options(${TEST_PROJECT_NAME})

set(TEST_INCLUDE 
"${PROJECT_SOURCE_DIR}/src"
)

target_compile_definitions(${TEST_PROJECT_NAME} PRIVATE ENABLED_GC_TEST)

# Link
set(ADDITIONAL_LIBRARY_DEPENDENCIES
	gtest_main
	spdlog::spdlog
)

target_link_libraries(${TEST_PROJECT_NAME} PUBLIC ${ADDITIONAL_LIBRARY_DEPENDENCIES})

target_include_directories(${TEST_PROJECT_NAME} PRIVATE ${TEST_INCLUDE})

# Add test to CTest
include(GoogleTest)
gtest_discover_tests(${TEST_PROJECT_NAME})
