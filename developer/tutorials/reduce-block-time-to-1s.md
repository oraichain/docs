# Reduce the block time to 1 second

In summary, the goal of reducing block time in the Oraichain network ecosystem is to improve transaction speed, network throughput, and overall performance, making the blockchain more suitable for a wide range of applications, particularly those requiring fast and efficient transaction processing.

## Edit config.toml

The current value of ***timeout_commit*** is now 5s; let's change it to 500ms.

```bash
[consensus]
timeout_commit = "500ms"
```

## Restart process

If you are using systemd to manage process

```bash
systemctl restart orai
```

If you are using docker

```bash
docker-compose restart orai
``` 