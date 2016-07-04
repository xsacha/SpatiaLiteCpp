
ENABLE_TESTING()

FIND_PACKAGE(Threads REQUIRED)

# ======================================================================
# Enable ExternalProject CMake module
# ----------------------------------------------------------------------
INCLUDE(ExternalProject)

# ======================================================================
# Download and install GoogleTest
# ----------------------------------------------------------------------
ExternalProject_Add(
    gtest
    URL https://googletest.googlecode.com/files/gtest-1.7.0.zip
    PREFIX ${CMAKE_CURRENT_BINARY_DIR}/gtest
    # Disable install step
    INSTALL_COMMAND ""
)

# ======================================================================
# Create a libgtest target to be used as a dependency by test programs
# ----------------------------------------------------------------------
ADD_LIBRARY(libgtest IMPORTED STATIC GLOBAL)
ADD_DEPENDENCIES(libgtest gtest)

# ======================================================================
# Set gtest properties
# ----------------------------------------------------------------------

ExternalProject_Get_Property(gtest source_dir binary_dir)
IF(WIN32)
    SET_TARGET_PROPERTIES(libgtest PROPERTIES
        "IMPORTED_LOCATION_DEBUG" "${binary_dir}/Debug/gtest.lib"
        "IMPORTED_LOCATION_RELEASE" "${binary_dir}/Release/gtest.lib"
        "IMPORTED_LINK_INTERFACE_LIBRARIES" "${CMAKE_THREAD_LIBS_INIT}")
ELSE()
    SET_TARGET_PROPERTIES(libgtest PROPERTIES
        "IMPORTED_LOCATION" "${binary_dir}/libgtest.a"
        "IMPORTED_LINK_INTERFACE_LIBRARIES" "${CMAKE_THREAD_LIBS_INIT}")
ENDIF()
INCLUDE_DIRECTORIES("${source_dir}/include")

# ======================================================================
# Download and install GoogleMock
# ----------------------------------------------------------------------
ExternalProject_Add(
    gmock
    URL https://googlemock.googlecode.com/files/gmock-1.7.0.zip
    PREFIX ${CMAKE_CURRENT_BINARY_DIR}/gmock
    # Disable install step
    INSTALL_COMMAND ""
)

# ======================================================================
# Create a libgmock target to be used as a dependency by test programs
# ----------------------------------------------------------------------
ADD_LIBRARY(libgmock IMPORTED STATIC GLOBAL)
ADD_DEPENDENCIES(libgmock gmock)

# ======================================================================
# Set gmock properties
# ----------------------------------------------------------------------

ExternalProject_Get_Property(gmock source_dir binary_dir)
IF(WIN32)
    SET_TARGET_PROPERTIES(libgmock PROPERTIES
        "IMPORTED_LOCATION_DEBUG" "${binary_dir}/Debug/gmock.lib"
        "IMPORTED_LOCATION_RELEASE" "${binary_dir}/Release/gmock.lib"
        "IMPORTED_LINK_INTERFACE_LIBRARIES" "${CMAKE_THREAD_LIBS_INIT}")
ELSE()
    SET_TARGET_PROPERTIES(libgmock PROPERTIES
        "IMPORTED_LOCATION" "${binary_dir}/libgmock.a"
        "IMPORTED_LINK_INTERFACE_LIBRARIES" "${CMAKE_THREAD_LIBS_INIT}")
ENDIF()
INCLUDE_DIRECTORIES("${source_dir}/include")

# ======================================================================
# Add test directory
# ----------------------------------------------------------------------
ADD_SUBDIRECTORY(SpatiaLiteCpp)
