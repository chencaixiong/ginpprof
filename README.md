ginpprof
========

[![GoDoc](https://godoc.org/github.com/chencaixiong/ginpprof?status.svg)](https://godoc.org/github.com/chencaixiong/ginpprof)
[![Build
Status](https://travis-ci.org/chencaixiong/ginpprof.svg?branch=master)](https://travis-ci.org/chencaixiong/ginpprof)

A wrapper for [golang web framework gin](https://github.com/gin-gonic/gin) to use `net/http/pprof` easily.

## Install

First install ginpprof to your GOPATH using `go get`:

```sh
go get github.com/chencaixiong/ginpprof
```

## Usage

```go
package main

import (
	"github.com/gin-gonic/gin"

	"github.com/chencaixiong/ginpprof"
)

func main() {
	router := gin.Default()

	router.GET("/ping", func(c *gin.Context) {
		c.String(200, "pong")
	})

	// automatically add routers for net/http/pprof
	// e.g. /debug/pprof, /debug/pprof/heap, etc.
	ginpprof.Wrap(router)

	// ginpprof also plays well with *gin.RouterGroup
	// group := router.Group("/debug/pprof")
	// ginpprof.WrapGroup(group)

	router.Run(":8080")
}
```

Start this server, and you will see such outputs:

```text
[GIN-debug] GET    /ping                     --> main.main.func1 (3 handlers)
[GIN-debug] GET    /debug/pprof/             --> github.com/chencaixiong/ginpprof.IndexHandler.func1 (3 handlers)
[GIN-debug] GET    /debug/pprof/heap         --> github.com/chencaixiong/ginpprof.HeapHandler.func1 (3 handlers)
[GIN-debug] GET    /debug/pprof/goroutine    --> github.com/chencaixiong/ginpprof.GoroutineHandler.func1 (3 handlers)
[GIN-debug] GET    /debug/pprof/block        --> github.com/chencaixiong/ginpprof.BlockHandler.func1 (3 handlers)
[GIN-debug] GET    /debug/pprof/threadcreate --> github.com/chencaixiong/ginpprof.ThreadCreateHandler.func1 (3 handlers)
[GIN-debug] GET    /debug/pprof/cmdline      --> github.com/chencaixiong/ginpprof.CmdlineHandler.func1 (3 handlers)
[GIN-debug] GET    /debug/pprof/profile      --> github.com/chencaixiong/ginpprof.ProfileHandler.func1 (3 handlers)
[GIN-debug] GET    /debug/pprof/symbol       --> github.com/chencaixiong/ginpprof.SymbolHandler.func1 (3 handlers)
[GIN-debug] POST   /debug/pprof/symbol       --> github.com/chencaixiong/ginpprof.SymbolHandler.func1 (3 handlers)
[GIN-debug] GET    /debug/pprof/trace        --> github.com/chencaixiong/ginpprof.TraceHandler.func1 (3 handlers)
[GIN-debug] GET    /debug/pprof/mutex        --> github.com/chencaixiong/ginpprof.MutexHandler.func1 (3 handlers)
[GIN-debug] Listening and serving HTTP on :8080
```

Now visit [http://127.0.0.1:8080/debug/pprof/](http://127.0.0.1:8080/debug/pprof/) and you'll see what you want.

Have Fun.
