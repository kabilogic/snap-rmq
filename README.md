# snap-rmq

A self-contained Snap package that runs a C# RabbitMQ producer to send messages over the network.

## Overview

This project demonstrates how to:

- Build a RabbitMQ message producer using C#
- Publish it as a Snap package using Snapcraft
- Run the Snap from any Linux system and send a message to a RabbitMQ server

---

## Prerequisites

- [.NET SDK 8.0+](https://dotnet.microsoft.com/en-us/download)
- [Snapcraft](https://snapcraft.io/docs/installing-snapcraft)
- A working [RabbitMQ server](https://www.rabbitmq.com/download.html)

---

## Project Structure

```bash
snap-rmq/
├── rmqSnap/                # C# console app
│   ├── Program.cs
│   ├── rmqSnap.csproj
│   └── publish/            # Output of dotnet publish
├── snapcraft.yaml          # Snap build config
```
### 1. Clone the Repo
```
git clone https://github.com/your-username/snap-rmq.git
cd snap-rmq/rmqSnap
```
### 2. Install RabbitMQ Client Package
```
dotnet add package RabbitMQ.Client
```
### 3. Publish the App
```
dotnet clean
dotnet build
dotnet publish -c Release -r linux-x64 --self-contained true -o publish /p:PublishSingleFile=true /p:PublishTrimmed=true
```
## Snapcraft Build
From the root ```snap-rmq/``` directory:
```
snapcraft
```
This creates:
```
snap-rmq_1.0.0_amd64.snap
```
Install it locally:
```
sudo snap install snap-rmq_1.0.0_amd64.snap --dangerous
```
## Usage
Run the Snap:
```
snap run snap-rmq
```
Expected output:
```
 [x] Sent Hello from snap!
```
Make sure your RabbitMQ server is reachable at the IP specified in Program.cs.
## RabbitMQ Setup
You can verify the message with a basic consumer or using the RabbitMQ UI at:
http://<your-server-ip>:15672

## Troubleshooting
```
rmqSnap.dll not found	- Make sure you publish with --self-contained
libicu.so not found	  - Add libicu70 under stage-packages in snapcraft.yaml
Snap doesn't run	    - Verify file is placed in bin/ in snapcraft.yaml organize section
Network unreachable	  - Ensure RabbitMQ server IP is accessible from your device
