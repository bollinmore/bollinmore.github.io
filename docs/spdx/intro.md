# Introduction to SPDX 

## Term
* Releationship: can be used to indicate relationships between Packages, such as dependency relationships.
* Package information section: Packages are an abstract concept that can be used to refer to any distribution of software, typically consisting of one or more files and capable of containing sub-packages. 
    * Cardinality: Optional, one or many.
* File information section: It provides important meta information about a given file including licenses and copyright.
    * Cardinality: Optional, one or many.
    * Files are assumed to be associated with the Package Information that immediately precedes it, if a package exists.
    * If a File is not part of any package, it shall precede any Package Information section reference in the SPDX document.
    * The first field to start off the description of a File shall be the File Name in tag:value format.



### License Identifier

A document with a single license is defined by **SPDX-License-Identifier**, for example:  
```
SPDX-License-Identifier: CDDL-1.0+
SPDX-License-Identifier: MIT
```

Some commonly found licenses have been defined in the [SPDX License List](https://spdx.org/licenses/), it represents in a short form.  

However, we might want to declare a private license, so a user defined license would be represented by **LicenseRef-<idstring>**. (`idstring = 1*(ALPHA / DIGIT / "-" / "." )`). If a license of a file was claimed as *LicenseRef-2*, and its license was defined in other file, then the copy of actual text will be filled in the field -- **ExtractedText**.

```
LicenseID: LicenseRef-2
ExtractedText: <text>This package includes the GRDDL parser developed by Hewlett Packard under the following license:
© Copyright 2007 Hewlett-Packard Development Company, LP

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met: 

Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer. 
Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution. 
The name of the author may not be used to endorse or promote products derived from this software without specific prior written permission. 
THIS SOFTWARE IS PROVIDED BY THE AUTHOR ``AS IS'' AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE. </text>

```

