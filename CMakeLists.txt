cmake_minimum_required(VERSION 3.22)
set(EIPSCANNER_MAJOR_VERSION 1)
set(EIPSCANNER_MINOR_VERSION 1)
set(EIPSCANNER_PATCH_VERSION 0)
set(EIPSCANNER_FULL_VERSION ${EIPSCANNER_MAJOR_VERSION}.${EIPSCANNER_MINOR_VERSION}.${EIPSCANNER_PATCH_VERSION})
project(eip_scanner VERSION ${EIPSCANNER_FULL_VERSION})

find_package(ament_cmake REQUIRED)

add_library(
    ${PROJECT_NAME} SHARED
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
target_include_directories(${PROJECT_NAME} PUBLIC $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/src>
                                                  $<INSTALL_INTERFACE:include>
                                                  $<INSTALL_INTERFACE:include/${PROJECT_NAME}>)
set_target_properties(
    ${PROJECT_NAME}
    PROPERTIES VERSION ${EIPSCANNER_FULL_VERSION}
               SOVERSION ${EIPSCANNER_MAJOR_VERSION}
               CXX_STANDARD 20
               CXX_STANDARD_REQUIRED YES
               CXX_EXTENSIONS OFF)

ament_export_targets(${PROJECT_NAME}Targets HAS_LIBRARY_TARGET)

install(
    TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}Targets
    LIBRARY DESTINATION lib)
install(
    DIRECTORY ${CMAKE_SOURCE_DIR}/src/
    DESTINATION include/${PROJECT_NAME}
    FILES_MATCHING
    PATTERN "*.h*"
    PATTERN "vendor" EXCLUDE)

ament_package()
