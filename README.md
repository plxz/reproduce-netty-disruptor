## netty-disruptor and mischievous-server
Applications developed to expose suspected reactor-netty resilience issue. 
To run:
- in new shell `cd mischievous-server && ./gradlew bootRun`
- in new shell `cd netty-disruptor && ./gradlew bootRun`
- `curl -v localhost:8123/req/8` - small amounts of requests work fine, exec `curl -s  http://localhost:8123/actuator/prometheus | grep reactor_netty_connection_provider_disruptorFixedConnectionPool_active_connections` to see all connections are released
- now hit it with bigger amount: `curl -v localhost:8123/req/900`
- `curl -s  http://localhost:8123/actuator/prometheus | grep reactor_netty_connection_provider_disruptorFixedConnectionPool_active_connections` now will show some number of 'active' (unusable really)  connections 
- when number of stuck connections reaches 100 (limit of `ConnectionProvider` `maxConnections`) any further attempt to destination fails

