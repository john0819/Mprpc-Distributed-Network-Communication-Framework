# mprpc Distributed Network Communication Framework

**Note: Please do not copy without permission. Cite the source if referenced. Violators will be prosecuted!**

## Introduction

mprpc is a distributed network communication framework based on the high-performance Muduo network library and Protobuf. The project is named "mprpc" to reflect its design principles and components.

![image](https://github.com/john0819/Mprpc-Distributed-Network-Communication-Framework/assets/70586660/47c35531-81d5-454c-b557-82b6e144207d)


## Features

- **High Performance**: Built on the Muduo network library, ensuring efficient network communication.
- **Protobuf**: Utilizes Protobuf for data serialization and deserialization, offering cross-platform data exchange.
- **ZooKeeper**: Implements service discovery and coordination using ZooKeeper.
- **Cluster and Distributed Architecture**: Supports both clustered and distributed deployments.

## Technologies Used

- **Muduo**: A high-performance C++ network library.
- **Protobuf**: Google's Protocol Buffers for efficient data serialization.
- **ZooKeeper**: Distributed coordination service for service discovery.
- **CMake**: Build system to integrate and compile the project.
- **VScode**: Remote development in Linux environments.

## Architecture

### Cluster and Distributed Concepts

- **Cluster**: Each server runs all modules of the project independently.
- **Distributed**: The project is divided into multiple modules, each deployed independently on different servers. These servers collaborate to provide services, and each server is a node in the distributed system. Nodes can be further clustered based on concurrency requirements.

### RPC Communication Principle

- **RPC (Remote Procedure Call)**: Enables remote method invocation by packaging and parsing method parameters using Protobuf for data serialization.
- **Network**: Includes finding RPC service hosts, initiating RPC requests, and responding to RPC results using the Muduo network library and ZooKeeper for service discovery.

## Project Structure

- **bin**: Executable files
- **build**: Build files
- **lib**: Project libraries
- **src**: Source files
- **test**: Test codes
- **example**: Example usage of the framework
- **CMakeLists.txt**: Top-level CMake file
- **README.md**: Project documentation
- **autobuild.sh**: One-click build script

## Installation and Configuration

### Muduo Installation

To install Muduo, follow the instructions on [Muduo's installation guide](https://blog.csdn.net/QIANGWEIYUAN/article/details/89023980).

### Protobuf Installation

1. Download the source code from [GitHub](https://github.com/google/protobuf).
2. Follow the installation steps in the `README.md` in the `src` directory of the source code package:
    ```sh
    unzip protobuf-master.zip
    cd protobuf-master
    sudo apt-get install autoconf automake libtool curl make g++ unzip
    ./autogen.sh
    ./configure
    make
    sudo make install
    sudo ldconfig
    ```

### ZooKeeper Configuration

1. Download and extract ZooKeeper:
    ```sh
    tar -xzf zookeeper-3.4.10.tar.gz
    cd zookeeper-3.4.10
    cp conf/zoo_sample.cfg conf/zoo.cfg
    ./bin/zkServer.sh start
    ```

2. Verify the server is running:
    ```sh
    ./bin/zkCli.sh -server 127.0.0.1:2181
    ```

![image](https://github.com/john0819/Mprpc-Distributed-Network-Communication-Framework/assets/70586660/6795d579-1fa1-4245-afb3-2a92f529bcbc)


## Usage

### Example: Running the mprpc Framework

1. Initialize the framework:
    ```sh
    MprpcApplication::Init(argc, argv);
    ```

2. Register services:
    ```cpp
    RpcProvider provider;
    provider.NotifyService(new UserService());
    provider.Run();
    ```

3. Compile and run the project using the provided `autobuild.sh` script.

### Example Services

Define your RPC services using Protobuf:
```proto
syntax = "proto3";

package fixbug;

service UserServiceRpc {
    rpc login(LoginRequest) returns (Response);
    rpc reg(RegRequest) returns (Response);
}

message LoginRequest {
    string name = 1;
    string pwd = 2;
}

message RegRequest {
    string name = 1;
    string pwd = 2;
    int32 age = 3;
    enum SEX {
        MAN = 0;
        WOMAN = 1;
    }
    SEX sex = 4;
    string phone = 5;
}

message Response {
    int32 errorno = 1;
    string errormsg = 2;
    bool result = 3;
}

