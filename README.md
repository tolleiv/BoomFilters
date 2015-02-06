# Stable Bloom Filter

This is Go implementation of Stable Bloom Filters as described by Deng and Rafiei in [Approximately Detecting Duplicates for Streaming Data using Stable Bloom Filters](http://webdocs.cs.ualberta.ca/~drafiei/papers/DupDet06Sigmod.pdf).

A Stable Bloom Filter (SBF) continuously evicts stale information so that it has room for more recent elements. Like traditional Bloom filters, an SBF has a non-zero probability of false positives, which is controlled by several parameters. Unlike the classic Bloom filter, an SBF has a tight upper bound on the rate of false positives while introducing a non-zero rate of false negatives. The false-positive rate of a classic Bloom filter eventually reaches 1, after which all queries result in a false positive. The stable-point property of an SBF means the false-positive rate asymptotically approaches a configurable fixed constant.

Stable Bloom Filters are useful for cases where the size of the data set isn't known a priori, which is a requirement for traditional Bloom filters. For example, an SBF can be used to deduplicate events from an unbounded event stream with a specified upper bound on false positives and minimal false negatives.

For documentation, see [godoc](http://godoc.org/github.com/tylertreat/StableBloomFilter).

## Installation 

```
$ go get github.com/tylertreat/StableBloomFilter
```

## Usage

```go
import (
    "fmt"
    "github.com/tylertreat/StableBloomFilter"
)

func main() {
    f := stable.NewDefaultBloomFilter(10000)
    fmt.Println("stable point", f.StablePoint())
    
    f.Add([]byte(`a`))
    if f.Test([]byte(`a`)) {
        fmt.Println("contains a")
    }
    
    if !f.TestAndAdd([]byte(`b`)) {
        fmt.Println("doesn't contain b")
    }
    
    if f.Test([]byte(`b`)) {
        fmt.Println("now it contains b!")
    }
    
    // Restore to initial state.
    f.Reset()
}
```
