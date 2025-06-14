# Kafka Development Environment with Docker Compose

This repository provides a complete Apache Kafka development environment using Docker Compose. It includes Kafka, Zookeeper, and **Kafka UI** for easy visual management and monitoring of your Kafka cluster through a web interface.

## What This Does

This Docker Compose setup creates a local Kafka cluster with the following components:

- **Apache Kafka**: A distributed streaming platform for building real-time data pipelines
- **Apache Zookeeper**: Coordination service required by Kafka for managing cluster metadata
- **Kafka UI**: A modern, user-friendly web-based interface for managing and monitoring your Kafka cluster without command-line complexity

## Services Overview

### Zookeeper
- **Port**: 2181
- **Purpose**: Manages Kafka cluster coordination and configuration
- **Image**: `confluentinc/cp-zookeeper:7.4.0`

### Kafka Broker
- **Port**: 9092 (external), 29092 (internal)
- **Purpose**: The main Kafka message broker
- **Image**: `confluentinc/cp-kafka:7.4.0`
- **Features**: Auto-topic creation enabled, single broker setup

### Kafka UI
- **Port**: 8080
- **Purpose**: Modern web interface for managing Kafka topics, messages, and consumers
- **Image**: `provectuslabs/kafka-ui:latest`
- **Access**: http://localhost:8080
- **Features**: Visual topic management, message browsing, consumer group monitoring, schema registry support

## Prerequisites

- Docker installed on your system
- Docker Compose installed on your system
- At least 4GB of available RAM (recommended)

### Docker Installation
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (Windows/macOS)
- Docker Engine (Linux)

## How to Use

### 1. Clone the Repository
```bash
git clone <your-repo-url>
cd kafka-docker-dev-environment-with-ui
```

### 2. Start the Kafka Environment
```bash
docker-compose -f kafka.yml up -d
```

This command will:
- Download the required Docker images (if not already present)
- Start Zookeeper first
- Start Kafka broker
- Start Kafka UI

### 3. Verify the Setup
Check that all services are running:
```bash
docker-compose -f kafka.yml ps
```

You should see all three services in "Up" status.

### 4. Access Kafka UI
Open your web browser and navigate to:
```
http://localhost:8080
```

**Kafka UI provides a visual interface where you can:**
- View and create topics with ease
- Browse and search through messages
- Monitor consumer groups and their lag
- View broker information and cluster health
- Manage topic configurations
- Send test messages directly from the UI

### 5. Connect Applications
Your applications can connect to Kafka using:
- **Bootstrap servers**: `localhost:9092`
- **Zookeeper**: `localhost:2181` (if needed)

## Common Operations

### Create a Topic

**Windows Command Prompt:**
```cmd
docker exec -it kafka kafka-topics --create --bootstrap-server localhost:9092 --topic my-topic --partitions 3 --replication-factor 1
```

**Windows PowerShell:**
```powershell
docker exec -it kafka kafka-topics --create `
  --bootstrap-server localhost:9092 `
  --topic my-topic `
  --partitions 3 `
  --replication-factor 1
```

**Linux/macOS/Git Bash:**
```bash
docker exec -it kafka kafka-topics --create \
  --bootstrap-server localhost:9092 \
  --topic my-topic \
  --partitions 3 \
  --replication-factor 1
```

### List Topics

**Windows Command Prompt:**
```cmd
docker exec -it kafka kafka-topics --list --bootstrap-server localhost:9092
```

**Linux/macOS/Git Bash:**
```bash
docker exec -it kafka kafka-topics --list --bootstrap-server localhost:9092
```

### Produce Messages

**Windows Command Prompt:**
```cmd
docker exec -it kafka kafka-console-producer --bootstrap-server localhost:9092 --topic my-topic
```

**Linux/macOS/Git Bash:**
```bash
docker exec -it kafka kafka-console-producer --bootstrap-server localhost:9092 --topic my-topic
```

### Consume Messages

**Windows Command Prompt:**
```cmd
docker exec -it kafka kafka-console-consumer --bootstrap-server localhost:9092 --topic my-topic --from-beginning
```

**Linux/macOS/Git Bash:**
```bash
docker exec -it kafka kafka-console-consumer --bootstrap-server localhost:9092 --topic my-topic --from-beginning
```

### View Logs

**Windows Command Prompt:**
```cmd
# View all services logs
docker-compose -f kafka.yml logs

# View specific service logs
docker-compose -f kafka.yml logs kafka
docker-compose -f kafka.yml logs zookeeper
docker-compose -f kafka.yml logs kafka-ui
```

**Linux/macOS/Git Bash:**
```bash
# View all services logs
docker-compose -f kafka.yml logs

# View specific service logs
docker-compose -f kafka.yml logs kafka
docker-compose -f kafka.yml logs zookeeper
docker-compose -f kafka.yml logs kafka-ui
```

## Stopping the Environment

### Stop Services (keeps containers)

**Windows Command Prompt:**
```cmd
docker-compose -f kafka.yml stop
```

**Linux/macOS/Git Bash:**
```bash
docker-compose -f kafka.yml stop
```

### Stop and Remove Containers

**Windows Command Prompt:**
```cmd
docker-compose -f kafka.yml down
```

**Linux/macOS/Git Bash:**
```bash
docker-compose -f kafka.yml down
```

### Clean Up Everything (including volumes)

**Windows Command Prompt:**
```cmd
docker-compose -f kafka.yml down -v
```

**Linux/macOS/Git Bash:**
```bash
docker-compose -f kafka.yml down -v
```

## Configuration Notes

- **Single Broker Setup**: This configuration uses only one Kafka broker, suitable for development
- **Auto-Topic Creation**: Topics will be created automatically when first accessed
- **Replication Factor**: Set to 1 (minimum for single broker)
- **Data Persistence**: Kafka data is stored in Docker volumes and persists between container restarts

## Troubleshooting

### Port Conflicts
If ports 2181, 8080, or 9092 are already in use, modify the port mappings in `kafka.yml`:
```yaml
ports:
  - "9093:9092"  # Use 9093 instead of 9092
```

### Connection Issues
- Ensure Docker is running
- Check that no firewall is blocking the ports
- Verify all containers are healthy: `docker-compose -f kafka.yml ps`

### Command Syntax Issues
- **Windows Command Prompt**: Use single-line commands (no line continuation)
- **Windows PowerShell**: Use backtick `` ` `` for line continuation
- **Linux/macOS/Git Bash**: Use backslash `\` for line continuation

## Production Considerations

This setup is designed for development and testing. For production use, consider:

- Multiple Kafka brokers for high availability
- Proper security configuration (SSL, SASL)
- Resource limits and monitoring
- Data backup strategies
- Network security and access controls

## License

This configuration is provided as-is for development purposes. Please refer to the individual component licenses:
- [Apache Kafka License](https://kafka.apache.org/license)
- [Confluent Platform License](https://www.confluent.io/confluent-community-license/)
- [Kafka UI License](https://github.com/provectus/kafka-ui/blob/master/LICENSE)
