A partitioned cache can also be fronted by a `Near` cache, which is a smaller local cache that stores most recently or most frequently accessed data. Just like with a partitioned cache, the user can control the size of the near cache and its eviction policies. 

Near caches can be created directly on *client* nodes by passing `NearConfiguration` into the `Ignite.createNearCache(NearConfiguration)` or `Ignite.getOrCreateNearCache(NearConfiguration)` methods.
[block:code]
{
  "codes": [
    {
      "code": "// Create distributed cache on the server nodes, called \"myCache\".\nignite.getOrCreateCache(new CacheConfiguration<MyKey, MyValue>(\"myCache\"));\n\n// Create near-cache configuration for \"myCache\".\nNearCacheConfiguration<MyKey, MyValue> nearCfg = new NearCacheConfiguration<>();\n\n// Use LRU eviction policy to automatically evict entries\n// from near-cache, whenever it reaches 100_000 in size.\nnearCfg.setEvictionPolicy(new LruEvictionPolicy<>(100_000));\n\n// Create near-cache for \"myCache\".\nIgniteCache<MyKey, MyValue> cache = ignite.getOrCreateNearCache(\"myCache\", nearCfg);",
      "language": "java"
    }
  ]
}
[/block]
In the vast majority of use cases, whenever utilizing Ignite with affinity colocation, near caches should not be used. If computations are collocated with the corresponding partition cache nodes then the near cache is simply not needed because all the data is available locally in the partitioned cache.

However, there are cases when it is simply impossible to send computations to remote nodes. For cases like this near caches can significantly improve scalability and the overall performance of the application.
[block:callout]
{
  "type": "info",
  "title": "Near Caches on Server Nodes",
  "body": "In rare cases, whenever accessing data from `PARTITIONED` caches on the server side in non-collocated fashion, you may need to configure near-caches on the server nodes as well via `CacheConfiguration.setNearConfiguration(...)` property."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Configuration"
}
[/block]
Following are configuration parameters on `NearCacheConfiguration`.
[block:parameters]
{
  "data": {
    "0-0": "`setNearEvictionPolicy(CacheEvictionPolicy)`",
    "h-0": "Setter Method",
    "h-1": "Description",
    "h-2": "Default",
    "0-1": "Eviction policy for the near cache.",
    "0-2": "None",
    "1-0": "`setNearStartSize(int)`",
    "1-2": "`375,000`",
    "1-1": "Eviction policy for near cache."
  },
  "cols": 3,
  "rows": 2
}
[/block]