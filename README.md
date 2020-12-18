# ratelimit

Package `ratelimit` provides a simpler concurrent resource based rate limiter API and multiple implementations. It should be safe for concurrent usage. 

The aim is to find a performant implementation to use a generic log rate limiter for Luno microservices. Performance is more important than correctness in this use case since we expect high level of concurrency and over (or under) limiting is not an issue.

```
// RateLimiter is the interface implemented by a resource rate limiter.
type RateLimiter interface {
	// Request returns true if the request of the resource conforms
	// to the rate limit; it contributes to the rate. It returns false
	// if the request would exceed the rate limit; it doesn't
	// contributes to the rate.
	Request(resource string) bool
}
```

The package also provides performance benchmarking. The following are results of `ratelimit.Benchmark` for different implementations with config aimed at `limit=100, period=10ms` executed on MacBook Pro Quad Core 16GB RAM: 

|Implementation | 1 | 4 | 16 | 64 | 256 | 1024 | 4096 | 16384 |
|---|--|--|--|--|--|--|--|--|
|github.com/corver/ratelimit.NaiveWindow | 327 | 1_558 | 8_062 | 27_476 | 101_406 | 469_711 | 1_859_209 | 6_403_791 |
|github.com/corver/ratelimit.SyncMapWindow | 611 | 2_060 | 13_346 | 50_025 | 193_808 | 799_810 | 2_149_220 | 5_444_247 |

Please DM me link to your implementation for me to add your benchmark score.
