
1. Long 缓存了-128到127的所有Long值。Integer也一样。



2.Long大家为什么推荐使用valueOf，而不是parseLong？

因为Long实现了缓存机制，缓存了-128到127.valueOf会从缓存中取值，如果命中，则减少资源的开销。parseLong则就没有这个机制。
