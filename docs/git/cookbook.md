## Frequently Used Command


### Submodule
* 如果有多個submodule，不想因為其中一個submodule執行後得到非零回傳值而中止執行，官網的教學是讓加上 ` || : ` 在指令後面。  
`git submodule foreach ' run_some_cmd || :'`  


### Diff
* Exclude files from "git diff"  
`git diff -- . ':(exclude)**/AssemblyInfo.cs' ':(exclude)**/*.dll' ':(exclude)**/*.exe'`  
    * It supports regular expression to exclude files => `':(exclude)file.*[cs|resx]`
* Filter files from "git status"  
    * grep -i *swid* => search keyword, case-insensitive.
    * cut -d ':' -f2 => use ':' as delimiter and use field #2
    * tail -n+2 => start passing through on the second line of the output
    * xargs => run `git diff` per line  
`git status | grep -i swid | cut -d ':' -f2 | tail -n+2 | xargs git diff`
* Show unmerged path only
`git diff --name-status --diff-filter=U`
    * There're several options could be used: --diff-filter=[(A|C|D|M|R|T|U|X|B)…​[*]]
    * Remove the preceding **U** => `sed -e 's/U\s//g'

### Cherry-pick
* How could I filter commits that contains specific path?  
`git log branch_A..branch_B -- <path> ':(exclude)file.*[cs|resx]' | cut -d ' ' -f2 | tac | sed -e 's/\x1b\[[0-9;]*m//g' | xargs git cp`  
    * tac => Reverse the order of previous out in order to do cherry-pick in the ascending order.
    * sed -e 's/\x1b\[[0-9;]*m//g' => Remove the ANSI color codes from text stream.

### Sparse Checkout
With Git-Sparse-Checkout feature, it improves the performance of cloning repository.  
The key point is to describe path in the **.git/info/sparse-checkout** and set `config.sparseCheckout true`.
```
git init
git remote add origin <remote>
git config core.sparseCheckout true
echo "relative path to the file" >> .git/info/sparse-checkout
git fetch --depth=1 origin <revision>
git checkout <revision>
```

### FAQ
#### Certificate issue
* When you encounter the problem: *SSL certificate problem: unable to get local issuer certificate*, you'll have 3 different methods below to workaround it:  
    - Reinstall Git and select `native Windows secure channel library` as transport backend.
    - `git config --global http.sslBackend schannel`
    - `git config --global http.sslVerify false`

* Remove files or folder from commit history.  
`git filter-branch --index-filter 'git rm -rf --cached --ignore-unmatch path_to_file' HEAD`  
