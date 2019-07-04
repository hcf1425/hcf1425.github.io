### Inspecting containers

Containers are runtime instances of an image and have a lot of associated data that characterizes their behavior. To get more information about a specific container, we can use the inspect command. As usual, we have to provide either the container ID or name to identify the container of which we want to obtain the data. So, let's inspect our sample container:

```
$ docker container inspect quotes 
```

The response is a big JSON object full of details. It looks similar to this:

```
[
    {
        "Id": "a0dec1f50f53d27ae7b2b146aa5a8834a6710d6d7c912fb3388e46d68821090e",
        "Created": "2019-07-04T01:50:52.5099589Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "while :; do wget -qO- https://talaikis.com/api/quotes/random; printf '\\n'; sleep 5; done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 2390,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2019-07-04T01:50:53.1928871Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:4d90542f0623c71f1f9c11be3da23167174ac9d93731cf91912922e916bab02c",
        "ResolvConfPath": "/var/lib/docker/containers/a0dec1f50f53d27ae7b2b146aa5a8834a6710d6d7c912fb3388e46d68821090e/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/a0dec1f50f53d27ae7b2b146aa5a8834a6710d6d7c912fb3388e46d68821090e/hostname",
        "HostsPath": "/var/lib/docker/containers/a0dec1f50f53d27ae7b2b146aa5a8834a6710d6d7c912fb3388e46d68821090e/hosts",
        "LogPath": "/var/lib/docker/containers/a0dec1f50f53d27ae7b2b146aa5a8834a6710d6d7c912fb3388e46d68821090e/a0dec1f50f53d27ae7b2b146aa5a8834a6710d6d7c912fb3388e46d68821090e-json.log",
        "Name": "/quotes",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "shareable",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DiskQuota": 0,
            "KernelMemory": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": 0,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/asound",
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/f1df36d50bf30cc5a8484360099d685623fedaf0347f4aa6fc7ace89eb3b42e9-init/diff:/var/lib/docker/overlay2/dc0cf2d6c37c0312225d1670f52fa7bf9b222027aa8a5fdd234f8e8ce824633b/diff",
                "MergedDir": "/var/lib/docker/overlay2/f1df36d50bf30cc5a8484360099d685623fedaf0347f4aa6fc7ace89eb3b42e9/merged",
                "UpperDir": "/var/lib/docker/overlay2/f1df36d50bf30cc5a8484360099d685623fedaf0347f4aa6fc7ace89eb3b42e9/diff",
                "WorkDir": "/var/lib/docker/overlay2/f1df36d50bf30cc5a8484360099d685623fedaf0347f4aa6fc7ace89eb3b42e9/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "a0dec1f50f53",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "while :; do wget -qO- https://talaikis.com/api/quotes/random; printf '\\n'; sleep 5; done"
            ],
            "Image": "alpine",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "1411bafbb766afe6d11fe48d50a5dd8a74cb65ddf7e4a88ea66fd0336fbedeb0",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/1411bafbb766",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "e538b7e1dd9bae2bfa2debf5f0c7abe36e179b04223a264dba2fba470ccec402",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "909e5ecf30c2b5839a05d9bc14ecabc2e390f3fdb8bc83d3cec7a06ab45aa973",
                    "EndpointID": "e538b7e1dd9bae2bfa2debf5f0c7abe36e179b04223a264dba2fba470ccec402",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

The output has been` shortened` for readability.

Please take a moment to analyze what you got. You should see information such as:

- The ID of the container
- The creation date and time of the container
- From which image the container is built and so on

Many sections of the output, such as Mounts or NetworkSettings don't make much sense right now, but we will certainly discuss those in the upcoming chapters of the book. The data you're seeing here is also named the **metadata** of a container. We will be using the inspect command quite often in the remainder of the book as a source of information.

Sometimes, we need just a tiny bit of the overall information, and to achieve this, we can either use the **grep tool** or a **filter**. `The former method does not always result in the expected answer, so let's look into the latter approach`:

```
$ docker container inspect -f "{{json .State}}" quotes | jq 
```

The -f or --filter parameter is used to define the filter. The filter expression itself uses the **Go template** syntax. In this example, we only want to see the state part of the whole output in the JSON format.

To nicely format the output, we pipe the result into the jq tool:

```
    {
      "Status": "running",
      "Running": true,
      "Paused": false,
      "Restarting": false,
      "OOMKilled": false,
      "Dead": false,
      "Pid": 6759,
      "ExitCode": 0,
      "Error": "",
      "StartedAt": "2017-12-31T10:31:51.893299997Z",
      "FinishedAt": "0001-01-01T00:00:00Z"
    }
    
```

