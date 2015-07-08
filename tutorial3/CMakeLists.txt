# Our minimum cmake list

# Setup Project
cmake_minimum_required(VERSION 2.6 FATAL_ERROR)
project(tutorial2)

# Find Geant4 and activate ALL UI and vis drivers by default
option(WITH_GEANT4_UIVIS "Build example with Geant4 UI and Vis drivers" ON)
if(WITH_GEANT4_UIVIS)
  find_package(Geant4 REQUIRED ui_all vis_all)
else()
  find_package(Geant4 REQUIRED)
endif()

# Setup include directories
include(${Geant4_USE_FILE})
include_directories(${PROJECT_SOURCE_DIR}/include)

# Locate the source and header files
file(GLOB sources ${PROJECT_SOURCE_DIR}/src/*.cpp)
file(GLOB headers ${PROJECT_SOURCE_DIR}/include/*.hh)

# Add exec and links the libraries
add_executable(tutorial2 tutorial2.cpp ${sources} ${headers})
target_link_libraries(tutorial2 ${Geant4_LIBRARIES})

# if you want to copy scripts over uncomment this section
 set(TUTORIAL_SCRIPTS
#   tutorial.in
#   tutorial.out
   init_vis.mac
#   run1.mac
#   run2.mac
   vis.mac
   )
# 
foreach(_script ${TUTORIAL_SCRIPTS})
  configure_file(
    ${PROJECT_SOURCE_DIR}/${_script}
    ${PROJECT_BINARY_DIR}/${_script}
    COPYONLY
    )
endforeach()

# Install exec's to bin directory under CMAKE_INSTALL_PREFIX
install(TARGETS tutorial2 DESTINATION bin)