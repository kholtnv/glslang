if (TARGET gmock)
  message(STATUS "Google Mock found - building tests")

  set(TEST_SOURCES
    # Framework related source files
    ${CMAKE_CURRENT_SOURCE_DIR}/Initializer.h
    ${CMAKE_CURRENT_SOURCE_DIR}/main.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/Settings.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/Settings.h
    ${CMAKE_CURRENT_SOURCE_DIR}/TestFixture.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/TestFixture.h

    # Test related source files
    ${CMAKE_CURRENT_SOURCE_DIR}/AST.FromFile.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/BuiltInResource.FromFile.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/Pp.FromFile.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/Spv.FromFile.cpp
  )

  add_executable(glslangtests ${TEST_SOURCES})
  target_compile_definitions(glslangtests
    PRIVATE GLSLANG_TEST_DIRECTORY="${CMAKE_CURRENT_SOURCE_DIR}/../Test")
  target_include_directories(glslangtests PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${PROJECT_SOURCE_DIR}
    ${gmock_SOURCE_DIR}/include
    ${gtest_SOURCE_DIR}/include)
  target_link_libraries(glslangtests PRIVATE
    glslang OSDependent OGLCompiler HLSL glslang
    SPIRV glslang-default-resource-limits gmock)
  add_test(NAME glslang-gtests COMMAND glslangtests)
endif()
