cmake_minimum_required(VERSION 3.22)

set(EIPSCANNER_MAJOR_VERSION 1)
set(EIPSCANNER_MINOR_VERSION 1)
set(EIPSCANNER_PATCH_VERSION 0)
set(EIPSCANNER_FULL_VERSION
    ${EIPSCANNER_MAJOR_VERSION}.${EIPSCANNER_MINOR_VERSION}.${EIPSCANNER_PATCH_VERSION}
)

project(eip_scanner VERSION ${EIPSCANNER_FULL_VERSION})

find_package(ament_cmake REQUIRED)

set(EIP_SOURCE_DIR ${CMAKE_SOURCE_DIR}/src)
set(EIP_INCLUDE_DIR
    ${CMAKE_INSTALL_PREFIX}/include
    CACHE INTERNAL "")

set(SOURCE_FILES
    src/cip/connectionManager/ForwardCloseRequest.cpp
    src/cip/connectionManager/ForwardOpenRequest.cpp
    src/cip/connectionManager/LargeForwardOpenRequest.cpp
    src/cip/connectionManager/ForwardOpenResponse.cpp
    src/cip/connectionManager/NetworkConnectionParametersBuilder.cpp
    src/cip/CipRevision.cpp
    src/cip/EPath.cpp
    src/cip/MessageRouterRequest.cpp
    src/cip/MessageRouterResponse.cpp
    src/cip/CipString.cpp
    src/eip/CommonPacket.cpp
    src/eip/CommonPacketItem.cpp
    src/eip/CommonPacketItemFactory.cpp
    src/eip/EncapsPacket.cpp
    src/eip/EncapsPacketFactory.cpp
    src/fileObject/FileObjectEmptyState.cpp
    src/fileObject/FileObjectLoadedState.cpp
    src/fileObject/FileObjectNonExistentState.cpp
    src/fileObject/FileObjectState.cpp
    src/fileObject/FileObjectUploadInProgressState.cpp
    src/sockets/BaseSocket.cpp
    src/sockets/EndPoint.cpp
    src/sockets/TCPSocket.cpp
    src/sockets/UDPBoundSocket.cpp
    src/sockets/UDPSocket.cpp
    src/utils/Logger.cpp
    src/utils/Buffer.cpp
    src/BaseObject.cpp
    src/ConnectionManager.cpp
    src/DiscoveryManager.cpp
    src/FileObject.cpp
    src/IdentityObject.cpp
    src/IOConnection.cpp
    src/MessageRouter.cpp
    src/ParameterObject.cpp
    src/SessionInfo.cpp)

file(
  GLOB_RECURSE EIP_HEADER_FILES
  RELATIVE ${EIP_SOURCE_DIR}
  ${EIP_SOURCE_DIR}/*.h**)
list(FILTER EIP_HEADER_FILES EXCLUDE REGEX ^vendor) # remove vendor files

add_custom_target(copyIncludeFiles)
foreach(file ${EIP_HEADER_FILES})
  set(sourceFile ${EIP_SOURCE_DIR}/${file})
  set(destinationFile ${EIP_INCLUDE_DIR}/${PROJECT_NAME}/${file})
  add_custom_command(
    TARGET copyIncludeFiles
    PRE_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy ${sourceFile} ${destinationFile}
    BYPRODUCTS ${destinationFile}
    VERBATIM)
endforeach()

add_library(${PROJECT_NAME} SHARED ${SOURCE_FILES})
target_include_directories(${PROJECT_NAME} PUBLIC ${EIP_INCLUDE_DIR})
target_include_directories(${PROJECT_NAME} PRIVATE ${EIP_SOURCE_DIR})
add_dependencies(${PROJECT_NAME} copyIncludeFiles)
include_directories(${EIP_INCLUDE_DIR})

set_target_properties(
  ${PROJECT_NAME}
  PROPERTIES VERSION ${EIPSCANNER_FULL_VERSION}
             SOVERSION ${EIPSCANNER_MAJOR_VERSION}
             CXX_STANDARD 14
             CXX_STANDARD_REQUIRED YES
             CXX_EXTENSIONS OFF)

add_library(${PROJECT_NAME}S STATIC ${SOURCE_FILES})
target_include_directories(${PROJECT_NAME}S PRIVATE ${EIP_SOURCE_DIR})
add_dependencies(${PROJECT_NAME}S copyIncludeFiles)

ament_export_targets(${PROJECT_NAME}Targets HAS_LIBRARY_TARGET)

set_target_properties(
  ${PROJECT_NAME}S
  PROPERTIES VERSION ${EIPSCANNER_FULL_VERSION}
             SOVERSION ${EIPSCANNER_MAJOR_VERSION}
             CXX_STANDARD 20
             CXX_STANDARD_REQUIRED YES
             CXX_EXTENSIONS OFF)

install(
  TARGETS ${PROJECT_NAME}
  EXPORT ${PROJECT_NAME}Targets
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES
  DESTINATION include)

ament_package()
