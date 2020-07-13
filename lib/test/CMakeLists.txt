# ______________________________________________________
# Example application project CMakeLists file
#
# @file        CMakeLists.txt
# @author      Mustafa Kemal GILOR <mgilor@nettsi.com>
# @date        13.07.2020
#
# @copyright   2020 NETTSI Informatics Technology Inc.
#
# Unauthorized copying of this file, via any medium is strictly prohibited
# Proprietary and confidential
# ______________________________________________________

# Here we declare a new project. The `project` directive declares several CMake variables
# which are accessible after the `project` directive. These variables are used for
# some of the hadouken features, such as `no-explicit-name make_target` and `project_metadata_exposure`.
project(hadouken-test.ut LANGUAGES CXX VERSION 0.0.1.0 DESCRIPTION "Example library")

# In this file, several types of unit test declarations with different arguments are illustrated. 
# At its' core, `UNIT_TEST` target is an executable target which is defined by `add_executable(...)` directive
# in the background. But different than a regular EXECUTABLE target type, several different steps are taken:
#
#   1-) The unit test target are automatically linked to the unit test provider target;
#   
#   The unit test provider (*) is defined when an unit test library integration is enabled, e.g. `GOOGLE_TEST`
#   `SET(HADOUKEN_TEST_TOOLCONF_USE_GOOGLE_TEST  TRUE CACHE BOOL "Enable/disable gtest/gmock integration" FORCE)`
#   When an unit test provider target is enabled, a CMake target for that unit test provider is automatically created
#   via gathering its' headers and libraries from the environment. (**) The name for the created target will be 
#   `<TOP_LEVEL_PROJECT_NAME>.test`, which will be `hadouken-test.test` in our case.
#   This target is automatically injected to the created target when make_target type specified as unit test, so no
#   further action is required regarding linking or adding the include path of the unit test provider to the target.
#   
#   2-) The target is registered into CMake's test automation system (CTest) via invoking add_test(...) command;
#   
#   This will make CTest to recognize unit tests, which allows us to run all unit tests defined via `make_target(TYPE UNIT_TEST)` 
#   with a simple invocation of `ctest` command in the build directory. (which ./hadouken --test is defined as)
#   
#   3-) The individual test cases are made visible to CTest via invoking `gtest_discover_tests(...)` command (provided by enable_testing())
#   
#   When add_test() is invoked as-is, CTest is able to execute unit test executables, but cannot see the individual test cases defined by 
#   the developer. In order to provide that, `gtest_discover_tests(...)` is invoked upon the created test target, which will cause CTest
#   to recognize each unit test case individually. Thus, CTest will be able to report each test case result individually.
#
# This allows us to automate test case execution in CI/CD pipelines without any additional efforts. In fact, our in-house CI/CD test stages only
# contain `./hadouken --test` command. Nothing else is required when using `make_target(TYPE UNIT_TEST)`.
#
# (*)   The only unit test provider at the moment is googletest. In future, we are planning to add support for catch2 and boost test.
# (**)  Googletest binaries and headers are included in hadouken docker container by default. If you are not using the container, you must install
#       the googletest binaries into compilation environment you're using.


# Example 01:
# First unit test example, this is the simplest unit test declaration you can make via 
# using make_target() function.
# - The TYPE parameter explicitly specifies this is an unit test
# - SOURCES parameter allows us to explicitly specify which files will be used for compilation (paths are relative to current project directory)
# - SUFFIX parameter adds suffix to the project name before creating the target
# - LINK parameter is used to specify dependencies of this unit test
# - `NO_AUTO_COMPILATION_UNIT` parameter disables source file gathering from `include/` and `src/` directories (which is unnecessary for this scenario)
make_target(
    TYPE UNIT_TEST 
    SOURCES ut_foo_01.cpp 
    SUFFIX .test_01
    LINK hadouken-test.lib.shared
    NO_AUTO_COMPILATION_UNIT
)

# Example 02:
# Unit test with coverage integration.
# - The TYPE parameter explicitly specifies this is an unit test
# - SOURCES parameter allows us to explicitly specify which files will be used for compilation (paths are relative to current project directory)
# - SUFFIX parameter adds suffix to the project name before creating the target
# - LINK parameter is used to specify dependencies of this unit test
# - `NO_AUTO_COMPILATION_UNIT` parameter disables source file gathering from `include/` and `src/` directories (which is unnecessary for this scenario)
# - `WITH_COVERAGE` parameter enables code coverage related instrumentation argument injection to the target. `-fcoverage` (*)
# - `COVERAGE_TARGETS` paramater is used for focusing the coverage scope to specified CMake target. Coverage reports will only 
#    include these target's source.
#
# (*) WITH_COVERAGE requires gcov and (lcov or gcovr) integrations to be enabled.
make_target(
    TYPE UNIT_TEST 
    SOURCES ut_foo_02.cpp 
    SUFFIX .test_02
    LINK hadouken-test.lib.shared
    NO_AUTO_COMPILATION_UNIT 
    WITH_COVERAGE 
    COVERAGE_TARGETS hadouken-test.lib.shared
)