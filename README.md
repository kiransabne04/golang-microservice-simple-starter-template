# golang-microservice-simple-starter-template
This is simple golang microservice starter template. Based on simple authentication service, logger service, a test frontend, mailer service with postgreSQL &amp; mongo DB. Built it while learning. many things still to add.

i had to use absolute path to generate protobuf file
/opt/homebrew/bin/protoc \
  --plugin=protoc-gen-go=$(go env GOPATH)/bin/protoc-gen-go \
  --go_out=. --go_opt=paths=source_relative \
  --plugin=protoc-gen-go-grpc=$(go env GOPATH)/bin/protoc-gen-go-grpc \
  --go-grpc_out=. --go-grpc_opt=paths=source_relative logs.proto