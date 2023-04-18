# fx-webserver

Example usage [Uber Fx](https://github.com/uber-go/fx)

## Structure

- cmd/
  - server/
    - main.go
- internal/
  - providers/
    - config/
    - logger/
    - server/
      - handlers/
        - handler.go
      - engine.go
      - module.go
      - routes.go
      - server.go
    - providers.go
- pkg/
  - model
    - config
      - config.go
- config.yaml

### internal/providers

Use to create provider modules, and implement on cmd/server/main.go

```go
package main

import (
	"github.com/lbernardo/fx-webserver/internal/providers"
	"github.com/lbernardo/fx-webserver/internal/providers/server"
	"go.uber.org/fx"
)

func main() {
	newServerRegisters().Run()
}

func newServerRegisters() *fx.App {
	return fx.New(
		fx.Provide(providers.DefaultProviders...), // Default providers
		server.New(),
	)
}
```

### Create routes

Change `internal/providers/server/routes.go`

```go
package server

import (
	"github.com/gin-gonic/gin"
	"github.com/lbernardo/fx-webserver/internal/providers/server/handlers"
)

func RegisterRoutes(router *gin.RouterGroup, handler *handlers.Handler) {
	router.GET("/", handler.Get)
	router.POST("/", handler.Create)
	router.DELETE("/:id", handler.Delete)
	router.PATCH("/:id", handler.Update)
}
```