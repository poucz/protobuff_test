cmake_minimum_required(VERSION 3.15)
project(my_project)

include(FetchContent)
FetchContent_Declare(
  gRPC
  GIT_REPOSITORY https://github.com/grpc/grpc
  GIT_TAG        v1.43.0
)
set(FETCHCONTENT_QUIET OFF)
FetchContent_MakeAvailable(gRPC)

find_package(Protobuf REQUIRED)


include_directories(${Protobuf_INCLUDE_DIRS})
include_directories(${CMAKE_CURRENT_BINARY_DIR})
PROTOBUF_GENERATE_CPP(PROTO_SRCS PROTO_HDRS my_api.proto)
protobuf_generate_grpc_cpp(GRPC_SRCS GRPC_HDRS my_api.proto)

#protobuf_generate(
#  PROTO_SRCS
#  PROTO_HDRS
#  my_api.proto
#  PLUGIN "grpc_cpp_plugin"
#  GENERATE_EXTENSIONS .grpc.pb.h .grpc.pb.cc .pb.h .pb.cc
#)

message("POU-" ${GRPC_SRCS})


add_executable(my_exe my_exe.cc  ${PROTO_SRCS} ${PROTO_HDRS})
target_link_libraries(my_exe grpc++)


#protobuf_generate(
#    TARGET my_exe
#    OUT_VAR PROTO_GENERATED_FILES
#    LANGUAGE cpp
#    GENERATE_EXTENSIONS .grpc.pb.h .grpc.pb.cc
#    #PLUGIN "protoc-gen-grpc=\$<TARGET_FILE:gRPC::grpc_cpp_plugin>"
#    # PLUGIN_OPTIONS "generate_mock_code=true"
#    PROTOS ${CMAKE_CURRENT_LIST_DIR}/my_api.proto
#    PROTOC_OUT_DIR "${CMAKE_CURRENT_BINARY_DIR}")
#
