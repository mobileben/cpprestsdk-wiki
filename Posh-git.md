## Posh-Git

Posh-Git offers the best command line experience with Git on Windows. [Here](http://haacked.com/archive/2011/12/13/better-git-with-powershell.aspx) is a great blog post that introduces it.  

Setting it up is very easy. In PowerShell window, type this:  

```powershell
(new-object Net.WebClient).DownloadString("http://psget.net/GetPsGet.ps1") | iex
install-module posh-git
```