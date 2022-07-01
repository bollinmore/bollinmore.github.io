# 更新CefPython Chromium 版本筆記
>更新日期：  2022/4/28  
>狀態：嘗試中

近日傳出Chrome瀏覽器出現漏洞([CVE-2022-1364](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2022-1364))，允許攻擊者遠端執行任意程式碼，官方的建議是更新至100.0.4896.127以後的版本。不過基於CefPython開發的專案就沒有那麼幸運了，因為目前最新CefPython的版本仍停留在66，看來只能手動更新CEF版本了。

雖然作者有提供更新CEF版本的[Workflow](https://github.com/cztomczak/cefpython/issues/264), 但是可能版本差異過大，過程中遇到很多錯誤需要自行處理。

## 快速瀏覽更新步驟
* 下載 [CefPython 原始碼](https://github.com/cztomczak/cefpython.git)
* 下載 [Prebuilt](https://cef-builds.spotifycdn.com/index.html#windows64) 不然從CEF原始碼編譯會太久
* 解開下載的prebuilt壓縮檔，並且放在**路徑較短的目錄**，原因在於編譯過程中可能會出現路徑過程的錯誤: VS 2022 Cannot open compiler generated file: '': Invalid argument
* `mkdir build && cd build` 建立build資料夾並移至該資料夾
* `pip install -r tools\requirements.txt`
* 修改 *src\cef_version_win.h* 此步驟要設定正確版號才能正確往下執行
* 修改 *tools\common.py* 主要原因是我使用Python 3.10
* 修改編譯設定*tools\common.py* & *tools\automate.py*，增加VS2022路徑
* 為了避免 code page (950) 錯誤，開啟 *C:\CefBin\cef_binary_101.0.15+gca159c5+chromium-101.0.4951.54_windows64\tests\ceftests\os_rendering_unittest.cc* 並加上下列兩行，此檔案必須另存為UTF-8 with BOM
    * #pragma warning(disable : 4819)
    * #pragma warning(disable : 4566)
* `python ..\tools\automate.py --prebuilt-cef --build-dir C:\CefBin\`
* 上述指令完成會在*C:\CefBin*下產生一個新的資料夾，複製到*CefPython\build*
* 執行 `python ..\tool\build.py xx.x`

## CefPython 編譯運作機制(Prebuilt)
* 執行 build.py 後，其內部會呼叫 cython_setup.py 去產生需要的標頭檔，第二次呼叫則腳本會檢查 *build\build_cefpython* 目錄下是否還有未處理的*.pyx檔案，沒有則代表已經產生 cefpython_py39_fixed.h
* cython_setup.py 會分析原始碼產生 handler 相關的原始碼與標頭。

## Trobleshooting
### 找不到Python.h
網路上可以找到Linux的解決方法，就是透過`sudo apt install python-dev`，但是我一開始是用Microsoft Store的方式安裝Python, 就沒有看到微軟有把相關的標頭檔放進去，所以最簡單的做法就是去官方網站下載Python安裝，並且修改以下Include Path，若你的安裝位置在`C:\Python\Python39\include`，則：  
```python
def get_python_include_path():
    base_dir = os.path.dirname(sys.executable)
    try_dirs = ["{base_dir}/include",
                "{base_dir}/../include/python{ver}",
                "{base_dir}/../include/python{ver}*",
                ("{base_dir}/../Frameworks/Python.framework/Versions/{ver}"
                 "/include/python{ver}*"),
                "/usr/include/python{ver}",
                "C:\\Python\\Python39\\include"
                ]
```

### 修改編譯設定
以我使用的VS2022為例，由於目前CefPython只有支援VS2015以前的，所以必須手動修改*tool\common.py*, 於約Line 223行插入：  
```python
VS2022_VCVARS = ("C:\\Program Files\\Microsoft Visual Studio\\2022\\Preview\\VC\\Auxiliary\\Build\\vcvarsall.bat")
```

但這個做的前提必須是已經裝過VC++了，沒有的話可以用**Visual Studio Installer**安裝**Desktop development with C++**.  

如果是手動編譯CEF的話，則需要一併修改*automate.py*, 在`prepare_build_command()`中調整branch號碼，因為此修正在4896之後，因此修改如下：  
```python
def prepare_build_command(build_lib=False, vcvars=None):
    """On Windows VS env variables must be set up by calling vcvarsall.bat"""
    command = list()
    if platform.system() == "Windows":
        if build_lib:
            ...
        else:
            if int(Options.cef_branch) >= 4896:
                command.append(VS2022_VCVARS)
            # if int(Options.cef_branch) >= 2704:
            #     command.append(VS2015_VCVARS)
            else:
                command.append(VS2013_VCVARS)
            command.append(VS_PLATFORM_ARG)
        command.append("&&")
    return command
```

### 修改版本號碼
下載下來的prebuilt檔案命名如*cef_binary_100.0.24+g0783cf8+chromium-100.0.4896.127_windows64*，則可以根據版號規則修改*cef_version_win.h*中的Marco定義如下：  
```c
#define CEF_VERSION "100.0.24+g0783cf8+chromium-100.0.4896.127"
#define CHROME_VERSION_MAJOR 100
#define CHROME_VERSION_MINOR 0
#define CHROME_VERSION_BUILD 4896
```

至於為什麼要修改上述定義，因為在編譯過程Python腳本會根據版號定義去尋找prebuilt目錄，不改會找不到檔案。


### 其他編譯失敗問題
執行 build.py xx.x 會遇到幾個C++的編譯錯誤，整理在下方：
#### 找不到 cefpython_py39_fixed.h:  
以我使用的Python版本為例，卻仍因為PY_MINOR_VERSION找不到定義而編譯失敗。最簡單的方式就是直接把`#include "../../build/build_cefpython/cefpython_py39_fixed.h"`放在最外層，保證一定可以include到。  
#### `CefRequestCallback` not found:  
從CEF的commit log可以看到已經被`CefCallback`取代了  
#### `OVERRIDE`, `scoped_ptr`, `base::Closure` unknown identifier:
這類問題都可以用CEF的codebase比較，拉新的程式碼過來，不過要用比的，不能全蓋掉。  
#### 新增include到 cefpython_app.cpp:
* `#include "client_handler/request_context_handler.h"`
* `#include "include/cef_callback.h"`
