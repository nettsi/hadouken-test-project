# ______________________________________________________
# hadouken-test project top level CMakeLists.txt file
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

cmake_minimum_required(VERSION 3.00)

project(hadouken-test VERSION 0.0.1 LANGUAGES CXX)

# Turn on any desired tool/feature below.
SET(HADOUKEN_TEST_MISC_NO_HADOUKEN_BANNER   FALSE CACHE BOOL "Enable/disable hadouken banner printing" FORCE)
SET(HADOUKEN_TEST_TOOLCONF_USE_IWYU         FALSE CACHE BOOL "Enable/disable include what you use integration" FORCE)
SET(HADOUKEN_TEST_TOOLCONF_USE_GOOGLE_TEST  FALSE CACHE BOOL "Enable/disable gtest/gmock integration" FORCE)
SET(HADOUKEN_TEST_TOOLCONF_USE_GOOGLE_BENCH FALSE CACHE BOOL "Enable/disable google benchmark integration" FORCE)
SET(HADOUKEN_TEST_TOOLCONF_USE_CLANG_FORMAT FALSE CACHE BOOL "Enable/disable clang-format integration" FORCE)
SET(HADOUKEN_TEST_TOOLCONF_USE_CLANG_TIDY   FALSE CACHE BOOL "Enable/disable clang-tidy integration" FORCE)
SET(HADOUKEN_TEST_TOOLCONF_USE_CPPCHECK     FALSE CACHE BOOL "Enable/disable cppcheck integration" FORCE)
SET(HADOUKEN_TEST_TOOLCONF_USE_CCACHE       FALSE CACHE BOOL "Enable/disable ccache integration" FORCE)
SET(HADOUKEN_TEST_TOOLCONF_USE_GCOV         FALSE CACHE BOOL "Enable/disable gcov integration" FORCE)
SET(HADOUKEN_TEST_TOOLCONF_USE_LCOV         FALSE CACHE BOOL "Enable/disable lcov integration" FORCE)
SET(HADOUKEN_TEST_TOOLCONF_USE_GCOVR        FALSE CACHE BOOL "Enable/disable gcovr integration" FORCE)

# Include hadouken
include(.hadouken/hadouken.cmake)

# Print VCS status to stdout
git_print_status()

# Components of the project
add_subdirectory(app)

