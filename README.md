
git clone -b v1.43.0 https://github.com/grpc/grpc

git submodule update --init




third_party/protobuf/protoc --cpp_out=.  --plugin=protoc-gen-grpc=grpc_cpp_plugin --grpc_out=.   pou.proto 
