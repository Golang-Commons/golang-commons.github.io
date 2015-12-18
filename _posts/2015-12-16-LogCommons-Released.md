---
layout: post
title: "First Version of logCommons"
date: 2015-12-16
comments: true
categories:
---


This the beginning of Golang-Commons, and today we released the very first version of [logCommons](logCommons).

##Excellence

As the first package of Golang-Commons, I did not do much thing beside the `log` package. 
The `logCommons` package is just a small wrapper of the official `log` package, 
but supports some advanced features:

* Log Level
* Log Color
* Record function name

What's more, because we just do wrappings around `log`, the original functions such as `log.SetFlags(log.Ldate | log.Ltime | log.Lshortfile)` are still available and useful.

And in order of formatting, we provided the wrapper `logCommons.SetFlags(logCommons.Ldate | logCommons.Ltime | logCommons.Lshortfile)`,`logCommons.SetOutput(logFile)` too.

##Log Level

Log levels from Debug to Fatal:

* DEBUG   negligible
* INFO    
* WARNING 
* ERROR   
* PANIC   
* FATAL   significant

##Example

A complete example is:

```go
package main

import (
    "github.com/Golang-Commons/logCommons"
    "os"
)

type T struct {
    A string
    B string
}

func main() {
    logFile, err := os.OpenFile("./log.log", os.O_RDWR|os.O_CREATE|os.O_APPEND, 0666)
    if err != nil {
        logCommons.Println("error opening log file:", err)//we only copied Println mechod.
        os.Exit(1)
    }
    defer logFile.Close()

    a := T{"tt", "nn"}
    logCommons.SetLevel(logCommons.INFO)
    logCommons.SetFlag(logCommons.Lcolor | logCommons.Lfuncname | logCommons.Llevelname)
    logCommons.Debug("6666")//Debug will be ignored since log level is Info
    logCommons.Info("6666")
    logCommons.SetFlags(logCommons.Ldate | logCommons.Ltime | logCommons.Lshortfile)
    logCommons.Warn(a)
    logCommons.SetOutput(logFile)
    logCommons.Error("6666")
    logCommons.Panic("6666")
    logCommons.Fatal("6666")
}

```

output is:

```
$ go run f.go 
2015/12/17 03:59:04 [INFO] main.main 6666
2015/12/17 03:59:04 log.go:116: [WARNING] main.main {tt nn}
$ cat log.log 
2015/12/17 03:59:04 log.go:116: [ERROR] main.main 6666
2015/12/17 03:59:04 log.go:116: [PANIC] main.main 6666
2015/12/17 03:59:04 log.go:116: [FATAL] main.main 6666
```

[logCommons]: https://github.com/Golang-Commons/logCommons "logCommons github page"
