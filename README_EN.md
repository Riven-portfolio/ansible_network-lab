# Ansible Network Lab - Enterprise Network Automation Deployment

[![Ansible](https://img.shields.io/badge/Ansible-2.10%2B-red.svg)](https://www.ansible.com/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![Cisco IOS](https://img.shields.io/badge/Cisco-IOS%2F%20IOL-005073.svg)](https://www.cisco.com/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![GitHub](https://img.shields.io/badge/GitHub-Repository-black.svg)](https://github.com/Riven-portfolio/ansible_network-lab)

> **Project Type**: Personal Learning Project
> A comprehensive journey from manual Cisco ENARSI lab configuration to fully automated network deployment using Ansible and Infrastructure as Code (IaC) principles.

## Project Background

### Learning Journey

During preparation for the **Cisco ENARSI** (Implementing Cisco Enterprise Advanced Routing and IP Services) certification, I completed a comprehensive enterprise network lab featuring HSRP, OSPF, BGP, and GRE tunnel implementations (detailed in the [Lab Guide PDF](Enterprise%20Network%20Lab%20Guide.pdf) included in this project).

### The Problem

Manual configuration of this 10-device lab environment revealed significant challenges:
- **Time-Consuming**: Full configuration required 2-3 hours
- **Error-Prone**: A single typo would trigger lengthy debugging sessions
- **Hard to Reproduce**: Redeploying the environment was incredibly tedious
- **Configuration Sprawl**: Configurations for 10 devices scattered across various locations, making management difficult

### The Solution: Network as Code

I decided to learn **Ansible** and transform this manual lab into an automated deployment pipeline:
- **85% Faster Deployment**: From 2-3 hours down to just 15 minutes
- **Zero Configuration Drift**: Idempotency ensures configuration consistency
- **Reproducible Environments**: Rebuild the entire lab with a single command
- **Version-Controlled**: Git-managed configurations for audit trails and rollback capabilities

### Learning Objectives

This project demonstrates:
1. **Network Knowledge Deepening**: Understand OSPF, BGP, HSRP dynamics through automation
2. **IaC Best Practices**: Master Infrastructure as Code principles and patterns
3. **Automation Mindset Shift**: From "manual operations" to "programmatic management"
4. **Career Foundation Building**: Prepare for network automation and SRE roles

## Core Features

### Automation Capabilities

- **Infrastructure as Code (IaC)**: All network configurations managed declaratively via YAML
- **Modular Architecture**: 8 independent Playbooks (S0-S8) for composable deployments
- **Idempotency Guarantees**: Repeat executions produce no errors; ensures configuration convergence
- **GitOps Implementation**: Version control and audit trails for all configuration changes
- **Automated Validation**: Built-in post-deployment verification ensures service health

### Enterprise-Grade Network Features

- **HSRP High Availability**: Dual-router redundancy (R1/R2) with automatic failover
- **Multi-Area OSPF**: Area 0/10/50 design with Stub areas and MD5 authentication
- **BGP External Connectivity**: Dual-ISP redundancy (AS 65001/65002)
- **GRE VPN Tunnels**: Secure HQ-to-Branch connectivity
- **VLAN Segmentation**: Multi-segment management (User/IT/DMZ/Log)
- **Security Hardening**: DMZ ACLs and NAT configurations

### Integrated Skillset Demonstration

This project showcases:
- **Network Expertise**: OSPF, BGP, HSRP, GRE, and VLAN design and implementation
- **Automation Proficiency**: Ansible Playbook design, variable management, error handling
- **System Architecture**: Modular design, staged deployments, maintainability optimization
- **DevOps Mindset**: GitOps workflows, declarative configuration, reproducible deployments

## Network Topology Architecture

This project implements a complete enterprise network environment with **10 devices** based on the **Enterprise Network Lab Guide**:

### Device Configuration

- **Headquarters (HQ)**: R1, R2 (HSRP active/standby), R3 (DMZ/ABR), SW1
- **Branch Office**: BR1, BR-SW, Host-BR
- **ISP Routers**: ISP1 (AS65001), ISP2 (AS65002)
- **Servers**: WebSrv (DMZ)
  - **Note**: LogSrv is currently undeployed; VLAN 60 is reserved for future use

### Network Planning

```
HQ VLAN:
  - VLAN 10 (User):    10.10.10.0/24  → VIP: 10.10.10.254
  - VLAN 20 (IT):      10.10.20.0/24  → VIP: 10.10.20.254
  - VLAN 50 (DMZ):     10.10.50.0/24  → GW:  10.10.50.1
  - VLAN 60 (Log):     10.10.60.0/24  → VIP: 10.10.60.254
  - VLAN 99 (Transit): 10.10.99.0/24

Branch:
  - VLAN 110:          10.110.10.0/24 → GW:  10.110.10.254

Tunnel:
  - GRE Overlay:       172.16.10.0/30
  - Underlay:          100.64.1.0/30
```

## Project Outcomes

### Automation Benefits

| Metric | Manual Configuration | Automated Deployment | Improvement |
|--------|----------------------|----------------------|-------------|
| **Deployment Time** | 2-3 hours | 15 minutes | 88% reduction |
| **Configuration Error Rate** | Manual mistakes | Zero errors | 100% improvement |
| **Configuration Consistency** | Difficult to guarantee | Fully consistent | 100% guaranteed |
| **Change Traceability** | No records | Complete Git history | Fully auditable |

### Technical Capability Demonstration

✅ **Network Automation**: Ansible-managed Cisco devices with cross-device orchestration
✅ **IaC Implementation**: Declarative configuration enabling reproducible deployments
✅ **Modular Design**: 8 independent playbooks with clear separation of concerns
✅ **GitOps Workflow**: Version control + audit trails for all changes
✅ **Validation Mechanisms**: Automated testing ensures configuration correctness

### Laboratory Verification

- ✅ Full validation in EVE-NG simulation environment
- ✅ High-availability testing (HSRP failover scenarios)
- ✅ Routing protocol convergence verification (OSPF, BGP)
- ✅ VPN connectivity testing (GRE tunnel establishment)

## Quick Start

### Prerequisites

- **Python** 3.8+
- **Ansible** 2.10+
- **Cisco IOS Devices** (IOL/VIRL/EVE-NG)
- **RAM**: ~2.5-3 GB (L3: 256-384MB per device, L2: 192MB per switch)

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/Riven-portfolio/ansible_network-lab.git
cd ansible_network-lab
```

2. **Install dependencies**
```bash
pip install -r requirements.txt
```

3. **Configure device management IPs**
Edit `inventory/hosts.yml` and update management IP addresses for all devices:
```yaml
R1:
  ansible_host: 192.168.1.11  # Update to your actual management IP
```

4. **Configure authentication credentials** (Optional, using Ansible Vault)
```bash
ansible-vault create group_vars/vault.yml
```

Content:
```yaml
vault_ansible_password: "your_device_password"
vault_ansible_enable_password: "your_enable_password"
```

5. **Test connectivity**
```bash
ansible all -m cisco.ios.ios_command -a "commands='show version'" --one-line
```

## Usage Guide

### Method 1: Quick Start Script (Recommended)

```bash
./quickstart.sh
```

Interactive menu options:
1. Full deployment (all stages S0-S8)
2. Staged deployment (incremental execution)
3. Validation only (no configuration changes)
4. Connectivity check

### Method 2: Manual Playbook Execution

#### Full Deployment
```bash
ansible-playbook playbooks/deploy_all.yml
```

#### Staged Deployment

```bash
# S0: Preparation and Baseline
ansible-playbook playbooks/s0_preparation.yml

# S1: HQ L2/L3 and HSRP
ansible-playbook playbooks/s1_hq_vlan_hsrp.yml

# S2: DMZ and R3 Integration
ansible-playbook playbooks/s2_dmz_r3.yml

# S3: OSPF Summarization and Default Routes
ansible-playbook playbooks/s3_ospf_summary.yml

# S4: GRE Underlay and Tunnel Configuration
ansible-playbook playbooks/s4_gre_tunnel.yml

# S5: Branch Office LAN
ansible-playbook playbooks/s5_branch_lan.yml

# S6: eBGP to Upstream ISPs
ansible-playbook playbooks/s6_ebgp.yml

# S7: NAT and ACL (Optional)
ansible-playbook playbooks/s7_nat_acl.yml --tags scenario_a

# S8: Observability and Logging
ansible-playbook playbooks/s8_observability.yml
```

#### Validate Deployment
```bash
ansible-playbook playbooks/verify_deployment.yml
```

## Architecture Design

### Why Ansible?

- **Declarative Syntax**: Describe "desired state" rather than "steps to achieve it" for improved readability
- **Agentless Architecture**: Direct device management via SSH without agent installation
- **Idempotency Guarantees**: Repeated playbook executions produce no side effects
- **Rich Network Modules**: Native Cisco IOS support simplifies device management

### Playbook Organization Strategy

**Staged Deployment Model**:
```
S0: Foundation Setup → S1-S2: L2/L3 Basics → S3-S4: Routing Protocols
→ S5-S6: Branch & External Connectivity → S7-S8: Security & Monitoring
```

**Advantages**:
- **Reduced Complexity**: Each stage focuses on single functionality, easier to understand and debug
- **Independent Testing**: Execute individual stages for isolated verification
- **Progressive Learning**: Newcomers can follow stages sequentially to understand network construction
- **Failure Isolation**: Problems are quickly localized to specific stages

### Variable Management Architecture

```
group_vars/
├── all.yml           # Global variables (VLANs, IP ranges, ASNs)
└── hq_routers.yml    # Role-specific variables (HSRP, OSPF configs)

inventory/
└── hosts.yml         # Device inventory and grouping logic
```

**Design Principles**:
- **DRY Principle**: Avoid duplication, enhance maintainability
- **Variable Hierarchy**: Global → Group → Host level with clear precedence
- **Readability First**: Descriptive variable names reduce cognitive load

## Project Structure

```
ansible_network-lab/
├── README.md                    # Project documentation
├── README_EN.md                 # English documentation
├── ansible.cfg                  # Ansible configuration
├── requirements.txt             # Python dependencies
├── quickstart.sh               # Quick start script
├── .gitignore                   # Git ignore rules
│
├── inventory/
│   └── hosts.yml               # Device inventory
│
├── group_vars/
│   ├── all.yml                 # Global variables (network design)
│   └── hq_routers.yml          # HQ router variables
│
├── host_vars/                   # Host-specific variables (optional)
│
├── playbooks/
│   ├── deploy_all.yml          # Master playbook
│   ├── s0_preparation.yml      # S0: Preparation
│   ├── s1_hq_vlan_hsrp.yml     # S1: VLAN & HSRP
│   ├── s2_dmz_r3.yml           # S2: DMZ configuration
│   ├── s3_ospf_summary.yml     # S3: OSPF summarization
│   ├── s4_gre_tunnel.yml       # S4: GRE tunneling
│   ├── s5_branch_lan.yml       # S5: Branch LAN setup
│   ├── s6_ebgp.yml             # S6: BGP external routing
│   ├── s7_nat_acl.yml          # S7: NAT & ACLs
│   ├── s8_observability.yml    # S8: Logging and observability
│   └── verify_deployment.yml   # Deployment validation
│
└── roles/                       # Ansible Roles (future expansion)
```

## Validation Checklist

After deployment completion, verify the following items:

### A. HSRP and Inter-VLAN Routing
- [ ] R1 = Active, R2 = Standby
- [ ] VIP correctly set to .254 on each VLAN
- [ ] Cross-VLAN ping successful

### B. OSPF Configuration
- [ ] Area 0/50/10 neighbors all FULL
- [ ] R1/R2 receive only 10.110.0.0/16 aggregate route

### C. GRE Tunnels
- [ ] Tunnel0 interface up/up
- [ ] R3 ↔ BR1 bidirectional ping successful
- [ ] MD5 authentication enabled

### D. eBGP Connectivity
- [ ] R1/R2 BGP sessions with ISPs established
- [ ] Only aggregate routes advertised externally

### E. Default Route Distribution
- [ ] HQ has 0.0.0.0/0 in routing table
- [ ] Branch office can reach external networks

### F. High Availability Testing
- [ ] HSRP active/standby failover working
- [ ] GRE tunnel failover scenarios functional

## Advanced Features

### Using Tags

```bash
# Execute only HSRP configuration
ansible-playbook playbooks/s1_hq_vlan_hsrp.yml --tags hsrp

# Skip optional features
ansible-playbook playbooks/deploy_all.yml --skip-tags optional

# Deploy only NAT scenario A
ansible-playbook playbooks/s7_nat_acl.yml --tags scenario_a
```

### Dry-Run Mode

```bash
# Check changes without actual execution
ansible-playbook playbooks/deploy_all.yml --check
```

### Verbose Output

```bash
# Show detailed execution
ansible-playbook playbooks/deploy_all.yml -v
ansible-playbook playbooks/deploy_all.yml -vv   # More verbose
ansible-playbook playbooks/deploy_all.yml -vvv  # Maximum verbosity with debug
```

## Troubleshooting

### Connection Issues

```bash
# Test single device connectivity
ansible R1 -m ping

# Verify authentication
ansible R1 -m cisco.ios.ios_command -a "commands='show version'"
```

### Configuration Verification

```bash
# Validate deployment results
ansible-playbook playbooks/verify_deployment.yml

# Check Ansible logs
tail -f ansible.log
```

### Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| Connection timeout | Incorrect `ansible_host` IP | Verify management IP addresses |
| Authentication failed | Wrong credentials | Confirm passwords and enable secret |
| Module not found | Missing dependencies | Run `pip install -r requirements.txt` |
| YAML parse error | Syntax issues | Check YAML indentation (2 spaces) |

## Future Roadmap

### Technology Expansion

- **Ansible Roles Refactoring**: Convert playbooks to reusable, composable roles
- **CI/CD Integration**: Implement GitLab CI / GitHub Actions for automated testing
- **Centralized Logging**: Deploy Syslog server for centralized log management
- **Monitoring System**: Integrate Prometheus + Grafana for network device monitoring

### Automation Deepening

- **Network Test Automation**: Add pytest-testinfra for state-based validation
- **Configuration Backup**: Implement automatic device configuration backups to Git
- **Change Approval Workflow**: Establish PR-based configuration change process
- **Infrastructure as Code**: Combine Terraform for virtualization management

## Reference Documentation

- [Enterprise Network Lab Guide PDF](Enterprise%20Network%20Lab%20Guide.pdf)
- [Ansible Network Modules Documentation](https://docs.ansible.com/ansible/latest/network/index.html)
- [Cisco IOS Configuration Guides](https://www.cisco.com/c/en/us/support/ios-nx-os-software/ios-15-4m-t/products-installation-and-configuration-guides-list.html)
- [Cisco ENARSI Certification](https://learningnetwork.cisco.com/)

## Contributing

Contributions are welcome! Here's how you can help:

### Reporting Issues

- Check existing issues to avoid duplicates
- Include device type, Ansible version, and error logs
- Provide minimal reproducible examples

### Submitting Pull Requests

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes with clear commit messages
4. Test your changes against multiple Cisco IOS versions
5. Ensure YAML syntax is valid: `ansible-playbook --syntax-check playbooks/*.yml`
6. Submit a PR with detailed description of changes

### Development Setup

```bash
# Install development dependencies
pip install -r requirements.txt
pip install yamllint pytest

# Validate YAML syntax
yamllint playbooks/ group_vars/ inventory/

# Run tests
pytest tests/
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Author

**Riven-portfolio**

## Contact & Support

For questions, issues, or suggestions:
- Open an [Issue on GitHub](https://github.com/Riven-portfolio/ansible_network-lab/issues)
- Check existing documentation and troubleshooting guides first

---

## Important Notes

- **Lab Environment Only**: This project is designed for educational/lab use, not recommended for direct production deployment
- **Isolated Testing**: Always test in an isolated network environment before any production use
- **Configuration Backup**: Maintain regular backups of device configurations
- **IOL Limitations**: Some features (e.g., NAT) may be unstable on IOL; see PDF Appendix A for Linux-based alternatives
- **Cisco Device Support**: Tested on IOS 15.x and newer; compatibility with older versions may vary

---

**Last Updated**: 2026-02-02
**Status**: Active Development
**Community**: Contributions welcome!
