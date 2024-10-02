Redis WatcherEx
====

[![build](https://github.com/jcasbin/redis-watcher-ex/actions/workflows/ci.yml/badge.svg)](https://github.com/jcasbin/redis-waycher-ex/actions/workflows/ci.yml)
[![Maven Central](https://img.shields.io/maven-central/v/org.casbin/jcasbin-redis-watcher-ex.svg)](https://central.sonatype.com/artifact/org.casbin/jcasbin-redis-watcher-ex)

---
## Installation
For Maven:
```xml
<dependency>
    <groupId>org.casbin</groupId>
    <artifactId>jcasbin-redis-watcher-ex</artifactId>
    <version>1.1.0</version>
</dependency>
```
## Simple Example

```java
package org.casbin.watcherEx;

import io.lettuce.core.RedisURI;
import org.casbin.jcasbin.main.Enforcer;


public class Main {

    public static void main(String[] args) {
        // Initialize the RedisWatcher.
        // Use the Redis host and port as parameters.
        WatcherOptions options = new WatcherOptions();
        options.setChannel("jcasbin-channel");
        options.setOptions(RedisURI.builder().withHost("your ip").withPort(6379).build());

        RedisWatcherEx redisWatcherEx = new RedisWatcherEx(options);
        
        // Set the update callback.
        redisWatcherEx.setUpdateCallback((msg) ->{
            System.out.println(msg);
        });

        // Initialize the JCasbin enforcer.
        Enforcer enforcer = new Enforcer("examples/rbac_model.conf", "examples/rbac_policy.csv");
        // Set the watcher for the enforcer.
        enforcer.setWatcher(redisWatcherEx);

        // Update the policy to test the effect.
        // You should see "[casbin rules updated]" in the log.
        enforcer.savePolicy();

        // Only exists in test (Wait for input to keep the program running)
        Scanner scanner = new Scanner(System.in);
        scanner.nextLine();
    }
}
```

## Getting Help

- [jCasbin](https://github.com/casbin/jcasbin)

## License

This project is under Apache 2.0 License. See the [LICENSE](LICENSE) file for the full license text.