# MiniDFSCluster Docker Image

[![Docker Pulls](https://img.shields.io/docker/pulls/avs75alatau/minidfscluster)](https://hub.docker.com/r/avs75alatau/minidfscluster)
[![GitHub license](https://img.shields.io/github/license/avs-alatau/minidfscluster)](LICENSE)

This Docker image provides a local MiniDFSCluster environment to simplify testing HDFS-based applications such as Apache Spark.

---

## Features

- NameNode with fixed ports for both RPC and Web UI.
- Configurable number of DataNodes (default: 1, max: 9).
- Automatic creation of `/tmp` and `/user` directories in HDFS.
- Verified on Ubuntu, CentOS, Windows, and macOS.

---

## Installation

The Docker image is available on GitHub Packages:

[ghcr.io/avs-alatau/minidfscluster](https://ghcr.io/avs-alatau/minidfscluster)

### Pull the Image

```bash
docker pull ghcr.io/avs-alatau/minidfscluster:latest
```

---

## Running the Container

### On Linux

```bash
docker run -d --name hdfs-minicluster --network=host \
  -e NUM_DATANODES=3 ghcr.io/avs-alatau/minidfscluster:latest
```

### On Windows/macOS

```bash
docker run -d --name hdfs-minicluster \
  -p 35200:35200 -p 35100:35100 -p 30000-30030:30000-30030 \
  -e NUM_DATANODES=3 ghcr.io/avs-alatau/minidfscluster:latest
```

#### Notes

- The `NUM_DATANODES` environment variable is **optional** (default: 1).
- Maximum allowed `NUM_DATANODES`: **9**.

---

## Ports Information

| Service            | Port  |
|------------------- |------ |
| NameNode RPC       | 35200 |
| NameNode Web UI    | 35100 |
| DataNode RPC       | 30000-30030 |
| DataNode Transfer  | 30000-30030 |
| DataNode HTTP      | 30000-30030 |

---

## Usage

### Checking Logs

After starting the container, wait for about **30 seconds**, then check the logs:

```bash
docker logs hdfs-minicluster
```

This will display connection details and available ports.

### Connecting to HDFS

Use the full HDFS path for operations:

```bash
hdfs dfs -ls hdfs://localhost:35200/
```

### Pre-Created HDFS Directories

Upon startup, the following directories are created:

- `/tmp`
- `/user`

---

## Example: Running with Apache Spark

You can run `spark-shell` and connect to HDFS:

```bash
/opt/spark-3.5.1/bin/spark-shell --master local[*] \
  --conf spark.hadoop.fs.defaultFS=hdfs://localhost:35200
```

---

## Feedback and Contributions

For issues, improvements, or contributions, please visit the [GitHub repository](https://github.com/avs-alatau/minidfscluster).

---

Happy Testing!
