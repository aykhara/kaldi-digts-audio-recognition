# 'bad variable name' 
## Problem
Local OS: Windows 10 Enterprise 19042
Remote OS: WSL 2 Ubuntu 20.04

When I am running `run.sh`, I faced errors like below.
```
sh: 2: export: (x86)/Common: bad variable name
--> ERROR: data/lang/L.fst is not olabel sorted
sh: 2: export: (x86)/Common: bad variable name
--> ERROR: data/lang/L_disambig.fst is not olabel sorted
--> ERROR (see error messages above)
prepare_lang.sh: error validating output
```

## Investigation
Obviourly, the error is related to space of the Windows folder name `Program Files (x86)`. When I look into the place where the error is happening, `run.sh` calls `utils/prepare_lang.sh`. The error happened when `utils/prepare_lang.sh` calls `utils/validate_lang.pl`.

`utils/validate_lang.pl`
```
$res = system(". ./path.sh; $cmd");
```

`path.sh` calls $PATH, which includes the root cause of this problem.
```
export PATH=$PWD/utils/:$KALDI_ROOT/tools/openfst/bin:$PWD:$PATH
```

When I checked `$PATH` on WSL 3, it includes Windows paths, which caused the error of bad variable name due to the space of `Program Files (x86)`.
```
$ echo $PATH

/home/kokawata/.pyenv/shims:/home/kokawata/.pyenv/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/mnt/c/Program Files (x86)/Common Files/Oracle/Java/javapath:/mnt/c/Program Files (x86)/Microsoft SDKs/Azure/CLI2/wbin:/mnt/c/Program Files/Common Files/Microsoft Shared/Microsoft Online Services:/mnt/c/Program Files (x86)/Common Files/Microsoft Shared/Microsoft Online Services:
```

## Solution
`wsl.conf` can be the solution here so WSL 2 avoids including Windows paths. If you do not have `wsl.conf`, you can create the file. You can refer [WSL configuration](https://docs.microsoft.com/en-us/windows/wsl/wsl-config) from docs.microsoft.com. 

```
sudo touch /etc/wsl.conf
sudo sh -c "echo [interop] >> /etc/wsl.conf"
sudo sh -c "echo appendWindowsPath = false >> /etc/wsl.conf"
```

After restarting your machine, you will not see Windows paths on WSL 2, and you do not face bad variable name errors any more.
```
$ echo $PATH
/home/kokawata/.pyenv/shims:/home/kokawata/.pyenv/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:
```
