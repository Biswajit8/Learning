https://www.youtube.com/watch?v=1dgRD5NJoaE

## Overview of Hadoop Architecture
- Hadoop architecture follows a master-slave distributed model.
- It is designed to handle huge volumes of data efficiently.
- Data storage & data processing are done on different layers.
- Hadoop architecture is divided into four main layers.
  1. Storage layer (HDFS)
  2. Resource management layer (YARN) - manages CPU & memory
  3. Processing layer (MapReduce) - calculates results
  4. Utility layer (Hadoop Common) - provides all the tools

## Hadoop Architecture Diagram
screenshot 2:34

## HDFS Architecture (Storage Layer)
- To store very large files reliably.
- To split data and distribute it across multiple machines.
- To ensure fault tolerance using replication.

## HDFS Components
- NameNode (MASTER NODE)
- DataNode (SLAVE NODES)
- Secondary NameNode (Support Node) - supports DataNode

## NameNode (Master Node)
- NameNode is the central controller of HDFS.
- It stores metadata only, not actual data.
- Metadata includes:
  - File names

## NameNode Metadata Details
- Directory structure
- Block IDs
- Block locations
- Maintains mapping:

## NameNode Mapping
- File → Blocks → DataNodes
- Decides where data blocks should be stored.
- Handles client requests like:
  - File creation

## NameNode Client Requests
- File deletion
- File permission checks
- If NameNode crashes, entire HDFS becomes unavailable.

## DataNode (Slave Nodes)
- DataNodes store the actual data blocks.
- They are installed on every worker machine.
- Perform read and write operations.
- Periodically send heartbeat signals to NameNode.

## DataNode Responsibilities
- Send block reports showing stored blocks.
- Replicate data blocks as instructed by NameNode.
- Failure of a DataNode does not cause data loss due to replication.

## Secondary NameNode
- Periodically takes snapshots of metadata.
- Merges edit logs with file system image.
- Helps reduce NameNode recovery time.
- It is not a backup NameNode.

## HDFS Architecture Diagram
screenshot 12:58

## YARN Architecture (Resource Management Layer)
- To manage cluster resources like CPU and memory.
- To schedule and monitor running applications.
- To allow multiple processing frameworks to run.

## YARN Architecture
screenshot 14:29

## YARN Components
- ResourceManager
- NodeManager
- ApplicationMaster

## ResourceManager
- Central authority of YARN.
- Manages all cluster resources.
- Decides which job gets how much CPU and memory.
- Schedules applications.

## NodeManager
- Runs on every worker node.
- Manages local resources of that node.
- Launches containers for tasks.
- Reports resource usage to ResourceManager.

## ApplicationMaster
- Created for each application.
- Negotiates resources with ResourceManager.
- Monitors execution of tasks.
- Handles failures and restarts.

## MapReduce Architecture (Processing Layer)
- To process huge datasets in parallel.
- To bring computation close to data.
- To reduce processing time.

## MapReduce Phases
- Map Phase
- Shuffle and Sort Phase
- Reduce Phase

## Map Phase
- Input data is split into blocks.
- Each block is processed independently.
- Mapper converts data into key-value pairs.
- Mapping runs in parallel on multiple nodes.

## Shuffle and Sort Phase
- Intermediate key-value pairs are grouped.
- Same keys are sent to the same reducer.
- Data is sorted before reducing.

## Reduce Phase
- Reducer aggregates values.
- Produces final result.
- Output is written back to HDFS.

## MapReduce Architecture Diagram
screenshot 20:57

## Hadoop Common (Utility Layer)
- Contains common Java libraries.
- Provides APIs, configuration files, and utilities.
- Required for HDFS, YARN, and MapReduce to work.
- Acts as the foundation of Hadoop.

## Complete Flow Summary (Exam Gold)
- Client stores data in HDFS.
- NameNode manages metadata.
- DataNodes store data.
- YARN manages resources.
- MapReduce processes data.
- Results stored back in HDFS.
