# klog-gokit [![CircleCI](https://circleci.com/gh/simonpasquier/klog-gokit.svg?style=svg)](https://circleci.com/gh/simonpasquier/klog-gokit)

This package is a replacement for [k8s.io/klog](https://github.com/kubernetes/klog)
in projects that use the [go-kit logger](https://godoc.org/github.com/go-kit/log).

It also supports [k8s.io/klog/v2](https://pkg.go.dev/k8s.io/klog/v2).

It is heavily inspired by the [`github.com/kubermatic/glog-gokit`](https://github.com/kubermatic/glog-gokit) package.

## Usage

Override the official klog package with this one.
This simply replaces the code in `vendor/k8s.io/klog` with the code of this package.

**With dep**

In your `Gopkg.toml`:
```toml
[[override]]
  name = "k8s.io/klog"
  source = "github.com/simonpasquier/klog-gokit"
```

**With Go modules**

Add this line to your `go.mod` file:

```
replace k8s.io/klog => github.com/simonpasquier/klog-gokit master
```

In your `main.go`:
```go
// Import the package like it is original klog
import "k8s.io/klog"


// Create go-kit logger in your main.go
logger := log.NewLogfmtLogger(log.NewSyncWriter(os.Stdout))
logger = log.With(logger, "ts", log.DefaultTimestampUTC)
logger = log.With(logger, "caller", log.DefaultCaller)
logger = level.NewFilter(logger, level.AllowAll())

// Overriding the default klog with our go-kit klog implementation.
// Thus we need to pass it our go-kit logger object.
klog.SetLogger(logger)
```

Setting the logger to the klog package **MUST** happen before using klog in any package.

## Function Levels

|     klog     | gokit |
| ------------ | ----- |
| Info         | Debug |
| InfoDepth    | Debug |
| Infof        | Debug |
| Infoln       | Debug |
| Warning      | Warn  |
| WarningDepth | Warn  |
| Warningf     | Warn  |
| Warningln    | Warn  |
| Error        | Error |
| ErrorDepth   | Error |
| Errorf       | Error |
| Errorln      | Error |
| Exit         | Error |
| ExitDepth    | Error |
| Exitf        | Error |
| Exitln       | Error |
| Fatal        | Error |
| FatalDepth   | Error |
| Fatalf       | Error |
| Fatalln      | Error |

This table is rather opinionated and build for use with the Kubernetes' [Go client](https://github.com/kubernetes/client-go).

## Disclaimer

This project doesn't aim at covering the complete `klog` API. That being said, it should work ok for
projects that use `k8s.io/client-go` (like [Prometheus](https://github.com/prometheus/prometheus) for instance).

## License

Apache License 2.0, see [LICENSE](https://github.com/simonpasquier/klog-gokit/blob/master/LICENSE).
