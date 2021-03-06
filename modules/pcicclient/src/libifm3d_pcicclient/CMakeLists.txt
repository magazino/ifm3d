find_package(Threads REQUIRED)
find_package(Boost REQUIRED COMPONENTS system regex date_time)
find_package(glog QUIET CONFIG NAMES google-glog glog)
if (NOT TARGET glog::glog)
	find_library(LIB_glog NAMES glog)
else()
	set(LIB_glog glog::glog)
endif (NOT TARGET glog::glog)

file(GLOB IFM3D_PCICCLIENT_SOURCES *.cpp)

add_library(ifm3d_pcicclient_shared SHARED ${IFM3D_PCICCLIENT_SOURCES})
set_property(TARGET ifm3d_pcicclient_shared PROPERTY POSITION_INDEPENDENT_CODE 1)

target_link_libraries(
  ifm3d_pcicclient_shared
  ifm3d_camera_shared
  ${LIB_glog}
  ${CMAKE_THREAD_LIBS_INIT}
  ${Boost_LIBRARIES}
)


set_target_properties(ifm3d_pcicclient_shared PROPERTIES
VERSION ${IFM3D_VERSION_STRING}
OUTPUT_NAME ifm3d_pcicclient
LINKER_LANGUAGE CXX
)

if(WIN32)
    target_compile_definitions(ifm3d_pcicclient_shared PRIVATE
        IFM3D_PCICCLIENT_DLL_BUILD=1)
endif(WIN32)

install(TARGETS ifm3d_pcicclient_shared
  RUNTIME DESTINATION ${_bin} COMPONENT pcicclient
  LIBRARY DESTINATION ${_lib} COMPONENT pcicclient
  ARCHIVE DESTINATION ${_lib} COMPONENT pcicclient
  PUBLIC_HEADER DESTINATION ${_include} COMPONENT pcicclient
  )
