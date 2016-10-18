cmake_minimum_required(VERSION 2.8.3)
# the project name should match that of the external project (kinda sketchy)
project(catch)

find_package(catkin REQUIRED)

include(ExternalProject)

# the name here doesn't really matter, but it's used further below
set(EP_NAME Catch)
ExternalProject_Add(${EP_NAME}
  # prefix for the external project in the build folder
  PREFIX external
  # the source
  URL https://github.com/utiasASRL/Catch/archive/feature/stl-container-approx.zip
  # Install to devel
  INSTALL_DIR ${CATKIN_DEVEL_PREFIX}/catch
  # don't update, we lock the version
  UPDATE_COMMAND ""
  # don't configure, it's header only
  CONFIGURE_COMMAND ""
  # don't build, it's header only
  BUILD_COMMAND ""
  # catkin doesn't want autogen includes in build, it wants them in devel
  INSTALL_COMMAND rsync -r <SOURCE_DIR>/include <INSTALL_DIR>
)


# let catkin install the include directory
ExternalProject_Get_Property(${EP_NAME} INSTALL_DIR)
# we have to make it so it exists for catkin at generation time
file(MAKE_DIRECTORY ${INSTALL_DIR}/include)
catkin_package(
  INCLUDE_DIRS ${INSTALL_DIR}/include
)

# install the include directory from 
install(DIRECTORY ${INSTALL_DIR}/include/
        DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION})
