install-lint-deps: ## Install linter dependencies
	@echo "==> Updating linter dependencies..."
	@curl -sSfL -q https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s -- -b $(GOPATH)/bin v1.52.0

lint: install-lint-deps ## Lint Go code
	@if [ ! -z  $(PKG_NAME) ]; then \
		echo "golangci-lint run ./$(PKG_NAME)/..."; \
		golangci-lint run ./$(PKG_NAME)/...; \
	else \
		echo "golangci-lint run ./..."; \
		golangci-lint run ./...; \
	fi

.PHONY: install-protoc
install-protoc:
	go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
	go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest 

gen: install-protoc
	protoc -I=api --go_out=. movie.proto

gen-grpc: install-protoc
	protoc -I=api --go_out=. --go-grpc_out=. movie.proto

.PHONY: gen-mock
gen-mock:
	mkdir -p gen/mock/metadata/repository
	mockgen -package=repository -source=metadata/internal/controller/metadata/controller.go >  gen/mock/metadata/repository/repository.go

install-mockgen:
	go mod download github.com/golang/mock
	go install github.com/golang/mock/mockgen


create-metadata-mock:
	mkdir -p gen/mock/metadata/repository/
	mockgen -package=repository -source=metadata/internal/controller/metadata/controller.go > gen/mock/metadata/repository/repository.go
