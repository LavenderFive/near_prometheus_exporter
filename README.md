# Near Prometheus Exporter

This service exports various metrics from Near node for consumption by [Prometheus](https://prometheus.io). It uses [JSON-RPC](https://docs.near.org/docs/interaction/rpc) interface to collect the metrics. Inspired by [Ethereum Prometheus Exporter](https://github.com/31z4/ethereum-prometheus-exporter)

## Install Docker
```
sudo apt-get update
sudo apt install docker.io
```

## Usage

You can deploy using the [lavender5/near-prometheus-exporter](https://hub.docker.com/r/lavender5/near_prometheus_exporter) Docker image.

```
sudo docker run -dit \
    --restart always \
    --name near-exporter \
    --network=host \
    -p 9333:9333 \
    lavender5/near_prometheus_exporter:latest /dist/main -accountId <YOUR_POOL_ID> -external-rpc <YOUR RPC> -url <YOUR RPC>
```

docker compose
```
  near_exporter:
    image: lavender5/near_prometheus_exporter:latest
    container_name: near_exporter
    restart: unless-stopped
    env_file:
      - '.env'
    command:
      - '/bin/near_exporter'
      - '-accountId=${NEAR_ACCOUNT_ID}'
      - '-external-rpc=${NEAR_NODE_URL}'
      - '-url=${NEAR_NODE_URL}'
    ports:
      - "9333:9333"
```

### Build own image

```
git clone https://github.com/lavenderfive/near-prometheus-exporter
cd near-prometheus-exporter
sudo docker build -t near-exporter .
```

```
sudo docker run -dit \
    --restart always \
    --name near-exporter \
    --network=host \
    -p 9333:9333 \
    near-exporter:latest /bin/near_exporter -accountId <YOUR_POOL_ID>
```

By default the exporter serves on `:9333` at `/metrics`.

## Exported Metrics

| Name | Description |
| ---- | ----------- |
| near_block_number | The number of most recent block |
| near_epoch_block_produced_number | The number of blocks produced in epoch |
| near_epoch_block_expected_number | The number of block expected in epoch |
| near_seat_price | The current seat price |
| near_current_stake | The current stake of a given account id |
| near_sync_state | The current sync state of node |
| near_epoch_start_height | The epoch start height |
| near_version_build{build,version} | The version build of the near node |
| near_dev_version_build{build,version} | The version build of of the public rpc node |
| near_next_validator_stake{account_id,public_key,shards} | The next stake of epoch |
| near_current_validator_stake{account_id,num_produced_blocks,num_expected_blocks,public_key,shards,slashed} |  The current stake of epoch |
| near_current_proposals_stake{account_id,public_key} | The current stake proposals  |
| near_prev_epoch_kickout{account_id,reason,produced,expected,stake_u128,threshold_u128} | Previous epoch kicked out validators |

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
