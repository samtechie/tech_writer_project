# Multi-Region PostgreSQL and Redis

You have an application powered by PostgreSQL and you want to make it fast by adding a caching layer like Redis. But you don't want to stop there. You want to make it globally distributed. Turns out PostgreSQL and Redis work great on Fly.io's CDN-like infrastructure. Best of all, if your app is read heavy, it will work across Fly.io regions without requiring any modifications to your architecture.

Sounds too good to be true? We will show you how.

Fly enables a database run in multiple regions by utilizing read replicas and a primary writable database. The `fly-replay` headers are used to specify which requests need to be handled by the writable database. With this approach, you are really geo caching with the read replicas. Luckily, we have Redis which is perfect for this sort of thing.

Redis will also leverage this simple but powerful distributed design with a primary write instance and regional replicas. However, we can take it step further. Redis has a handy [config option](https://redis.io/topics/replication#read-only-replica): `replica-read-only` which defaults to `yes` but can be set to `no` so we can accept writes to replicas that are closest to our users and still sync changes from our primary instance.

There are two approaches we can take if we are using both PostgreSQL and Redis.

1. We can have a two node PostgreSQL server in one region with a leader for writes, a replica for redundancy and a primary Redis instance in the same region with replicas in other regions. Writes to the primary region propagate while writes to other regions are region local

2. In addition to approach 1, we add PostgreSQL regional read replicas and take advantage of the `fly-replay` headers for writes to the primary database.

Redis is useful for anything that requires simple fast storage but we will fall back to Postgres for the writes and reads that may be a bit more complicated. We will combine the two approaches.

## Setting up PostgreSQL

Setting up multi-region PostgreSQL is covered in detail [here](https://fly.io/docs/getting-started/multi-region-databases/) but adding read replicas to regions is as simple as running this command using Fly CLI assuming you have a PostgreSQL cluster named `friendly-postgres`:

```
fly volumes create pg_data -a friendly-postgres --size 10 --region atl

fly volumes create pg_data -a friendly-postgres --size 10 --region ord
```

The `friendly-postgres` cluster will have replicas in Georgia and Chicago

## Setting up Redis

Setting up a Redis Standalone server is covered in detail [here](https://fly.io/docs/app-guides/redis/)

Below are the steps you would take to run multi-region Redis

Create a volume in your primary region:

```
fly volumes create redis_server --size 10 --region scl
```

Add volumes in other regions:

```
fly volumes create redis_server --size 10 --region ord

fly volumes create redis_server --size 10 --region atl
```

Add resource instances i.e increase the VMs the app is running on:

```
fly scale count 2
```

You should run your application in the same region as your primary Redis instance and connect to the other instances using region specific addresses.

You can differentiate the regions by setting a `PRIMARY_REGION` environment variable on any application that connects to your Redis cluster and then utilizing the region specific addresses to determine where you send writes.

The primary Redis URL will be in this format

```
# format
redis://x:<password>@<primary-region>.<appname>.internal:6379

# example in atl
redis://x:password@atl.my-redis-app.internal:6379
```

To connect to a different replica, you only change the regional prefix. You can take advantage of the `$FLY_REGION` environment variable. This is one of the environment variables made available to the [Fly runtime environment](https://fly.io/docs/reference/runtime-environment/)

This architecture is not right for all applications and you may have to make some modifications to your application if it executes write heavy operations but for the most part, its fairly straightforward to take advantage of Fly to run multi-region applications that are much faster and offer a superior user experience. Learn more about the rationale behind globally distributed PostgreSQL and Redis [here](https://fly.io/blog/globally-distributed-postgres/) and [here](https://fly.io/blog/last-mile-redis/)
