project(grpc-cmake-example)
cmake_minimum_required(VERSION 3.2)

add_compile_options(-std=c++11)

# GRPC and Protocol Buffers libraries location
list(APPEND CMAKE_PREFIX_PATH "/opt/grpc" "/opt/protobuf")

# Cmake find modules
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")

find_package(Protobuf REQUIRED)
find_package(GRPC REQUIRED)

#set(PROTOS_0 ${CMAKE_CURRENT_SOURCE_DIR}/protos/helloworld.proto)
set(PROTOS_1 ${CMAKE_CURRENT_SOURCE_DIR}/protos/teerpc.proto)
set(PROTOS_2 ${CMAKE_CURRENT_SOURCE_DIR}/protos/teetask.proto)

set(PROTO_SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR})
file(MAKE_DIRECTORY ${PROTO_SRC_DIR})
include_directories(${PROTO_SRC_DIR})

message(STATUS ${PROTO_SRCS})
message(STATUS ${GRPC_SRCS})

protobuf_generate_cpp(PROTO_SRCS_1 PROTO_HDRS ${PROTO_SRC_DIR} ${PROTOS_1})
grpc_generate_cpp(GRPC_SRCS_1 GRPC_HDRS ${PROTO_SRC_DIR} ${PROTOS_1})
message(STATUS ${PROTO_SRCS})
message(STATUS ${GRPC_SRCS})

protobuf_generate_cpp(PROTO_SRCS_2 PROTO_HDRS ${PROTO_SRC_DIR} ${PROTOS_2})
message(STATUS ${PROTO_SRCS})
message(STATUS ${GRPC_SRCS})

# protobuf_generate_cpp(PROTO_SRCS_0 PROTO_HDRS ${PROTO_SRC_DIR} ${PROTOS_0})
# grpc_generate_cpp(GRPC_SRCS_0 GRPC_HDRS ${PROTO_SRC_DIR} ${PROTOS_0})
# message(STATUS ${PROTO_SRCS})
# message(STATUS ${GRPC_SRCS})

# Building server
add_executable(greeter_server
    ${CMAKE_CURRENT_SOURCE_DIR}/greeter_server.cc
    ${PROTO_SRCS_1}
    ${GRPC_SRCS_1}
    ${PROTO_SRCS_2}
)
target_link_libraries(greeter_server
    gRPC::grpc++_reflection
    protobuf::libprotobuf
)

## Building client
#add_executable(greeter_client
#    ${CMAKE_CURRENT_SOURCE_DIR}/greeter_client.cc
#    ${PROTO_SRCS_0}
#    ${GRPC_SRCS_0}
#)
#target_link_libraries(greeter_client
#    gRPC::grpc++_reflection
#    protobuf::libprotobuf
#)
