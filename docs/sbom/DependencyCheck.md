# Dependency-Check

Dependency-Check 是一個軟體組成分析 (SCA) 工具，可識別專案依賴關係並檢查是否存在任何已知的、公開揭露的漏洞。本文件提供如何安裝和使用它的指南，以及如何處理誤報。

## 安裝

### 下載

您可以從 [GitHub releases page](https://github.com/jeremylong/DependencyCheck/releases) 下載 Dependency-Check。

*   `dependency-check-*-release.zip`: 命令列介面 (CLI) 版本。
*   `dependency-check-ant-*-release.zip`: 用於 Apache Ant。

### Java 版本

Dependency-Check v11.0.0 及更高版本需要 Java 11 或更高版本。如果您使用的是較舊版本的 Java，您將看到如下錯誤：

```text
Exception in thread "main" java.lang.UnsupportedClassVersionError
...
this version of the Java Runtime only recognizes class file versions up to 52.0
```

要解決此問題，請安裝 OpenJDK 11 並相應地設定 `JAVA_HOME` 和 `PATH` 環境變數。您可以使用 `java -version` 驗證 Java 版本。

## 基本用法

Java 設定正確後，您可以從命令列執行 Dependency-Check：

```bash
.\bin\dependency-check.bat --scan <專案資料夾> --out <報告目錄>
```

您可以使用 `-h` 旗標查看所有可用選項。要產生 HTML 報告，請使用 `--format HTML` 旗標。

如果您有 NVD API 金鑰，您可以使用它來加速資料檢索：

```bash
--nvdApiKey <您的-api-金鑰>
```

或者，您可以設定 `NVD_API_KEY` 環境變數。

## 誤報與抑制

Dependency-Check 允許您使用抑制 XML 檔案來抑制誤報。

### 抑制 XML 格式

抑制檔案必須具有正確的命名空間。您可以透過點擊 HTML 報告中的「Complete XML Doc」連結來產生完整的 XML 文件。

每個 `<suppress>` 元素可以針對 CVE、CPE、artifact、CPE pattern 或 location。

### 抑制 CVE

```xml
<?xml version="1.0" encoding="UTF-8"?>
<suppressions xmlns="https://jeremylong.github.io/DependencyCheck/dependency-suppression.1.3.xsd">
  <suppress>
    <cve>CVE-2023-12345</cve>
    <notes>此 CVE 不影響我們的專案。</notes>
  </suppress>
</suppressions>
```

### 抑制 CPE

```xml
<suppressions xmlns="https://jeremylong.github.io/DependencyCheck/dependency-suppression.1.3.xsd">
  <suppress>
    <cpe>cpe:/a:vendor:product:version</cpe>
    <notes>此 CPE 是誤報，可以安全地忽略。</notes>
  </suppress>
</suppressions>
```

您也可以使用 `<cpePattern>` 來抑制整個供應商：

```xml
<suppress>
  <cpePattern>cpe:/a:vendor:.*</cpePattern>
</suppress>
```

### 使用抑制檔案

使用 `--suppression` 旗標指定抑制檔案：

```bash
--suppression dependency-check-suppression.xml
```

## 疑難排解

如果抑制不起作用，通常是因為 XML 檔案中缺少命名空間或根元素不正確。

確保您的 `suppression.xml` 檔案具有正確的標頭和 `<suppressions>` 根元素。您可以使用 HTML 報告來產生完整的範例。另外，請檢查抑制檔案的路徑，特別是對於有空格或編碼問題的情況。