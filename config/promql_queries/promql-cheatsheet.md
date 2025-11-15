4.1 CPU Usage (% from Telegraf)

100 - (rate(cpu_time_idle{cpu="cpu-total"}[5m]) * 100)


4.2 Memory Usage (% from Telegraf)

(mem_used / mem_total) * 100


4.3 Disk Read Throughput (bytes/sec)

rate(diskio_read_bytes[5m])


4.4 Disk Space Usage (Node Exporter, per filesystem)


100 * (1 - (node_filesystem_avail_bytes{fstype!~"tmpfs|overlay|squashfs"} / node_filesystem_size_bytes{fstype!~"tmpfs|overlay|squashfs"}))
