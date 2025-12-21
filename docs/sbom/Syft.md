# Syft

Syft 是一個用來掃描 Docker container / Binary 的套件，可以列出偵測到的軟體組件清單，
產生出來的資料以及欄位意義如下表格：

| 欄位             | 含義                                    |
| -------------- | ------------------------------------- |
| `name`         | 套件名稱                                  |
| `version`      | 套件版本                                  |
| `type`         | 套件類型（npm, maven, binary, os, etc.）    |
| `foundBy`      | 掃描器/策略名稱（哪個 cataloger 辨識到的）           |
| `locations`    | 這個 package 出現在 filesystem/image 的哪些路徑 |
| `language`     | 對 source package 時代表語言（npm, python）   |
| `purl`         | Package URL（符合 purl 規範，可做索引）          |
| `cpe` / `cpes` | 轉換後的 CPE，用於 CVE 索引                    |
| `licenses`     | 自動擷取/推測出的授權                           |
| `checksums`    | 包含 sha256 / md5 用於驗證                  |
| `metadataType` | `metadata` 的類型標記                      |
| `metadata`     | 與此 package 相關的原始欄位細節                  |

