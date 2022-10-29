"ile" project - my telegraf configuration.

# https://hub.docker.com/_/telegraf
# https://www.influxdata.com/blog/docker-run-telegraf-as-non-root/
docker run -d --restart=unless-stopped \
    --net host \
    --name=ile-telegraf \
    --user telegraf:$(stat -c '%g' /var/run/docker.sock) \
    -v $PWD/telegraf.conf:/etc/telegraf/telegraf.conf:ro \
    -v /var/run/docker.sock:/var/run/docker.sock:ro \
    -v /:/hostfs:ro \
    -e HOST_ETC=/hostfs/etc \
    -e HOST_PROC=/hostfs/proc \
    -e HOST_SYS=/hostfs/sys \
    -e HOST_VAR=/hostfs/var \
    -e HOST_RUN=/hostfs/run \
    -e HOST_MOUNT_PREFIX=/hostfs \
    telegraf:1.24.2
