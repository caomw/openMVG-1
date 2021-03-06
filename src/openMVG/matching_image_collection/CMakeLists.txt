project(matching_image_collection)

add_subdirectory(gpu)
add_subdirectory(mih)

file(
	GLOB_RECURSE
  matching_collection_images_files_header
  *.hpp
)

file(
	GLOB_RECURSE
  matching_collection_images_files_cpp
  *.cpp
)
file(GLOB_RECURSE REMOVEFILESUNITTEST *_test.cpp)

#Remove the unit test files (not been used by the library)
list(REMOVE_ITEM matching_collection_images_files_cpp ${REMOVEFILESUNITTEST})

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/gpu)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/mih/include)

ADD_LIBRARY(openMVG_matching_image_collection
  ${matching_collection_images_files_header}
  ${matching_collection_images_files_cpp})
add_dependencies(openMVG_matching_image_collection bitMatcher_cuda mih_cpu)
TARGET_LINK_LIBRARIES(openMVG_matching_image_collection bitMatcher_cuda mih_cpu openMVG_matching)
SET_TARGET_PROPERTIES(openMVG_matching_image_collection PROPERTIES SOVERSION ${OPENMVG_VERSION_MAJOR} VERSION "${OPENMVG_VERSION_MAJOR}.${OPENMVG_VERSION_MINOR}")
SET_PROPERTY(TARGET openMVG_matching_image_collection PROPERTY FOLDER OpenMVG)
INSTALL(TARGETS openMVG_matching_image_collection DESTINATION lib EXPORT openMVG-targets)

UNIT_TEST(openMVG Pair_Builder "")
