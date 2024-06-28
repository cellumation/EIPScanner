cmake_minimum_required(VERSION 3.22)

set(EIPSCANNER_MAJOR_VERSION 1)
set(EIPSCANNER_MINOR_VERSION 1)
set(EIPSCANNER_PATCH_VERSION 0)
set(EIPSCANNER_FULL_VERSION ${EIPSCANNER_MAJOR_VERSION}.${EIPSCANNER_MINOR_VERSION}.${EIPSCANNER_PATCH_VERSION})

project(
    eip_scanner
    VERSION ${EIPSCANNER_FULL_VERSION}
    DESCRIPTION "Free implementation of EtherNet/IP in C++"
    HOMEPAGE_URL "https://github.com/nimbuscontrols/EIPScanner")

find_package(ament_cmake REQUIRED)

add_library(
    ${PROJECT_NAME} SHARED
    src/cip/connectionManager/ConnectionParameters.h
    src/cip/connectionManager/ForwardCloseRequest.cpp
    src/cip/connectionManager/ForwardCloseRequest.h
    src/cip/connectionManager/ForwardOpenRequest.cpp
    src/cip/connectionManager/ForwardOpenRequest.h
    src/cip/connectionManager/ForwardOpenResponse.cpp
    src/cip/connectionManager/ForwardOpenResponse.h
    src/cip/connectionManager/LargeForwardOpenRequest.cpp
    src/cip/connectionManager/LargeForwardOpenRequest.h
    src/cip/connectionManager/NetworkConnectionParametersBuilder.cpp
    src/cip/connectionManager/NetworkConnectionParametersBuilder.h
    src/cip/connectionManager/NetworkConnectionParams.h
    src/cip/CipRevision.cpp
    src/cip/CipRevision.h
    src/cip/CipString.cpp
    src/cip/CipString.h
    src/cip/EPath.cpp
    src/cip/EPath.h
    src/cip/GeneralStatusCodes.h
    src/cip/MessageRouterRequest.cpp
    src/cip/MessageRouterRequest.h
    src/cip/MessageRouterResponse.cpp
    src/cip/MessageRouterResponse.h
    src/cip/Services.h
    src/cip/Types.h
    src/eip/CommonPacket.cpp
    src/eip/CommonPacket.h
    src/eip/CommonPacketItem.cpp
    src/eip/CommonPacketItem.h
    src/eip/CommonPacketItemFactory.cpp
    src/eip/CommonPacketItemFactory.h
    src/eip/EncapsPacket.cpp
    src/eip/EncapsPacket.h
    src/eip/EncapsPacketFactory.cpp
    src/eip/EncapsPacketFactory.h
    src/fileObject/FileObjectEmptyState.cpp
    src/fileObject/FileObjectEmptyState.h
    src/fileObject/FileObjectLoadedState.cpp
    src/fileObject/FileObjectLoadedState.h
    src/fileObject/FileObjectNonExistentState.cpp
    src/fileObject/FileObjectNonExistentState.h
    src/fileObject/FileObjectState.cpp
    src/fileObject/FileObjectState.h
    src/fileObject/FileObjectUploadInProgressState.cpp
    src/fileObject/FileObjectUploadInProgressState.h
    src/sockets/BaseSocket.cpp
    src/sockets/BaseSocket.h
    src/sockets/EndPoint.cpp
    src/sockets/EndPoint.h
    src/sockets/Platform.cpp
    src/sockets/Platform.h
    src/sockets/TCPSocket.cpp
    src/sockets/TCPSocket.h
    src/sockets/UDPBoundSocket.cpp
    src/sockets/UDPBoundSocket.h
    src/sockets/UDPSocket.cpp
    src/sockets/UDPSocket.h
    src/utils/Buffer.cpp
    src/utils/Buffer.h
    src/utils/Logger.cpp
    src/utils/Logger.h
    src/BaseObject.cpp
    src/BaseObject.h
    src/ConnectionManager.cpp
    src/ConnectionManager.h
    src/DiscoveryManager.cpp
    src/DiscoveryManager.h
    src/FileObject.cpp
    src/FileObject.h
    src/IdentityObject.cpp
    src/IdentityObject.h
    src/IOConnection.cpp
    src/IOConnection.h
    src/MessageRouter.cpp
    src/MessageRouter.h
    src/ParameterObject.cpp
    src/ParameterObject.h
    src/SessionInfo.cpp
    src/SessionInfo.h
    src/SessionInfoIf.h)
target_include_directories(
    ${PROJECT_NAME} PUBLIC $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/src> $<INSTALL_INTERFACE:include>
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
