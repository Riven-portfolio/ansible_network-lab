# Ansible Network Lab - 企業網路自動化部署

[![Ansible](https://img.shields.io/badge/Ansible-2.10%2B-red.svg)](https://www.ansible.com/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

> 💡 **專案性質**：個人學習專案
> 從 ENARSI 課程的手動配置練習，到用 Ansible 實現全自動化部署的學習之旅。

## 💡 專案起源

### 學習背景
在準備 **Cisco ENARSI**（Implementing Cisco Enterprise Advanced Routing and IP Services）認證時，我完成了一個包含 HSRP、OSPF、BGP、GRE 隧道的企業網路實驗（詳見專案中的 [Lab 指南 PDF](企業網路模擬%20Lab%20指南.pdf)）。

### 遇到的問題
手動配置這個 10 台設備的實驗環境時，我發現：
- ⏰ **耗時**：完整配置需要 2-3 小時
- 🐛 **容易出錯**：一個 typo 就要花時間 debug
- 🔄 **難以重現**：想重新部署一次非常麻煩
- 📝 **配置分散**：10 台設備的配置散落在各處，難以管理

### 解決方案：Network as Code
於是我開始學習 **Ansible**，嘗試將這個手動實驗轉化為自動化部署：
- ✅ **15 分鐘完成部署**：從 2-3 小時縮短到 15 分鐘
- ✅ **零配置錯誤**：冪等性保證配置一致
- ✅ **可重現性**：一鍵重建整個實驗環境
- ✅ **版本控制**：用 Git 管理所有配置變更

### 學習目標
這個專案的目的是：
1. 📚 **深化網路知識**：透過自動化理解 OSPF、BGP、HSRP 的運作邏輯
2. 🤖 **學習 IaC 實踐**：掌握基礎設施即代碼的思維和工具
3. 🔧 **培養自動化思維**：從「手動操作」轉變為「程式化管理」
4. 🎯 **為職涯做準備**：為未來的網路自動化 / SRE 職位奠定基礎

## 🎯 核心特色

### 🤖 自動化能力展示
- **基礎設施即代碼 (IaC)**：所有網路配置以 YAML 宣告式管理
- **模組化架構**：8 個獨立 Playbooks (S0-S8)，可組合部署
- **冪等性保證**：重複執行不產生錯誤，確保配置收斂
- **GitOps 實踐**：透過 Git 實現配置版本控制與變更審計
- **自動驗證**：內建部署後驗證流程，確保服務正常

### 📡 企業級網路功能
- **HSRP 高可用**：雙路由器冗餘（R1/R2），自動故障轉移
- **OSPF 多區域**：Area 0/10/50 設計，含 Stub + MD5 認證
- **BGP 外連**：雙 ISP 冗餘連接 (AS 65001/65002)
- **GRE VPN 隧道**：總部與分公司安全連接
- **VLAN 隔離**：多網段管理（User/IT/DMZ/Log）
- **安全防護**：DMZ ACL、NAT 配置

### 🔧 技能整合
這個專案展現了：
- 📡 **網路知識**：OSPF、BGP、HSRP、GRE、VLAN 設計與實作
- 🤖 **自動化能力**：Ansible Playbook 設計、變數管理、錯誤處理
- 🏗️ **系統架構**：模組化設計、階段式部署、可維護性優化
- 📝 **DevOps 思維**：GitOps、聲明式配置、可重現部署

## 🏢 網路拓撲架構

本專案基於 **企業網路模擬 Lab 指南**，部署包含 10 台設備的完整企業網路環境：

### 設備配置
- **總部 (HQ)**：R1, R2 (HSRP 高可用), R3 (DMZ/ABR), SW1
- **分公司 (Branch)**：BR1, BR-SW, Host-BR
- **ISP**：ISP1 (AS65001), ISP2 (AS65002)
- **伺服器**：WebSrv (DMZ)
  - **註**: LogSrv 當前未部署，VLAN 60 保留供未來使用

### 網路規劃
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

## 📊 專案成果

### 自動化效益
| 項目 | 手動配置 | 自動化部署 | 改善幅度 |
|------|---------|-----------|---------|
| **部署時間** | 2-3 小時 | 15 分鐘 | 📈 88% ↓ |
| **配置錯誤率** | 人為失誤 | 零錯誤 | ✅ 100% |
| **配置一致性** | 難以保證 | 完全一致 | ✅ 100% |
| **變更追溯** | 無記錄 | Git 完整記錄 | ✅ 可追溯 |

### 技術能力展示
✅ **網路自動化**：Ansible 管理 Cisco 設備，跨設備配置協調
✅ **IaC 實踐**：聲明式配置，可重現部署
✅ **模組化設計**：8 階段獨立 Playbooks，降低複雜度
✅ **GitOps 工作流**：版本控制 + 變更審計
✅ **驗證機制**：自動化測試確保配置正確性

### 實驗室驗證
- ✅ EVE-NG 模擬環境完整驗證
- ✅ 高可用性測試（HSRP 故障轉移）
- ✅ 路由協定收斂驗證（OSPF、BGP）
- ✅ VPN 連通性測試（GRE Tunnel）

## 🚀 快速開始

### 前置要求

- **Python** 3.8+
- **Ansible** 2.10+
- **網路設備** Cisco IOS (IOL/VIRL/EVE-NG)
- **RAM**: 約 2.5-3 GB (L3: 256-384MB, L2: 192MB)

### 安裝步驟

1. **克隆倉庫**
```bash
git clone https://github.com/Riven-portfolio/ansible_network-lab.git
cd ansible_network-lab
```

2. **安裝依賴**
```bash
pip install -r requirements.txt
```

3. **配置設備 IP**
編輯 `inventory/hosts.yml`，更新各設備的管理 IP：
```yaml
R1:
  ansible_host: 192.168.1.11  # 修改為實際 IP
```

4. **配置認證資訊** (可選，使用 Ansible Vault)
```bash
ansible-vault create group_vars/vault.yml
```
內容：
```yaml
vault_ansible_password: "your_device_password"
```

5. **測試連接**
```bash
ansible all -m cisco.ios.ios_command -a "commands='show version'" --one-line
```

## 📖 使用方法

### 方法 1: 使用快速開始腳本 (推薦)
```bash
./quickstart.sh
```
選單選項：
1. 完整部署 (S0-S8 全部階段)
2. 分階段部署 (逐步執行)
3. 僅驗證 (不部署)
4. 檢查連接

### 方法 2: 手動執行 Playbooks

#### 完整部署
```bash
ansible-playbook playbooks/deploy_all.yml
```

#### 分階段部署
```bash
# S0: 準備與基線
ansible-playbook playbooks/s0_preparation.yml

# S1: HQ L2/L3 與 HSRP
ansible-playbook playbooks/s1_hq_vlan_hsrp.yml

# S2: DMZ 與 R3 接入
ansible-playbook playbooks/s2_dmz_r3.yml

# S3: OSPF 聚合預設
ansible-playbook playbooks/s3_ospf_summary.yml

# S4: GRE Underlay 與 Tunnel
ansible-playbook playbooks/s4_gre_tunnel.yml

# S5: 分公司 LAN
ansible-playbook playbooks/s5_branch_lan.yml

# S6: eBGP 對上游
ansible-playbook playbooks/s6_ebgp.yml

# S7: NAT 與 ACL (選配)
ansible-playbook playbooks/s7_nat_acl.yml --tags scenario_a

# S8: 可觀測性與日誌
ansible-playbook playbooks/s8_observability.yml
```

#### 驗證部署
```bash
ansible-playbook playbooks/verify_deployment.yml
```

## 🏗️ 架構設計

### 為什麼選擇 Ansible？
- **聲明式語法**：描述「想要的狀態」而非「如何操作」，提升可讀性
- **無代理架構**：透過 SSH 直接管理設備，無需在目標設備安裝額外軟體
- **冪等性保證**：重複執行相同 Playbook 不會產生副作用
- **豐富的網路模組**：原生支援 Cisco IOS，簡化網路設備管理

### Playbook 組織策略

**階段式部署設計**：
```
S0: 基礎準備 → S1-S2: L2/L3 基礎 → S3-S4: 路由協定
→ S5-S6: 分支與外連 → S7-S8: 安全與監控
```

**優勢**：
- 🔹 **降低複雜度**：每個階段專注單一功能，易於理解和除錯
- 🔹 **獨立測試**：可單獨執行任一階段進行驗證
- 🔹 **漸進式部署**：新人可按順序學習，理解網路建構過程
- 🔹 **故障隔離**：問題發生時容易定位是哪個階段出錯

### 變數管理架構

```
group_vars/
├── all.yml           # 全局變數（VLAN、IP 範圍、AS 號碼）
└── hq_routers.yml    # 角色特定變數（HSRP、OSPF 配置）

inventory/
└── hosts.yml         # 設備清單與分組邏輯
```

**設計原則**：
- 📌 **DRY 原則**：避免重複定義，提升可維護性
- 📌 **層級變數**：全局 → 群組 → 主機，清晰的優先級
- 📌 **可讀性優先**：使用描述性變數名稱，降低理解成本

## 📂 項目結構

```
ansible_network-lab/
├── README.md                    # 專案說明
├── ansible.cfg                  # Ansible 配置
├── requirements.txt             # Python 依賴
├── quickstart.sh               # 快速開始腳本
├── .gitignore                   # Git 忽略文件
│
├── inventory/
│   └── hosts.yml               # 設備清單
│
├── group_vars/
│   ├── all.yml                 # 全局變量 (網路規劃)
│   └── hq_routers.yml          # HQ 路由器變量
│
├── host_vars/                   # 主機特定變量 (可選)
│
├── playbooks/
│   ├── deploy_all.yml          # 主 Playbook
│   ├── s0_preparation.yml      # S0: 準備
│   ├── s1_hq_vlan_hsrp.yml     # S1: VLAN & HSRP
│   ├── s2_dmz_r3.yml           # S2: DMZ
│   ├── s3_ospf_summary.yml     # S3: OSPF 聚合
│   ├── s4_gre_tunnel.yml       # S4: GRE
│   ├── s5_branch_lan.yml       # S5: 分公司
│   ├── s6_ebgp.yml             # S6: BGP
│   ├── s7_nat_acl.yml          # S7: NAT/ACL
│   ├── s8_observability.yml    # S8: 日誌
│   └── verify_deployment.yml   # 驗證腳本
│
└── roles/                       # Ansible Roles (未來擴展)
```

## ✅ 驗證清單

部署完成後，請驗證以下項目：

### A. HSRP 與 VLAN 間路由
- [ ] R1 = Active, R2 = Standby
- [ ] VIP 分別為 .254
- [ ] 跨 VLAN ping 通

### B. OSPF
- [ ] Area 0/50/10 鄰居均為 FULL
- [ ] R1/R2 只見 10.110.0.0/16 聚合路由

### C. GRE 隧道
- [ ] Tunnel0 up/up
- [ ] R3 ↔ BR1 互 ping 通
- [ ] MD5 認證啟用

### D. eBGP
- [ ] R1/R2 與 ISP 為 Established
- [ ] 對外只公告聚合路由

### E. 預設路由
- [ ] HQ 內部有 0.0.0.0/0
- [ ] 分公司能 ping 外部

### F. 高可用測試
- [ ] HSRP 切換測試
- [ ] GRE 隧道故障測試

## 🎯 進階功能

### 標籤 (Tags) 使用
```bash
# 僅執行 HSRP 配置
ansible-playbook playbooks/s1_hq_vlan_hsrp.yml --tags hsrp

# 跳過可選功能
ansible-playbook playbooks/deploy_all.yml --skip-tags optional

# 僅執行 NAT 情境 A
ansible-playbook playbooks/s7_nat_acl.yml --tags scenario_a
```

### Dry Run 模式
```bash
# 檢查變更但不實際執行
ansible-playbook playbooks/deploy_all.yml --check
```

### 詳細輸出
```bash
# 顯示詳細執行過程
ansible-playbook playbooks/deploy_all.yml -v
ansible-playbook playbooks/deploy_all.yml -vv   # 更詳細
ansible-playbook playbooks/deploy_all.yml -vvv  # 最詳細 (含 debug)
```

## 🔧 故障排除

### 連接問題
```bash
# 測試單一設備連接
ansible R1 -m ping

# 檢查認證
ansible R1 -m cisco.ios.ios_command -a "commands='show version'"
```

### 配置問題
```bash
# 檢查設備配置
ansible-playbook playbooks/verify_deployment.yml

# 查看 Ansible 日誌
tail -f ansible.log
```

### 常見錯誤
1. **連接超時**: 檢查 `ansible_host` IP 是否正確
2. **認證失敗**: 確認密碼和 enable 密碼
3. **模組未找到**: 執行 `pip install -r requirements.txt`

## 🔮 未來改進方向

### 技術擴展
- 🔹 **Ansible Roles 重構**：將 Playbooks 轉換為可重用的 Roles
- 🔹 **CI/CD 整合**：導入 GitLab CI / GitHub Actions 自動化測試
- 🔹 **集中式日誌**：部署 Syslog Server 實現日誌集中管理
- 🔹 **監控系統**：整合 Prometheus + Grafana 監控網路設備

### 自動化深化
- 🔹 **網路測試自動化**：加入 pytest-testinfra 進行狀態驗證
- 🔹 **配置備份機制**：定期備份設備配置到 Git
- 🔹 **變更審批流程**：實現 PR-based 配置變更工作流
- 🔹 **Terraform 整合**：結合 Terraform 管理虛擬化基礎設施

## 📚 參考文檔

- [企業網路模擬 Lab 指南 PDF](企業網路模擬%20Lab%20指南（重排版）.pdf)
- [Ansible 網路模組文檔](https://docs.ansible.com/ansible/latest/network/index.html)
- [Cisco IOS 配置指南](https://www.cisco.com/c/en/us/support/ios-nx-os-software/ios-15-4m-t/products-installation-and-configuration-guides-list.html)

## 🤝 貢獻

歡迎提交 Issues 和 Pull Requests！

## 📄 許可證

MIT License

## 👤 作者

Riven-portfolio

## 📞 聯繫方式

如有問題，請通過 GitHub Issues 聯繫。

---

**注意事項：**
- 本項目適用於實驗室環境，不建議直接用於生產環境
- 請確保在隔離的網路環境中測試
- 定期備份設備配置
- IOL 某些功能（如 NAT）可能不穩定，請參考 PDF 附錄 A 使用 Linux 替代方案
