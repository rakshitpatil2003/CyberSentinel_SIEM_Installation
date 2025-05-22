# CyberSentinel_SIEM_Installation
this is a installation repository for the complete installation of our Cyber Sentinel SIEM Solution

# CyberSentinel SIEM Installation Suite

Professional automated installation script for the complete CyberSentinel Security Information and Event Management (SIEM) platform.

## 🚀 Quick Start

```bash
# Download and run the installation script
curl -sSL https://raw.githubusercontent.com/rakshitpatil2003/CyberSentinel_SIEM_Installation/main/install_cybersentinel.sh | sudo bash
```

## 📋 What Gets Installed

### Core Infrastructure Components
- **CyberSentinel Endpoint Processor** (Kafka + Zookeeper) - Message queue system
- **CyberSentinel Database** (OpenSearch) - Log storage and analysis engine  
- **CyberSentinel Database Dashboard** (OpenSearch Dashboards) - Data visualization
- **CyberSentinel Secret Vault** (HashiCorp Vault) - Secrets management
- **CyberSentinel Manual Remediation** (JumpServer) - Secure access management

### Application Components
- **Node.js Runtime** (v22.x) - Application execution environment
- **PM2 Process Manager** - Service management and monitoring
- **CyberSentinel SIEM Platform** - Main security dashboard and analytics

## 🖥️ System Requirements

### Minimum Requirements
- **OS**: Ubuntu 18.04+ (64-bit)
- **RAM**: 4GB (8GB recommended)
- **Storage**: 20GB free space (50GB+ recommended)
- **Network**: Internet connection for downloads
- **Privileges**: Root access (sudo)

### Recommended Specifications
- **CPU**: 4+ cores
- **RAM**: 8GB+
- **Storage**: 100GB+ SSD
- **Network**: Stable broadband connection

## 📁 Repository Structure

You need to create this structure in your `CyberSentinel_SIEM_Installation` repository:

```
CyberSentinel_SIEM_Installation/
├── install_cybersentinel.sh          # Main installation script
├── logos/                            # JumpServer customization logos
│   ├── 125_x_18.png                 # Logo for header (125x18 pixels)
│   ├── 30_x_40.png                  # Small logo (30x40 pixels)  
│   ├── front_logo.png               # Login screen logo
│   └── favicon_logo.ico             # Browser favicon
└── README.md                        # This file
```

## 🛠️ Manual Installation Steps

### Step 1: Prepare Your Server
```bash
# Update your system
sudo apt update && sudo apt upgrade -y

# Ensure you have curl and git
sudo apt install -y curl git

# Check available space
df -h
```

### Step 2: Download Installation Script
```bash
# Create installation directory
sudo mkdir -p /opt/cybersentinel

# Download the script
sudo curl -o /opt/cybersentinel/install_cybersentinel.sh \
  https://raw.githubusercontent.com/rakshitpatil2003/CyberSentinel_SIEM_Installation/main/install_cybersentinel.sh

# Make it executable
sudo chmod +x /opt/cybersentinel/install_cybersentinel.sh
```

### Step 3: Run Installation
```bash
# Run the installation script
sudo /opt/cybersentinel/install_cybersentinel.sh
```

### Step 4: Configure Graylog Integration
During installation, you'll be prompted for:
- **CyberSentinel Container HOST**: Your Graylog server IP (e.g., 192.168.1.68)
- **CyberSentinel Container PORT**: Graylog port (default: 9000)
- **CyberSentinel Container USERNAME**: Graylog username
- **CyberSentinel Container PASSWORD**: Graylog password
- **CyberSentinel Container STREAM_ID**: Graylog stream identifier

## 🎮 Service Management

After installation, use these commands to manage CyberSentinel:

```bash
# Start all services
cybersentinel start

# Stop all services  
cybersentinel stop

# Restart all services
cybersentinel restart

# Check service status
cybersentinel status

# View service logs
cybersentinel logs

# Monitor services in real-time
cybersentinel monitor
```

## 🌐 Access Points

After successful installation and startup:

| Service | URL | Default Credentials |
|---------|-----|-------------------|
| **CyberSentinel Dashboard** | `http://YOUR_IP:3000` | admin:admin |
| **Manual Remediation** | `http://YOUR_IP:80` | admin:ChangeMe |
| **Database Dashboard** | `http://YOUR_IP:5601` | No authentication |
| **Secret Vault** | `http://YOUR_IP:8200` | Token: cybersentinel-vault-token |

## 🔧 Configuration Files

### Environment Files Location
- **Backend**: `/opt/cybersentinel/SECURITY-LOG-MANAGER/backend/.env`
- **Frontend**: `/opt/cybersentinel/SECURITY-LOG-MANAGER/frontend/.env`

### Service Configuration
- **PM2 Ecosystem**: `/opt/cybersentinel/SECURITY-LOG-MANAGER/ecosystem.config.js`
- **Docker Compose**: `/opt/cybersentinel/infrastructure/docker-compose.yml`

### Log Files Location
- **Application Logs**: `/var/log/cybersentinel/`
- **Service Logs**: `cybersentinel logs`

## 🚨 Troubleshooting

### Installation Issues

**Port Already in Use**
```bash
# Check what's using the port
sudo netstat -tulpn | grep :PORT_NUMBER

# Kill the process if safe to do so
sudo kill -9 PID_NUMBER
```

**Docker Issues**
```bash
# Restart Docker service
sudo systemctl restart docker

# Check Docker status
sudo systemctl status docker

# View Docker logs
sudo journalctl -u docker.service
```

**Permission Issues**
```bash
# Ensure script is run as root
sudo -i
/opt/cybersentinel/install_cybersentinel.sh
```

### Service Issues

**Services Won't Start**
```bash
# Check individual service status
cybersentinel status

# Check Docker containers
sudo docker ps -a

# Restart infrastructure
cd /opt/cybersentinel/infrastructure
sudo docker-compose restart
```

**Frontend Not Accessible**
```bash
# Check if Node.js services are running
cybersentinel status

# Restart frontend specifically
pm2 restart cybersentinel-frontend-ui

# Check frontend logs
pm2 logs cybersentinel-frontend-ui
```

### Network Issues

**Cannot Access Dashboard**
```bash
# Check firewall status
sudo ufw status

# Allow necessary ports (if firewall is enabled)
sudo ufw allow 3000/tcp  # Dashboard
sudo ufw allow 80/tcp    # JumpServer
sudo ufw allow 5601/tcp  # OpenSearch Dashboard
sudo ufw allow 8200/tcp  # Vault
```

## 📞 Support

### Log Analysis
View detailed logs for troubleshooting:
```bash
# View all service logs
cybersentinel logs

# View specific service logs
pm2 logs cybersentinel-backend-api
pm2 logs cybersentinel-frontend-ui
pm2 logs cybersentinel-data-collector

# View infrastructure logs
cd /opt/cybersentinel/infrastructure
sudo docker-compose logs
```

### Health Checks
```bash
# Check system resources
htop
df -h
free -h

# Check service ports
sudo netstat -tulpn | grep -E ':(3000|5000|5601|8200|9200|29092|80)'

# Test database connectivity
curl -X GET "localhost:9200/_cluster/health?pretty"

# Test vault connectivity  
curl -X GET "localhost:8200/v1/sys/health"
```

## 🔐 Security Recommendations

### Post-Installation Security
1. **Change Default Passwords**
   - CyberSentinel Dashboard: admin:admin
   - Manual Remediation: admin:ChangeMe

2. **Configure Firewall Rules**
   ```bash
   # Allow only necessary ports
   sudo ufw enable
   sudo ufw allow ssh
   sudo ufw allow 3000/tcp  # Dashboard
   sudo ufw allow 80/tcp    # JumpServer
   ```

3. **SSL/TLS Configuration**
   - Configure reverse proxy (nginx/apache)
   - Install SSL certificates
   - Redirect HTTP to HTTPS

4. **Regular Updates**
   ```bash
   # Update system packages
   sudo apt update && sudo apt upgrade -y
   
   # Update Docker images
   cd /opt/cybersentinel/infrastructure
   sudo docker-compose pull
   sudo docker-compose up -d
   ```

## 📈 Performance Tuning

### For High-Volume Environments
1. **Increase Memory Allocation**
   - Edit OpenSearch memory in docker-compose.yml
   - Adjust PM2 memory limits in ecosystem.config.js

2. **Storage Optimization**
   - Use SSD storage for databases
   - Configure log rotation
   - Set up automated cleanup

3. **Network Optimization**
   - Use dedicated network interfaces
   - Configure quality of service (QoS)
   - Monitor bandwidth usage

## 🔄 Backup and Recovery

### Backup Important Data
```bash
# Backup configuration files
sudo tar -czf cybersentinel-config-backup.tar.gz \
  /opt/cybersentinel/SECURITY-LOG-MANAGER/backend/.env \
  /opt/cybersentinel/SECURITY-LOG-MANAGER/frontend/.env \
  /opt/cybersentinel/SECURITY-LOG-MANAGER/ecosystem.config.js

# Backup OpenSearch data
sudo docker exec cybersentinel-database \
  curl -X PUT "localhost:9200/_snapshot/backup_repo" \
  -H 'Content-Type: application/json' \
  -d '{"type": "fs", "settings": {"location": "/usr/share/opensearch/backup"}}'
```

### Recovery Procedures
```bash
# Restore from backup
sudo tar -xzf cybersentinel-config-backup.tar.gz -C /

# Restart services
cybersentinel restart
```

---

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📧 Contact

For support and questions, please open an issue in the GitHub repository.

---

**⚠️ Important Note**: This installation script is designed for development and testing environments. For production deployments, additional security hardening and configuration may be required.
