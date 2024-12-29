# Setting Up a Three-Node Kafka KRaft Cluster on AWS EC2 Instances

Apache Kafka is a popular distributed event-streaming platform. This guide outlines the steps to set up a **three-node Kafka KRaft cluster** on **AWS EC2 instances**. This configuration is suitable for both development and small-scale production environments.

> **Important Note:** EC2 instances are chargeable. Terminate them after use to avoid unexpected costs.

---

## **Prerequisites**

1. **AWS Account**: Ensure you have access to an AWS account.
2. **EC2 Instances**:
   - Launch **three EC2 instances** of type `t2.medium`.
   - Configure the security group to allow **inbound traffic** on ports `9092` and `9093`.

---

## **Step-by-Step Guide**

### **1. Prepare the EC2 Instances**

Perform the following steps on **each EC2 instance**:

#### Update the package list:
```bash
sudo apt update
```

#### Install OpenJDK 11:
```bash
sudo apt install openjdk-11-jdk -y
java -version
```

#### Download and extract Kafka:
```bash
wget https://archive.apache.org/dist/kafka/3.0.0/kafka_2.13-3.0.0.tgz
tar -xzf kafka_2.13-3.0.0.tgz
```

#### Backup the default configuration:
```bash
mv kafka_2.13-3.0.0/config/kraft/server.properties kafka_2.13-3.0.0/config/kraft/server.properties.bak
```

---

### **2. Configure Kafka on Each Node**

Edit the Kafka configuration file:
```bash
vim kafka_2.13-3.0.0/config/kraft/server.properties
```

Replace the content with the following. Update `<IP_address>` with the respective instance's private IP addresses for each node.

#### Node 1:
```properties
node.id=1
advertised.listeners=PLAINTEXT://<IP_address1>:9092,PLAINTEXT_HOST://<IP_address1>:9092
controller.quorum.voters=1@<IP_address1>:9093,2@<IP_address2>:9093,3@<IP_address3>:9093
transaction.state.log.replication.factor=3
listener.security.protocol.map=CONTROLLER:PLAINTEXT,PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
inter.broker.listener.name=PLAINTEXT
controller.listener.names=CONTROLLER
offsets.topic.replication.factor=3
listeners=PLAINTEXT://:9092,CONTROLLER://:9093,PLAINTEXT_HOST://:9092
group.initial.rebalance.delay.ms=0
process.roles=broker,controller
num.partitions=3
log.dirs=/tmp/kraft-combined-logs
```

Repeat the configuration for Node 2 and Node 3, updating `node.id` and `advertised.listeners IP_address`.

---

### **3. Initialize Kafka Storage**

On **one server only**, generate a unique ID for the cluster:
```bash
kafka_2.13-3.0.0/bin/kafka-storage.sh random-uuid
```

Copy the generated UUID and run the following command on **each node**, replacing `<UUID>` with the generated value:
```bash
kafka_2.13-3.0.0/bin/kafka-storage.sh format -t <UUID> -c kafka_2.13-3.0.0/config/kraft/server.properties
```

---

### **4. Start the Kafka Cluster**

Start Kafka on all three nodes:
```bash
kafka_2.13-3.0.0/bin/kafka-server-start.sh -daemon kafka_2.13-3.0.0/config/kraft/server.properties
```

---

### **5. Testing the Setup**

1. **Create a Topic**:
   ```bash
   kafka_2.13-3.0.0/bin/kafka-topics.sh - create - topic Test1 - partitions 3 - replication-factor 3 - bootstrap-server <IP1>:9092,<IP2>:9092,<IP3>:9092
   ```

2. **Describe the Topic**:
   ```bash
   kafka_2.13-3.0.0/bin/kafka-topics.sh - describe - topic Test1 - bootstrap-server <IP1>:9092,<IP2>:9092,<IP3>:9092
   ```

3. **Send Messages**:
   ```bash
   kafka_2.13-3.0.0/bin/kafka-console-producer.sh - topic Test1 - bootstrap-server <IP1>:9092,<IP2>:9092,<IP3>:9092
   ```

4. **Consume Messages**:
   ```bash
   kafka_2.13-3.0.0/bin/kafka-console-consumer.sh - topic Test1 - from-beginning - bootstrap-server <IP1>:9092,<IP2>:9092,<IP3>:9092
   ```

---

### **Optional: Run Kafka as a Systemd Service**

Create a service file:
```bash
sudo nano /etc/systemd/system/kafka.service
```

Add the following content:
```ini
[Unit]
Description=Apache Kafka
After=network.target

[Service]
User=ubuntu
Group=ubuntu
ExecStart=/home/ubuntu/kafka_2.13-3.0.0/bin/kafka-server-start.sh /home/ubuntu/kafka_2.13-3.0.0/config/kraft/server.properties
ExecStop=/home/ubuntu/kafka_2.13-3.0.0/bin/kafka-server-stop.sh
Restart=on-failure
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

Enable and start the service:
```bash
sudo systemctl daemon-reload
sudo systemctl enable kafka.service
sudo systemctl start kafka.service
sudo systemctl status kafka.service
```

---

### **Conclusion**

Setting up a Kafka KRaft cluster can feel challenging, but with careful planning and execution, you'll have a robust event-streaming system up and running. Always remember to terminate your EC2 instances when they're no longer needed to avoid unnecessary costs.

**Happy streaming with Kafka!**
