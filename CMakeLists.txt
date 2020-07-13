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

##################################################################################################################
# HADOUKEN OPTIONS
##################################################################################################################
# Turn on any desired tool/feature below.
##################################################################################################################
# This option controls the hadouken banner printed to stdout on configure.
SET(HADOUKEN_TEST_MISC_NO_HADOUKEN_BANNER   FALSE CACHE BOOL "Enable/disable hadouken banner printing" FORCE)
##################################################################################################################
# This option controls the include-what-you-use integration of the hadouken.
#
# Functionality provided by `.hadouken/toolconf/IncludeWhatYouUse.cmake` module.
# (https://github.com/nettsi/hadouken/blob/master/cmake/modules/toolconf/IncludeWhatYouUse.cmake)
#
# "Include what you use" means this: for every symbol (type, function variable, or macro) that you use in foo.cc, 
# either foo.cc or foo.h should #include a .h file that exports the declaration of that symbol. 
#
# When enabled, hadouken will try to find "iwyu" executable in compilation environment by invoking `find_program()` cmake command. 
# It globally affects the all targets and all source code will be analyzed with include-what-you-use.
# 
# (in future releases, this behavior will be changed and separate .iwyu targets will be created instead.)
#
# 
# For further information about `include-what-you-use (iwyu)`:
# https://include-what-you-use.org/
# https://github.com/include-what-you-use/include-what-you-use
SET(HADOUKEN_TEST_TOOLCONF_USE_IWYU         FALSE CACHE BOOL "Enable/disable include what you use integration" FORCE)
##################################################################################################################
# This option controls the `google test` and `google mock` integration of the hadouken.
#
# Functionality provided by `.hadouken/toolconf/GoogleTest.cmake` module.
# (https://github.com/nettsi/hadouken/blob/master/cmake/modules/toolconf/GoogleTest.cmake)
#
# Google test (gtest) and Google mock (gmock) are open source unit test/mocking libraries developed by Google.
#
# When enabled, hadouken will try to discover gtest/gmock headers and .so/.a files in the 
# compilation environment by invoking `find_package()` cmake command. When found, hadouken will create a new 
# CMake target named `<TOP_LEVEL_PROJECT_NAME>.test`, which makes  gtest/gmock headers and libraries 
# available when linked to another target. This .test target will be automatically injected as LINK dependency
# into `make_target(TYPE UNIT_TEST)` targets.
# 
#
# For further information about `Google Testing Framework (gtest)`:
# https://github.com/google/googletest
# https://github.com/google/googletest/blob/master/googletest/docs/primer.md
#
# For further information about `Google Mocking Framework (gmock)`:
# https://github.com/google/googletest/tree/master/googlemock
# https://chromium.googlesource.com/external/github.com/google/googletest/+/HEAD/googlemock/docs/cook_book.md
SET(HADOUKEN_TEST_TOOLCONF_USE_GOOGLE_TEST  TRUE  CACHE BOOL "Enable/disable gtest/gmock integration" FORCE)
##################################################################################################################
# This option controls the `google benchmark` integration of the hadouken.
#
# Functionality provided by `.hadouken/toolconf/GoogleBenchmark.cmake` module.
# (https://github.com/nettsi/hadouken/blob/master/cmake/modules/toolconf/GoogleBenchmark.cmake)
#
# Google benchmark is an open source micro benchmarking library developed by Google.
#
# When enabled, hadouken will try to discover google benchmark headers and .so/.a files in the 
# compilation environment by invoking `find_package()` cmake command. When found, hadouken will create a new 
# CMake target named `<TOP_LEVEL_PROJECT_NAME>.bechhmark`, which makes  google benchmark headers and libraries 
# available when linked to another target. This .benchmark target will be automatically injected as LINK dependency
# into `make_target(TYPE BENCHMARK)` targets.
# 
# For further information about `google benchmark`:
# https://opensource.google/projects/benchmark
# https://github.com/google/benchmark
# https://github.com/google/benchmark#user-guide
SET(HADOUKEN_TEST_TOOLCONF_USE_GOOGLE_BENCH FALSE CACHE BOOL "Enable/disable google benchmark integration" FORCE)
##################################################################################################################
# This option controls the `clang-format` integration of the hadouken.
#
# Functionality provided by `.hadouken/toolconf/ClangFormat.cmake` module.
# (https://github.com/nettsi/hadouken/blob/master/cmake/modules/toolconf/ClangFormat.cmake)
#
# clang-format is a code format tool for C/C++/Java/JavaScript/Objective-C/Protobuf/C#. 
# It is part of the llvm toolchain and is based on clang.
#
# When enabled, hadouken creates a special `.format` target for each target available in the project.
# For example, this project would have these extra special targets available when enabled:
#
# - hadouken-test.app.format
# - hadouken-test.lib.shared.format
# - hadouken-test.lib.static.format
# - hadouken-test.ut.test_01.format
# - hadouken-test.ut.test_02.format
#
# This special target invokes `clang-format` over the source files of that specific target. 
# clang-format's options are being read from `.clang-format` file in project root. This file is by default symlinked to
# `.clang-format` dotfile shipped with hadouken (under .hadouken/dotfiles/ folder). That `.clang-format` file is defined according
# to "Nettsi code & project standards" and most probably will not suit to your preferences. You can remove that 
# symbolic link file and  define your own `.clang-format` file in project root.
#  
# For further information about `clang-format` (and available linting options):
# https://clang.llvm.org/docs/ClangFormat.html
# https://github.com/llvm/llvm-project/tree/master/clang/tools/clang-format
SET(HADOUKEN_TEST_TOOLCONF_USE_CLANG_FORMAT TRUE CACHE BOOL "Enable/disable clang-format integration" FORCE)
##################################################################################################################
# This option controls the `clang-tidy` integration of the hadouken.
#
# Functionality provided by `.hadouken/toolconf/ClangTidy.cmake` module.
# (https://github.com/nettsi/hadouken/blob/master/cmake/modules/toolconf/ClangTidy.cmake)
#
# clang-tidy is a linter tool for C++. It is part of the llvm toolchain and is based on clang.
#
# When enabled, hadouken creates a special `.tidy` target for each target available in the project.
# For example, this project would have these extra special targets available when enabled:
#
# - hadouken-test.app.tidy
# - hadouken-test.lib.shared.tidy
# - hadouken-test.lib.static.tidy
# - hadouken-test.ut.test_01.tidy
# - hadouken-test.ut.test_02.tidy
#
# This special target invokes `clang-tidy` over the source files of that specific target. 
# clang-tidy's options are being read from `.clang-tidy` file in project root. This file is by default symlinked to
# `.clang-tidy` dotfile shipped with hadouken (under .hadouken/dotfiles/ folder). That `.clang-tidy` file is defined according
# to "Nettsi code & project standards" and most probably will not suit to your preferences. You can remove that 
# symbolic link file and  define your own `.clang-tidy` file in project root.
#  
# For further information about `clang-tidy` (and available linting options):
# https://clang.llvm.org/extra/clang-tidy/
# https://github.com/llvm/llvm-project/tree/master/clang-tools-extra/clang-tidy
SET(HADOUKEN_TEST_TOOLCONF_USE_CLANG_TIDY   TRUE CACHE BOOL "Enable/disable clang-tidy integration" FORCE)
##################################################################################################################
# This option controls the Cppcheck integration of the hadouken.
#
# Functionality provided by `.hadouken/toolconf/Cppcheck.cmake` module.
# (https://github.com/nettsi/hadouken/blob/master/cmake/modules/toolconf/CppCheck.cmake)
#
# Cppcheck is an open source C/C++ static analysis tool.
# When enabled, it globally affects the all targets and all source code will be analyzed with Cppcheck.
# 
# (in future releases, this behavior will be changed and separate .cppcheck targets will be created instead.)
#
# For further information about Cppcheck:
# http://cppcheck.sourceforge.net/
# https://github.com/danmar/cppcheck
SET(HADOUKEN_TEST_TOOLCONF_USE_CPPCHECK     FALSE CACHE BOOL "Enable/disable cppcheck integration" FORCE)
##################################################################################################################
# This option controls the CCache integration of the hadouken.
#
# Functionality provided by `.hadouken/toolconf/CCache.cmake` module.
# (https://github.com/nettsi/hadouken/blob/master/cmake/modules/toolconf/CCache.cmake)
#
# When enabled, all compiler and linker commands are forwarded into CCache executable.
# 
# "It speeds up recompilation by caching previous compilations and detecting 
#  when the same compilation is being done again."
# 
# For further information about CCache:
# https://ccache.dev/
# https://github.com/ccache/ccache
SET(HADOUKEN_TEST_TOOLCONF_USE_CCACHE       FALSE CACHE BOOL "Enable/disable ccache integration" FORCE)
##################################################################################################################
# This option controls the GCOV integration of the hadouken.
#
# Functionality provided by `.hadouken/toolconf/GCov.cmake` and `.hadouken/helper/GCov.cmake` modules.
# (https://github.com/nettsi/hadouken/blob/master/cmake/modules/toolconf/GCov.cmake)
# (https://github.com/nettsi/hadouken/blob/master/cmake/modules/helper/Gcov.cmake)
#
# This integration injects the required instrumentation flags for gcov into the `UNIT_TEST` and `BENCHMARK` type targets
# when `WITH_COVERAGE` parameter is specified.
SET(HADOUKEN_TEST_TOOLCONF_USE_GCOV         TRUE CACHE BOOL "Enable/disable gcov integration" FORCE)
##################################################################################################################
# This option controls the LCOV integration of the hadouken.
#
# Functionality provided by `.hadouken/toolconf/LCov.cmake` module.
# (https://github.com/nettsi/hadouken/blob/master/cmake/modules/toolconf/Lcov.cmake)
#
#
# lcov is a graphical front-end for GCC's coverage testing tool gcov. It collects gcov data for multiple source files 
# and creates HTML pages containing the source code annotated with coverage information. 
# It also adds overview pages for easy navigation within the file structure.
#
# This injects code coverage report generation targets (using lcov) into `UNIT_TEST` and `BENCHMARK` type targets
# when `WITH_COVERAGE` parameter is specified. The created targets will be suffixed with 
# `.lcov` (for html report format). LCOV's options are being read from `.lcovrc` file in project root. 
# This file is by default symlinked to`.lcovrc` dotfile shipped with hadouken (under .hadouken/dotfiles/ folder). 
# That `.lcovrc` file is defined according to "Nettsi code & project standards" and most probably will not suit 
# to your preferences. You can remove that symbolic link file and define your own `.lcovrc` file in project root.
#
# For further information about LCov:
# 
# https://linux.die.net/man/1/lcov
# https://wiki.documentfoundation.org/Development/Lcov
# https://github.com/linux-test-project/lcov
SET(HADOUKEN_TEST_TOOLCONF_USE_LCOV         FALSE CACHE BOOL "Enable/disable lcov integration" FORCE)
##################################################################################################################
# This option controls the GCOVR integration of the hadouken.
#
# Functionality provided by `.hadouken/toolconf/GCovR.cmake` module.
# (https://github.com/nettsi/hadouken/blob/master/cmake/modules/toolconf/GCovR.cmake)
#
# This injects code coverage report generation targets (using gcovr) into `UNIT_TEST` and `BENCHMARK` type targets
# when `WITH_COVERAGE` parameter is specified. The created targets will be suffixed with 
# `.gcovr.html` (for html report format), `.gcovr.xml` (for xml report format).
#
# For further information about GCovR: 
# https://gcovr.com/en/stable/
# https://github.com/gcovr/gcovr
SET(HADOUKEN_TEST_TOOLCONF_USE_GCOVR        TRUE CACHE BOOL "Enable/disable gcovr integration" FORCE)

# Include hadouken
include(.hadouken/hadouken.cmake)

# Print VCS status to stdout
git_print_status()

# Components of the project
add_subdirectory(app)
add_subdirectory(lib)

