# As of 2025-11-25, Comparative using github references and AI to do the mapping.

## CPU

| Node Exporter Metric | Description | Closest OpenTelemetry Metric | Notes | Additional Notes |
|---------------------|-------------|------------------------------|-------|------------------|
| node_cpu_bug_info | CPU bugs reported by kernel | No direct equivalent | Node Exporter exposes hardware errata; OpenTelemetry does not track CPU bugs. | |
| node_cpu_core_throttles_total | Number of times CPU cores throttled | No direct equivalent | OpenTelemetry does not expose throttling counters. | |
| node_cpu_flag_info | CPU feature flags (e.g., SSE, AVX) | No direct equivalent | Node Exporter provides hardware capability info. | |
| node_cpu_frequency_hertz | Current CPU frequency per core | system.cpu.frequency | Both expose CPU frequency, but OpenTelemetry standardizes across systems. | |
| node_cpu_guest_seconds_total | Time spent running guest VMs | No direct equivalent | OpenTelemetry system.cpu.time does not include guest state. | |
| node_cpu_info | CPU model, vendor, stepping | system.cpu.physical.count / system.cpu.logical.count | OpenTelemetry provides counts, not detailed model info. | |
| node_cpu_isolated | Isolated CPUs (kernel isolation) | No direct equivalent | OpenTelemetry does not expose CPU isolation. | |
| node_cpu_package_throttles_total | Package-level throttling events | No direct equivalent | Node Exporter is more hardware-specific. | |
| node_cpu_scaling_frequency_hertz | Scaled CPU frequency | No direct equivalent | OpenTelemetry frequency is abstracted; Node Exporter shows scaling details. | |
| node_cpu_scaling_frequency_max_hertz | Max scaling frequency | No direct equivalent | OpenTelemetry does not expose scaling ranges. | |
| node_cpu_scaling_frequency_min_hertz | Min scaling frequency | No direct equivalent | Same as above. | |
| node_cpu_scaling_governor | Current CPU governor (performance, powersave) | No direct equivalent | Node Exporter provides kernel governor info. | |
| node_cpu_seconds_total | Total CPU time by mode (user, system, idle, etc.) | system.cpu.time | Direct mapping: both expose CPU time breakdown. | |
| node_cpu_vulnerabilities_info | CPU vulnerabilities (e.g., Spectre, Meltdown) | No direct equivalent | OpenTelemetry does not track vulnerabilities. | |
| — | — | system.cpu.utilization | Utilization percentage across cores | Node Exporter does not expose utilization directly; must be derived from node_cpu_seconds_total. |

## Memory

| Node Exporter Metric | Description | Closest OpenTelemetry Metric | Notes |
|---------------------|-------------|------------------------------|-------|
| node_memory_Active_anon_bytes, node_memory_Active_file_bytes, node_memory_Active_bytes | Active memory (anon/file/total) | system.memory.usage | OpenTelemetry reports overall usage; Node Exporter splits into anon/file. |
| node_memory_AnonHugePages_bytes, node_memory_AnonPages_bytes | Anonymous pages, huge pages | No direct equivalent | OpenTelemetry does not expose anon/huge page breakdown. |
| node_memory_Bounce_bytes | Bounce buffers | No direct equivalent | Kernel-specific detail not in HostMetrics. |
| node_memory_Buffers_bytes | Buffers cache | system.memory.usage | Part of memory usage, but not broken out in HostMetrics. |
| node_memory_Cached_bytes | Page cache | system.memory.usage | Included in usage, not separated. |
| node_memory_CommitLimit_bytes | Commit limit | system.memory.limit | Both expose memory limit, Node Exporter shows commit limit specifically. |
| node_memory_Committed_AS_bytes | Committed memory | system.memory.usage | Usage metric covers committed memory. |
| node_memory_DirectMap2M_bytes, node_memory_DirectMap4k_bytes | Direct mapped memory | No direct equivalent | HostMetrics does not expose direct map sizes. |
| node_memory_Dirty_bytes | Dirty pages | system.linux.memory.dirty | Direct mapping. |
| node_memory_HardwareCorrupted_bytes | Corrupted memory | No direct equivalent | Not exposed in HostMetrics. |
| node_memory_HugePages_* (Free, Rsvd, Surp, Total, Hugepagesize) | Huge page stats | No direct equivalent | HostMetrics does not expose huge page stats. |
| node_memory_Inactive_* (anon, file, total) | Inactive memory | system.memory.usage | Included in usage, not broken out. |
| node_memory_KernelStack_bytes | Kernel stack memory | No direct equivalent | Not exposed in HostMetrics. |
| node_memory_Mapped_bytes | Mapped files | No direct equivalent | Not exposed in HostMetrics. |
| node_memory_MemFree_bytes | Free memory | system.linux.memory.available | Available memory is the closest equivalent. |
| node_memory_MemTotal_bytes | Total memory | system.memory.limit | Both expose total memory. |
| node_memory_Mlocked_bytes | Locked memory | No direct equivalent | Not exposed in HostMetrics. |
| node_memory_NFS_Unstable_bytes | NFS unstable pages | No direct equivalent | Not exposed in HostMetrics. |
| node_memory_PageTables_bytes | Page table memory | No direct equivalent | Not exposed in HostMetrics. |
| node_memory_Shmem_bytes | Shared memory | No direct equivalent | Not exposed in HostMetrics. |
| node_memory_Slab_bytes, node_memory_SReclaimable_bytes, node_memory_SUnreclaim_bytes | Slab allocator memory | No direct equivalent | Not exposed in HostMetrics. |
| node_memory_SwapCached_bytes, node_memory_SwapFree_bytes, node_memory_SwapTotal_bytes | Swap stats | No direct equivalent | HostMetrics does not expose swap. |
| node_memory_Unevictable_bytes | Unevictable memory | No direct equivalent | Not exposed in HostMetrics. |
| node_memory_Vmalloc* (Chunk, Total, Used) | vmalloc stats | No direct equivalent | Not exposed in HostMetrics. |
| node_memory_Writeback_bytes, node_memory_WritebackTmp_bytes | Writeback memory | No direct equivalent | HostMetrics does not expose writeback. |
| NUMA-related (node_memory_numa_*) | NUMA node-specific memory stats | No direct equivalent | HostMetrics does not expose NUMA breakdown. |
| — | — | system.memory.page_size | Page size | Node Exporter does not expose page size directly. |
| — | — | system.memory.utilization | Utilization percentage | Node Exporter does not expose utilization directly; must be derived from MemTotal vs MemFree/Active |

## Network

| Node Exporter Metric | Description | Closest OpenTelemetry Metric | Notes (Mapping) | Additional Notes (OTel-only) |
|---------------------|-------------|------------------------------|------------------|------------------------------|
| node_network_receive_bytes_total | Total bytes received | system.network.io (direction=receive) | Direct mapping; OTel uses one metric with direction attribute instead of separate counters. | — |
| node_network_transmit_bytes_total | Total bytes transmitted | system.network.io (direction=transmit) | Same as above, mapped to direction=transmit. | — |
| node_network_receive_packets_total | Packets received | system.network.packets (direction=receive) | Equivalent; OTel collapses into one metric with direction. | — |
| node_network_transmit_packets_total | Packets transmitted | system.network.packets (direction=transmit) | Same mapping, transmit side. | — |
| node_network_receive_errs_total | Receive errors | system.network.errors (direction=receive) | Equivalent; OTel uses one metric with direction. | — |
| node_network_transmit_errs_total | Transmit errors | system.network.errors (direction=transmit) | Same mapping, transmit side. | — |
| node_network_receive_drop_total | Dropped incoming packets | system.network.dropped (direction=receive) | Equivalent; OTel collapses into one metric with direction. | — |
| node_network_transmit_drop_total | Dropped outgoing packets | system.network.dropped (direction=transmit) | Same mapping, transmit side. | — |
| node_network_collisions_total | Ethernet collisions detected | No direct equivalent | Node Exporter exposes low-level NIC collision stats; OTel does not. | — |
| node_network_receive_fifo_total | FIFO buffer errors on receive | No direct equivalent | Node Exporter exposes FIFO buffer errors; OTel omits. | — |
| node_network_transmit_fifo_total | FIFO buffer errors on transmit | No direct equivalent | Same as above, transmit side. | — |
| node_network_receive_frame_total | Frame errors on receive | No direct equivalent | Node Exporter exposes frame errors; OTel omits. | — |
| node_network_transmit_carrier_total | Carrier losses on transmit | No direct equivalent | Node Exporter exposes carrier errors; OTel omits. | — |
| node_network_receive_compressed_total | Compressed packets received | No direct equivalent | Node Exporter exposes compression stats; OTel omits. | — |
| node_network_transmit_compressed_total | Compressed packets transmitted | No direct equivalent | Same as above, transmit side. | — |
| node_network_receive_multicast_total | Multicast packets received | No direct equivalent | Node Exporter exposes multicast receive count; OTel omits. | — |
| — | — | system.network.connections | OTel-only metric: counts connections by protocol and state. Node Exporter does not expose this in node_network_* (though some info exists in node_netstat_*). | Richer context: TCP/UDP, connection states. |
| — | — | system.network.conntrack.count / system.network.conntrack.max | OTel-only metric: Linux conntrack table stats. Node Exporter does not expose conntrack. | Useful for NAT/firewall monitoring. |

## Disk

| Node Exporter Metric | Description | Closest OpenTelemetry Metric | Notes (Mapping) | Additional Notes (How to get HostMetrics from Node Exporter) |
|---------------------|-------------|------------------------------|------------------|---------------------------------------------------------------|
| node_disk_ata_rotation_rate_rpm | Disk rotation speed (RPM) | No direct equivalent | ATA-specific hardware info | Not available in HostMetrics |
| node_disk_ata_write_cache | ATA write cache status | No direct equivalent | ATA cache info | Not available in HostMetrics |
| node_disk_ata_write_cache_enabled | ATA write cache enabled flag | No direct equivalent | ATA cache info | Not available in HostMetrics |
| node_disk_device_mapper_info | Device mapper metadata | No direct equivalent | Device mapper info | Not available in HostMetrics |
| node_disk_discarded_sectors_total | Discarded sectors count | No direct equivalent | Discard stats | Not available in HostMetrics |
| node_disk_discards_completed_total | Completed discard requests | No direct equivalent | Discard stats | Not available in HostMetrics |
| node_disk_discards_merged_total | Merged discard requests | No direct equivalent | Discard stats | Not available in HostMetrics |
| node_disk_discard_time_seconds_total | Time spent on discards | No direct equivalent | Discard stats | Not available in HostMetrics |
| node_disk_filesystem_info | Filesystem metadata | No direct equivalent | Filesystem info | Not available in HostMetrics |
| node_disk_flush_requests_time_seconds_total | Time spent on flush requests | No direct equivalent | Flush stats | Not available in HostMetrics |
| node_disk_flush_requests_total | Flush requests count | No direct equivalent | Flush stats | Not available in HostMetrics |
| node_disk_info | Disk metadata (major/minor, model) | No direct equivalent | Disk info | Not available in HostMetrics |
| node_disk_io_now | Current I/O operations in progress | system.disk.pending_operations | Equivalent | Direct mapping, no split needed |
| node_disk_io_time_seconds_total | Time disk spent doing I/O | system.disk.io_time | Equivalent | Direct mapping, no split needed |
| node_disk_io_time_weighted_seconds_total | Weighted I/O time (queue length × time) | system.disk.weighted_io_time | Equivalent | Direct mapping, no split needed |
| node_disk_read_bytes_total | Bytes read | system.disk.io{direction="read"} | Equivalent | HostMetrics has unified metric system.disk.io. Node Exporter splits into read/write; use directly for read. To get total I/O bytes in HostMetrics, sum node_disk_read_bytes_total + node_disk_written_bytes_total. |
| node_disk_written_bytes_total | Bytes written | system.disk.io{direction="write"} | Equivalent | Same as above; sum read + write for HostMetrics total. |
| node_disk_reads_completed_total | Completed read requests | system.disk.operations{direction="read"} | Equivalent | HostMetrics has unified system.disk.operations. Node Exporter splits; sum reads + writes for total ops. |
| node_disk_writes_completed_total | Completed write requests | system.disk.operations{direction="write"} | Equivalent | Same as above. |
| node_disk_reads_merged_total | Merged read requests | system.disk.merged{direction="read"} | Equivalent | HostMetrics has unified system.disk.merged. Node Exporter splits; sum reads + writes for total merged ops. |
| node_disk_writes_merged_total | Merged write requests | system.disk.merged{direction="write"} | Equivalent | Same as above. |
| node_disk_read_time_seconds_total | Time spent reading | system.disk.operation_time{direction="read"} | Equivalent | HostMetrics has unified system.disk.operation_time. Node Exporter splits; sum read + write for total operation time. |
| node_disk_write_time_seconds_total | Time spent writing | system.disk.operation_time{direction="write"} | Equivalent | Same as above. |

## Filesystem

| Node Exporter Metric | Description | Closest HostMetrics Metric | Notes (Mapping) | Additional Notes (Reconstruction / Differences) |
|---------------------|-------------|----------------------------|-------------------|--------------------------------------------------|
| node_filesystem_size_bytes | Total size of the filesystem | system.filesystem.size | Direct equivalent | HostMetrics uses attributes (`state=used free reserved`) to split usage; Node Exporter only reports total size. |
| node_filesystem_avail_bytes | Free space available to non-root users | system.filesystem.usage{state="free"} | Equivalent concept | In HostMetrics, usage with state="free" matches this. |
| node_filesystem_free_bytes | Total free space including root-reserved blocks | system.filesystem.usage{state="free"} | Similar, but semantics differ | Node Exporter distinguishes free vs avail; HostMetrics typically reports free as non-root available. |
| node_filesystem_files | Total number of inodes | system.filesystem.inodes{state="used" + "free"} | Equivalent | HostMetrics splits into used and free; Node Exporter reports total. |
| node_filesystem_files_free | Free inodes available | system.filesystem.inodes{state="free"} | Equivalent | HostMetrics matches directly. |
| node_filesystem_device_error | Filesystem device error flag | No direct equivalent | Node Exporter-only | HostMetrics does not expose device error flags. |
| node_filesystem_readonly | Filesystem read-only flag | No direct equivalent | Node Exporter-only | HostMetrics does not expose read-only status. |

## Processes

| Node Exporter Metric | Description | Closest HostMetrics Metric | Notes (Mapping) | Additional Notes |
|---------------------|-------------|----------------------------|-------------------|-------------------|
| node_processes_running | Number of processes in running state | system.processes.count{state="running"} | Direct equivalent | Matches exactly. |
| node_processes_blocked | Number of processes in blocked (uninterruptible sleep) state | system.processes.count{state="blocked"} | Direct equivalent | Matches exactly. |
| node_processes_max_processes | Maximum number of processes allowed by the kernel | No direct equivalent | Node Exporter-only | HostMetrics does not expose kernel max process limit. |
| node_processes_max_threads | Maximum number of threads allowed by the kernel | No direct equivalent | Node Exporter-only | HostMetrics does not expose kernel max thread limit. |
| node_processes_pids | Number of allocated process IDs | No direct equivalent | Node Exporter-only | HostMetrics does not expose PID allocation count. |
| node_processes_state | Number of processes in each state (running, sleeping, zombie, etc.) | system.processes.count{state="*"} | Equivalent concept | Node Exporter reports per-state counts; HostMetrics uses attributes (state). Mapping is direct. |
| node_processes_threads | Number of threads across all processes | No direct equivalent | Node Exporter-only | HostMetrics does not expose total thread count. |
| — | — | system.processes.created | OTel-only metric | HostMetrics exposes process creation count; Node Exporter does not provide this metric. |
