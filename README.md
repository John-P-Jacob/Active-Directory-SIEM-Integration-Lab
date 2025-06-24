# Active Directory SIEM Integration Lab

## Objective

This comprehensive home lab project aimed to build a complete enterprise-grade Active Directory environment integrated with a SIEM solution for advanced threat detection and analysis. The primary focus was to create a realistic corporate network infrastructure, implement centralized logging with Splunk, and conduct controlled attack simulations to understand both offensive and defensive cybersecurity perspectives. This hands-on experience was designed to bridge the gap between theoretical knowledge and practical implementation of enterprise security monitoring.

### Skills Learned

- **Active Directory Architecture & Administration**: Complete setup and configuration of Windows Server 2022 domain controller with ADDS role
- **Enterprise SIEM Implementation**: Splunk Enterprise installation, configuration, and log analysis on Ubuntu Server
- **Advanced Endpoint Monitoring**: Sysmon deployment with Olaf Hartong's configuration for enhanced visibility
- **Network Infrastructure Design**: Custom NAT network configuration with proper DNS and static IP management  
- **Log Aggregation & Analysis**: Splunk Universal Forwarder deployment and custom inputs.conf configuration
- **Threat Simulation & Detection**: Atomic Red Team implementation for realistic attack scenario testing
- **Security Troubleshooting**: Complex multi-system debugging across Windows, Linux, and network components
- **Attack Pattern Recognition**: Analysis of brute force attacks and threat hunting methodologies
- **Service Account Security**: Proper configuration of service permissions for security tools

### Tools Used

- **Infrastructure**: VMware/VirtualBox for virtualization platform
- **Operating Systems**: Windows Server 2022 (Domain Controller), Windows 10 (Client), Ubuntu Server 22.04 LTS (SIEM), Kali Linux (Attack Platform)
- **Active Directory**: Windows Server ADDS, DNS Server, Group Policy Management
- **SIEM Platform**: Splunk Enterprise, Splunk Universal Forwarder
- **Endpoint Monitoring**: Sysmon with custom configuration, Windows Event Logs
- **Attack Simulation**: Atomic Red Team, Kali Linux penetration testing tools
- **Network Tools**: Custom NAT configuration (192.168.10.0/24), static IP management
- **Analysis Tools**: Splunk Search Processing Language (SPL), log correlation techniques

## Steps

### Step 1: Infrastructure Planning and Network Design
Designed a comprehensive network topology to simulate a realistic enterprise environment with proper segmentation and connectivity. The architecture consists of four main components connected via a custom NAT network (192.168.10.0/24):

- **Domain Controller (192.168.10.10)** - Windows Server 2022 running Active Directory Domain Services
- **Client Workstation (192.168.10.100)** - Windows 10 joined to the domain for realistic user simulation  
- **SIEM Server (192.168.10.20)** - Ubuntu Server hosting Splunk Enterprise for centralized logging
- **Attack Platform (192.168.10.50)** - Kali Linux for controlled penetration testing and attack simulation

### Step 2: Active Directory Domain Controller Deployment
Successfully promoted Windows Server 2022 to domain controller with comprehensive AD infrastructure configuration:

**Key Implementation Details:**
- Installed Active Directory Domain Services (ADDS) role through Server Manager
- Promoted server to domain controller creating new forest (cybersec.local domain)
- Configured integrated DNS server for proper domain name resolution
- Established Organizational Units (OUs) structure for logical separation of users and computers
- Created domain user accounts for testing authentication and access scenarios

### Step 3: Domain Client Integration and Configuration
Configured Windows 10 client workstation with proper network settings and successfully joined to the domain:

**Critical Configuration Steps:**
- Set Domain Controller (192.168.10.10) as primary DNS server for proper domain resolution
- Configured static IP addressing (192.168.10.100) for reliable network connectivity
- Resolved initial domain joining challenges by ensuring DNS was pointing to the Domain Controller
- Verified successful domain join by confirming computer account creation in Active Directory Users and Computers

### Step 4: Advanced Endpoint Monitoring with Sysmon
Implemented Sysmon across the Windows infrastructure using Olaf Hartong's advanced configuration for comprehensive endpoint visibility:

**Enhanced Logging Capabilities:**
- Process creation and termination events (Event ID 1)
- Network connections and DNS queries (Event ID 3, 22)
- File system modifications and registry changes (Event ID 11, 13)
- Image/library loading events (Event ID 7)
- Process access events for detecting credential dumping (Event ID 10)

This configuration provides security analysts with granular visibility into system activities that standard Windows Event Logs don't capture.

### Step 5: Splunk Enterprise SIEM Implementation
Deployed Splunk Enterprise on Ubuntu Server 22.04 LTS as the centralized Security Information and Event Management platform:

**SIEM Infrastructure Setup:**
- Installed Splunk Enterprise with appropriate licensing configuration
- Created custom indexes specifically for Windows Event Logs and Sysmon data
- Configured network settings to accept log forwarding from Windows systems
- Established proper user access controls and authentication mechanisms
- Optimized storage settings for efficient log retention and search performance

### Step 6: Universal Forwarder Deployment and Service Configuration
Deployed Splunk Universal Forwarders on Windows systems to enable centralized log collection with proper service permissions:

**Critical Technical Challenge Resolved:**
Initially encountered a significant issue where the Splunk Forwarder service was running under the default NT Service\SplunkForwarder account, which lacked sufficient permissions to access Windows Event Logs and Sysmon data. This prevented proper log collection and ingestion.

**Resolution Applied:**
- Changed the Splunk Universal Forwarder service to run under the Local System account
- This provided the necessary permissions to access all Windows Event Logs
- Verified successful log collection by monitoring Splunk ingestion queues
- Established reliable, continuous log forwarding from all Windows systems

### Step 7: Custom Log Collection Configuration
Developed specialized inputs.conf configuration files to collect critical Windows security logs and Sysmon telemetry:

**Log Sources Configured:**
- **Windows Security Event Logs**: Logon events (4624), failed logons (4625), privilege escalation (4672)
- **Sysmon Event Logs**: All 27 event types configured for comprehensive endpoint monitoring
- **System and Application Logs**: Additional Windows logs for broader system visibility
- **Custom Indexing Strategy**: Organized logs into dedicated indexes for efficient searching and analysis

This configuration ensures comprehensive coverage of security-relevant events across the entire Windows infrastructure.

### Step 8: Attack Simulation Framework with Atomic Red Team
Implemented the Atomic Red Team framework to generate realistic attack telemetry for detection testing and analyst training:

**Challenge Overcome - Windows Firewall Configuration:**
During initial Atomic Red Team implementation, encountered issues where security tests were blocked by Windows Firewall, preventing proper execution of attack simulations.

**Solution Implemented:**
- Configured necessary Windows Firewall exclusions to allow Atomic Red Team execution
- Balanced security testing requirements with system protection
- This experience provided deeper understanding of Windows security boundaries and their impact on security testing tools

**Attack Techniques Simulated:**
- MITRE ATT&CK technique implementations across multiple tactics
- Credential access techniques, persistence mechanisms, and defense evasion methods
- Generated diverse telemetry for analyst training and detection rule validation

### Step 9: Controlled Brute Force Attack Simulation
Conducted systematic brute force attacks from Kali Linux against Windows domain services to generate realistic attack telemetry:

**Attack Scenarios Executed:**
- **SMB Brute Force**: Used Hydra to attempt authentication against file shares
- **RDP Attack Simulation**: Targeted Remote Desktop Protocol with credential stuffing
- **Password Spraying**: Implemented low-and-slow password attacks against domain accounts
- Generated hundreds of failed authentication events (Event ID 4625) for analysis

These controlled attacks provided realistic log data for developing detection rules and understanding attack patterns from both offensive and defensive perspectives.

### Step 10: SIEM Analysis and Threat Hunting Development
Developed comprehensive Splunk searches, dashboards, and correlation rules to identify and analyze attack patterns:

**Advanced Analysis Techniques Implemented:**
- **SPL Query Development**: Created complex Search Processing Language queries for attack pattern identification
- **Timeline Analysis**: Developed correlation searches to track attack progression across multiple systems
- **Multi-Source Correlation**: Combined Windows Event Logs, Sysmon data, and network logs for comprehensive threat detection
- **Alert Creation**: Established automated alerting for critical security events and attack indicators
- **Dashboard Development**: Built executive and analyst-level dashboards for real-time security monitoring

### Step 11: Storage Optimization and Performance Tuning
Resolved critical infrastructure challenges related to disk space management and log ingestion performance:

**Technical Challenge - Disk Space Management:**
Identified significant disk space allocation problems on the Splunk server that were preventing proper log ingestion and causing performance degradation.

**Solutions Implemented:**
- **Index Configuration Optimization**: Configured appropriate retention policies for different log types
- **Storage Allocation**: Properly sized hot, warm, and cold buckets for efficient storage utilization
- **Log Rotation Policies**: Implemented automated log rotation and archival strategies
- **Performance Monitoring**: Established ongoing monitoring of disk usage and ingestion rates

This optimization ensured reliable, long-term operation of the SIEM infrastructure while maintaining comprehensive log coverage.

## Key Achievements and Lessons Learned

### Technical Mastery Gained
This project provided invaluable hands-on experience with enterprise-level security infrastructure:

- **Infrastructure Expertise**: Gained confidence in building and managing complex multi-system environments
- **Security Monitoring**: Developed practical skills in SIEM implementation and log analysis
- **Problem-Solving**: Enhanced troubleshooting abilities across Windows, Linux, and network components
- **Attack Analysis**: Learned to analyze security incidents from both offensive and defensive perspectives

### Real-World Applications
The skills developed directly translate to professional cybersecurity roles:
- **SOC Analyst Readiness**: Practical experience with SIEM platforms and log analysis
- **Incident Response**: Understanding of attack patterns and investigation techniques  
- **Infrastructure Security**: Knowledge of enterprise AD environments and security monitoring
- **Threat Detection**: Ability to identify and analyze malicious activities in network logs

### Future Enhancements
Potential expansions to further develop the lab environment:
- Integration of additional SOAR platforms (Shuffle, Tines)
- Implementation of threat intelligence feeds
- Advanced persistent threat (APT) simulation scenarios
- Integration with cloud security monitoring tools

## Conclusion

This Active Directory SIEM integration project successfully demonstrated the ability to build, secure, and monitor a realistic enterprise environment. The hands-on experience with industry-standard tools like Splunk, Active Directory, and Sysmon, combined with practical attack simulation and analysis, provides a solid foundation for cybersecurity roles focused on threat detection, incident response, and security operations center activities.

The project's emphasis on troubleshooting real-world challenges - from service permission issues to network configuration problems - mirrors the complex problem-solving requirements of professional cybersecurity environments.
