## Frequently Used Command


### Submodule
* 如果有多個submodule，不想因為其中一個submodule執行後得到非零回傳值而中止執行，官網的教學是讓加上 ` || : ` 在指令後面。  
`git submodule foreach ' run_some_cmd || :'`  


### Diff
* Exclude files from "git diff"  
`git diff -- . ':(exclude)**/AssemblyInfo.cs' ':(exclude)**/*.dll' ':(exclude)**/*.exe'`

### FAQ
#### Certificate issue
* When you encounter the problem: *SSL certificate problem: unable to get local issuer certificate*, you'll have 3 different methods below to workaround it:  
    - Reinstall Git and select `native Windows secure channel library` as transport backend.
    - `git config --global http.sslBackend schannel`
    - `git config --global http.sslVerify false`

