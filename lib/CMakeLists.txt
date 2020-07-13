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
project(hadouken-test.lib LANGUAGES CXX VERSION 0.0.1.0 DESCRIPTION "Example library")

# The source code and headers for both targets will be automatically gathered from
# `include/` and `src/` folders inside <CURRENT_PROJECT_DIRECTORY>.
# `Current project directory` is the directory which this file exist in, (<PROJECT_ROOT>/lib)
# It is obtained from ${PROJECT_SOURCE_DIR} parameter, which is defined by CMake upon 
# the invocation of project() directive. The variable contains the values of last project directive 
# in any given time.

# In our example, the last project() directive is `project(hadouken-test.lib)`, hence all CMake variables
# starting with `${PROJECT_*}` are set by this. If we omit this directive (comment out, remove), 
# the last project directive in parent CMakeLists file is considered. 
# Keep in mind that `project` directives in subdirectories, (via `add_subdirectory`) does not affect/override
# the project() directive made in that CMakeLists file.

# For more details, please refer to official CMake documentation about project() directive.
# https://cmake.org/cmake/help/latest/command/project.html


# Create a static library target, which has suffix of .static
# Name for this target will be <project-name> + <suffix> : `hadouken-test.lib.static`.
make_target(TYPE STATIC SUFFIX .static)

# Create a shared library target, which has suffix of .shared
# Name for this target will be <project-name> + <suffix> : `hadouken-test.lib.shared`.
make_target(TYPE SHARED SUFFIX .shared)

# Add unit test directory to project
add_subdirectory(test)
