GENDIR := gen

PROTO_GEN_GO_DIR ?= $(GENDIR)/go
PROTO_GEN_JAVA_DIR ?= $(GENDIR)/java

# Find all .proto files.
PROTO_FILES := $(wildcard opensergo/proto/*/*/*.proto)

PROTOC := protoc

# Function to execute a command. Note the empty line before endef to make sure each command
# gets executed separately instead of concatenated with previous one.
# Accepts command to execute as first parameter.
define exec-command
$(1)

endef

# Generate gRPC/Protobuf implementation for Go.
.PHONY: gen-go
gen-go:
	rm -rf ./$(PROTO_GEN_GO_DIR)
	mkdir -p ./$(PROTO_GEN_GO_DIR)
	$(foreach file,$(PROTO_FILES),$(call exec-command,$(PROTOC) \
	  --go_out=./$(PROTO_GEN_GO_DIR) \
	  --go-grpc_out=./$(PROTO_GEN_GO_DIR) \
	  $(file)))


# Generate gRPC/Protobuf implementation for Java.
.PHONY: gen-java
gen-java:
	rm -rf ./$(PROTO_GEN_JAVA_DIR)
	mkdir -p ./$(PROTO_GEN_JAVA_DIR)
	echo $(PROTO_FILES)
	$(foreach file,$(PROTO_FILES),$(call exec-command, $(PROTOC) \
	  --java_out=./$(PROTO_GEN_JAVA_DIR) \
	  --grpc-java_out=./$(PROTO_GEN_JAVA_DIR) \
	  $(file)))

api:
	mkdir -p build/java/ build/go/
	protoc --proto_path=. \
 	       --go_out=build/go/ \
 	       --go-grpc_out=build/go/ \
 	       --java_out=build/java/ \
	       --grpc-java_out=build/java/ \
	       specification/proto/servicecontract.proto

clean:
	rm -rf build/
