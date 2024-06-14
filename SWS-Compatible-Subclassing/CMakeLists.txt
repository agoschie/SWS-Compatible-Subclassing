# CMakeList.txt : CMake project for SWS-Compatible-Subclassing, include source and define
# project specific logic here.
#

# Add source to this project's executable.
add_executable (SWS-Compatible-Subclassing "SWS-Compatible-Subclassing.cpp" "SWS-Compatible-Subclassing.h")

if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET SWS-Compatible-Subclassing PROPERTY CXX_STANDARD 20)
endif()

# TODO: Add tests and install targets if needed.
