# 使用 ORT（ghcr.io/oss-review-toolkit/ort）搭配 FOSSology 的教學

> 本文件示範如何用 **OSS Review Toolkit (ORT)** 官方容器 + **FOSSology** 容器，完成依賴解析、授權掃描、策略評估與 SBOM 報表輸出。內容依據 ORT 官方文件與 FOSSology 官方 Wiki / REST API 撰寫。

---

## 目標與成果

- 用 ORT 分析專案依賴（`analyze`）
- 用 **FOSSology** 做授權掃描（`scan --scanner fossology`）
- 產生 **HTML / SPDX / CycloneDX** 報告（`report`）
- 針對 **Windows / Docker Desktop** 提供「路徑掛載」與 **host.docker.internal** 的設定要點

---

## 先備條件

- 已安裝 **Docker**（或 Podman）
- 原始碼（例：`/path/to/project`）
- Docker 可從容器內連到主機上的 FOSSology（Windows / macOS / Linux 皆可）

---

## 1. 啟動 FOSSology（授權掃描服務）

```bash
# 在主機端啟動 FOSSology
docker run -d --name fossology -p 8081:80 fossology/fossology

# 確認 UI： http://localhost:8081/repo
# 預設帳密（官方容器常見示範值）：fossy / fossy
```

---

## 2. 抓取 ORT 官方容器影像

```bash
docker pull ghcr.io/oss-review-toolkit/ort:latest
```

---

## 3. 建立專案目錄（Windows 路徑注意）

### Windows（PowerShell 推薦）
```powershell
# 例：專案放在 C:\ort-projects\myproj
cd C:\ort-projects\myproj
docker run --rm -v "${PWD}:/project" ghcr.io/oss-review-toolkit/ort:latest --help
```

---

## 4. ORT：Analyze（依賴解析）

```bash
docker run --rm -v "/path/to/project:/project" ghcr.io/oss-review-toolkit/ort:latest   analyze   -i /project   -o /project/ort-results
```

---

## 5. ORT：Scan（以 FOSSology 作為授權掃描 Backend）

```bash
docker run --rm -v "/path/to/project:/project"   -e ORT_SCANNER_FOSSOLGY_SERVER_URL="http://host.docker.internal:8081/repo/api/v1"   -e ORT_SCANNER_FOSSOLGY_USERNAME="fossy"   -e ORT_SCANNER_FOSSOLGY_PASSWORD="fossy"   ghcr.io/oss-review-toolkit/ort:latest   scan   -i /project/ort-results/analyzer-result.yml   -o /project/ort-results   --scanner fossology
```

---

## 6. ORT：Evaluate（依策略規則評分）

```bash
docker run --rm -v "/path/to/project:/project" ghcr.io/oss-review-toolkit/ort:latest   evaluate   -i /project/ort-results/scan-result.yml   -o /project/ort-results
```

---

## 7. ORT：Report（輸出 HTML / SPDX / CycloneDX）

```bash
docker run --rm -v "/path/to/project:/project" ghcr.io/oss-review-toolkit/ort:latest   report   -i /project/ort-results/evaluator-result.yml   -o /project/ort-results/report   -f StaticHtml,SpdxDocument,CycloneDxJson
```

---

## 8. ORT 設定檔（允許動態版本等）

```yaml
analyzer:
  allowDynamicVersions: true
scanner:
  skipExcluded: true
```

---

## 9. 疑難排解（FAQ）

### A. 容器內連不到主機的 FOSSology
請用 `http://host.docker.internal:8081`。

### B. `analyze` 只有一份檔案
正常。這是 ORT 的預期輸出。

### C. `.vcxproj` / NuGet 分析錯誤
請在 Windows（含 VS Build Tools）執行 `analyze`。

---
