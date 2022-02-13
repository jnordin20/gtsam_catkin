cmake_minimum_required(VERSION 2.8.3)
project(gtsam_catkin)

find_package(catkin_simple REQUIRED)

catkin_simple()

include(ExternalProject)
file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)
ExternalProject_Add(gtsam_src
  GIT_REPOSITORY https://github.com/borglab/gtsam.git
  GIT_TAG 4.0.3
  UPDATE_COMMAND ""
  CONFIGURE_COMMAND cmake -GNinja -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX} -DGTSAM_USE_SYSTEM_EIGEN=ON
  -DCMAKE_BUILD_TYPE=Release ../gtsam_src
  BUILD_COMMAND ninja
  INSTALL_COMMAND ninja install
)

install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/CppUnitLite
        DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
        FILES_MATCHING PATTERN "*.h")
install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/gtsam
        DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
        FILES_MATCHING PATTERN "*.h")
install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/gtsam_unstable
        DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
        FILES_MATCHING PATTERN "*.h")
install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/wrap
        DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
        FILES_MATCHING PATTERN "*.h")

install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/lib/
        DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        FILES_MATCHING PATTERN "libCppUnitLite*")
install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/lib/
        DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        FILES_MATCHING PATTERN "libgtsam*")

cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
          LIBRARIES CppUnitLite gtsam gtsam_unstable tbb)
