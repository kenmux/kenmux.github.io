---
title: "Android原始碼下載"
date: 2016-07-17 18:00:00 +0800
categories: [Programming]
tags: [Android,AOSP,kernel,repo]
imgurl: /assets/imgs/2016/07/17/android-source-download
comments: true
---

Android原始碼包含兩個部分：Android開源專案（Android Open Source Project，AOSP）原始碼與Android內核原始碼。  

下載前、首先確認以下事項：  

- git  
- curl  
- python  
- POSIX相容系統  

前三項都是下載所需的組件，不必多說；說說最後一項：一開始打算在Windows下使用MinGW32進行下載的，但是遇到這樣的錯誤：  

> ImportError: No module named fcntl  

於是、果斷投奔Mac OS X的懷抱。但也有人改換Cygwin，感興趣的可以一試。  

鑑於龐大的程式碼規模、請務必使用高速且穩定的網路：Android開源專案原始碼藉由專用工具repo、可以實現斷點續傳；而Android內核原始碼、一旦中斷就得重新開始。  

Android開源專案原始碼的下載教程請參考[這裡](https://source.android.com/source/downloading.html)，Android內核原始碼的下載教程請參考[這裡](https://source.android.com/source/building-kernels.html#downloading-sources)。以下簡單的做下筆記：  

- 其一、[Android開源專案原始碼下載]({{ page.url }}#aosp-source-download)  
- 其二、[Android內核原始碼下載]({{ page.url }}#android-kernel-source-download)  

<!-- more -->  

# <a name="aosp-source-download"></a>Android開源專案原始碼下載  

下載安裝repo：  

{% highlight text %}
nawix-osx:~ kenmux$ mkdir bin
nawix-osx:~ kenmux$ cd bin
nawix-osx:bin kenmux$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 26223  100 26223    0     0  17546      0  0:00:01  0:00:01 --:--:-- 17552
nawix-osx:bin kenmux$ chmod a+x ~/bin/repo
nawix-osx:bin kenmux$ 
{% endhighlight %}

**注：由於沒有將`~/bin`加入`PATH`，故以下採用完整路徑來呼叫repo。**  

初啟化repo客戶端：  

{% highlight text %}
nawix-osx:bin kenmux$ mkdir android
nawix-osx:bin kenmux$ cd android/
nawix-osx:android kenmux$ ~/bin/repo init -u https://android.googlesource.com/platform/manifest
warning: gpg (GnuPG) is not available.
warning: Installing it is strongly encouraged.

......

Your identity is: kenmux <kenmux@gmail.com>
If you want to change this, please re-run 'repo init' with --config-name

Testing colorized output (for 'repo diff', 'repo status'):
  black    red      green    yellow   blue     magenta   cyan     white 
  bold     dim      ul       reverse 
Enable color display in this user account (y/N)? y

repo has been initialized in /Users/kenmux/bin/android
nawix-osx:android kenmux$ 
{% endhighlight %}

下載Android原始碼樹：  

{% highlight text %}
nawix-osx:android kenmux$ ~/bin/repo sync
Fetching project device/google/atv
Fetching project device/htc/flounder
Fetching project platform/prebuilts/devtools
Fetching project platform/hardware/marvell/bt

......

Checking out files: 100% (507/507), done.ng out files:  27% (140/507)   
Checking out files: 100% (15778/15778), done.ut files:   3% (627/15778)   
Checking out files: 100% (19/19), done.
Syncing work tree: 100% (515/515), done.  

nawix-osx:android kenmux$ 
{% endhighlight %}

程式碼規模：  

<div style="text-align:center" markdown="1">
![2016-07-17-01.png]({{ page.imgurl }}/2016-07-17-01.png)  
</div>

檢視內容：  

{% highlight text %}
nawix-osx:android kenmux$ ls -al
total 48
drwxr-xr-x   32 kenmux  staff  1088  7 14 14:08 .
drwxr-xr-x    6 kenmux  staff   204  7 13 18:52 ..
-rw-r--r--@   1 kenmux  staff  8196  7 14 14:08 .DS_Store
drwxr-xr-x   10 kenmux  staff   340  7 14 13:39 .repo
lrwxr-xr-x    1 kenmux  staff    19  7 14 13:39 Android.bp -> build/soong/root.bp
-r--r--r--    1 kenmux  staff    87  7 14 13:39 Makefile
drwxr-xr-x   26 kenmux  staff   884  7 14 13:39 art
drwxr-xr-x   19 kenmux  staff   646  7 14 13:39 bionic
drwxr-xr-x    3 kenmux  staff   102  7 14 13:39 bootable
lrwxr-xr-x    1 kenmux  staff    26  7 14 13:39 bootstrap.bash -> build/soong/bootstrap.bash
drwxr-xr-x   14 kenmux  staff   476  7 14 13:39 build
drwxr-xr-x   18 kenmux  staff   612  7 14 13:40 cts
drwxr-xr-x   14 kenmux  staff   476  7 14 13:40 dalvik
drwxr-xr-x    5 kenmux  staff   170  7 14 13:40 developers
drwxr-xr-x   22 kenmux  staff   748  7 14 13:41 development
drwxr-xr-x   12 kenmux  staff   408  7 14 13:41 device
drwxr-xr-x    3 kenmux  staff   102  7 14 13:42 docs
drwxr-xr-x  245 kenmux  staff  8330  7 14 13:48 external
drwxr-xr-x   17 kenmux  staff   578  7 14 13:49 frameworks
drwxr-xr-x   13 kenmux  staff   442  7 14 13:50 hardware
drwxr-xr-x    3 kenmux  staff   102  7 14 13:50 kernel
drwxr-xr-x   31 kenmux  staff  1054  7 14 13:50 libcore
drwxr-xr-x   15 kenmux  staff   510  7 14 13:50 libnativehelper
drwxr-xr-x   23 kenmux  staff   782  7 14 13:50 ndk
drwxr-xr-x    9 kenmux  staff   306  7 14 13:52 packages
drwxr-xr-x    8 kenmux  staff   272  7 14 13:52 pdk
drwxr-xr-x    7 kenmux  staff   238  7 14 13:52 platform_testing
drwxr-xr-x   20 kenmux  staff   680  7 14 13:58 prebuilts
drwxr-xr-x   31 kenmux  staff  1054  7 14 13:59 sdk
drwxr-xr-x   22 kenmux  staff   748  7 14 13:59 system
drwxr-xr-x    3 kenmux  staff   102  7 14 13:59 toolchain
drwxr-xr-x    5 kenmux  staff   170  7 14 14:00 tools
nawix-osx:android kenmux$ 
{% endhighlight %}

建議在做研究之前、先做備份：  

{% highlight text %}
nawix-osx:android kenmux$ cd ..
nawix-osx:bin kenmux$ rsync -av android android.bak
building file list ... done
created directory android.bak
android/
android/.DS_Store
android/Android.bp -> build/soong/root.bp
android/Makefile
android/bootstrap.bash -> build/soong/bootstrap.bash
android/.repo/

......

android/tools/test/connectivity/acts/tests/google/wifi/WifiScannerMultiScanTest.py
android/tools/test/connectivity/acts/tests/google/wifi/WifiScannerScanTest.py
android/tools/test/connectivity/acts/tests/google/wifi/WifiScannerTests.config
android/tools/test/connectivity/acts/tests/sample/
android/tools/test/connectivity/acts/tests/sample/SampleTest.py

sent 35318215998 bytes  received 13530514 bytes  8087844.00 bytes/sec
total size is 35274344962  speedup is 1.00
nawix-osx:bin kenmux$ 
{% endhighlight %}

羅列下屬專案：  

{% highlight text %}
nawix-osx:bin kenmux$ cd android
nawix-osx:android kenmux$ ~/bin/repo forall -c 'echo "$REPO_PATH -- $REPO_PROJECT"'
art -- platform/art
bionic -- platform/bionic
bootable/recovery -- platform/bootable/recovery
build -- platform/build
build/blueprint -- platform/build/blueprint
build/kati -- platform/build/kati
build/soong -- platform/build/soong
cts -- platform/cts

......

toolchain/binutils -- toolchain/binutils
tools/apksig -- platform/tools/apksig
tools/external/fat32lib -- platform/tools/external/fat32lib
tools/external/gradle -- platform/tools/external/gradle
tools/test/connectivity -- platform/tools/test/connectivity
nawix-osx:android kenmux$ 
{% endhighlight %}

檢視Git分支：  

{% highlight text %}
nawix-osx:android kenmux$ cd .repo/manifests/
nawix-osx:manifests kenmux$ ls -a
.		..		.git		default.xml
nawix-osx:manifests kenmux$ git branch -r
  m/master -> origin/master
  origin/adt_23.0.3
  origin/android-1.6_r1
  origin/android-1.6_r1.1
  origin/android-1.6_r1.2
  origin/android-1.6_r1.3
  origin/android-1.6_r1.4
  origin/android-1.6_r1.5

  ......

  origin/webview-m40_r1
  origin/webview-m40_r2
  origin/webview-m40_r3
  origin/webview-m40_r4
nawix-osx:manifests kenmux$ 
{% endhighlight %}

這樣、依次執行以下指令、便可以切換特定版本的Android原始碼：  

{% highlight text %}
~/bin/repo init -u https://android.googlesource.com/platform/manifest -b [ANDROID_BRANCH]
~/bin/repo sync
{% endhighlight %}

關於repo指令的使用說明，請參考[Repo command reference](https://source.android.com/source/using-repo.html)，或者Efalk資料檔之[Repo](http://www.efalk.org/Docs/Git/repo.html)，這裡就不多加著墨了。  

# <a name="android-kernel-source-download"></a>Android內核原始碼下載  

Android內核原始碼下載則比較簡單，直接從遠端存儲庫複製即可：  

{% highlight bash %}
git clone https://android.googlesource.com/kernel/common.git
git clone https://android.googlesource.com/kernel/goldfish.git
git clone https://android.googlesource.com/kernel/hikey-linaro
git clone https://android.googlesource.com/kernel/x86_64.git
git clone https://android.googlesource.com/kernel/exynos.git
git clone https://android.googlesource.com/kernel/msm.git
git clone https://android.googlesource.com/kernel/omap.git
git clone https://android.googlesource.com/kernel/samsung.git
git clone https://android.googlesource.com/kernel/tegra.git
{% endhighlight %}

注：  

- common專案、其原始碼通用  
- goldfish專案、其原始碼用於模擬器  
- 其他類別專案、其原始碼用於不同廠商/設備  

這裡下載的是common專案下的原始碼：  

{% highlight text %}
cd nawix-osx:~ kenmux$ cd ~/bin
nawix-osx:bin kenmux$ mkdir kernel
nawix-osx:bin kenmux$ cd kernel
nawix-osx:kernel kenmux$ git clone https://android.googlesource.com/kernel/common.git
Cloning into 'common'...
remote: Sending approximately 1.01 GiB ...
remote: Total 4614730 (delta 3857285), reused 4614730 (delta 3857285)
Receiving objects: 100% (4614730/4614730), 1.01 GiB | 1.04 MiB/s, done.
Resolving deltas: 100% (3857285/3857285), done.
Checking connectivity... done.
nawix-osx:kernel kenmux$ 
{% endhighlight %}

程式碼規模：  

<div style="text-align:center" markdown="1">
![2016-07-17-02.png]({{ page.imgurl }}/2016-07-17-02.png)  
</div>

檢視Git分支：  

{% highlight text %}
nawix-osx:kernel kenmux$ cd common/
nawix-osx:common kenmux$ ls -a
.	..	.git
nawix-osx:common kenmux$ git branch -av
* master                                                  fe8bf45 empty commit
  remotes/origin/HEAD                                     -> origin/master
  remotes/origin/android-3.10                             e11ce62 UPSTREAM: ppp: take reference on channels netns
  remotes/origin/android-3.10.y                           ed22006 update defconfig to enable ext4 encryption
  remotes/origin/android-3.14                             6186bde UPSTREAM: ppp: take reference on channels netns
  remotes/origin/android-3.18                             0433cb1 UPSTREAM: ppp: take reference on channels netns
  remotes/origin/android-3.4                              cb38f01 UPSTREAM: ppp: take reference on channels netns
  remotes/origin/android-4.1                              7436668 UPSTREAM: ppp: take reference on channels netns
  remotes/origin/android-4.4                              f8a27f3 UPSTREAM: ppp: take reference on channels netns
  remotes/origin/android-trusty-3.10                      78dd8a2 trusty-ipc: Add support for default Trusty IPC device
  remotes/origin/android-trusty-3.18                      182c86e trusty-virtio: Fix trusty_virtio_notify prototype
  remotes/origin/android-trusty-4.4                       096c9fe trusty-irq: Add support for secure interrupt mapping
  remotes/origin/bcmdhd-3.10                              4f750ce net: wireless: bcmdhd: Use different structs for scan reporting
  remotes/origin/brillo-m10-dev                           82c6fea ANDROID: dm-crypt: run in a WQ_HIGHPRI workqueue
  remotes/origin/brillo-m10-release                       22b269c BACKPORT: FROMLIST: mm: ASLR: use get_random_long()
  remotes/origin/brillo-m7-dev                            829d8c9 staging: ion: debugfs to shrink pool
  remotes/origin/brillo-m7-mr-dev                         829d8c9 staging: ion: debugfs to shrink pool
  remotes/origin/brillo-m7-release                        829d8c9 staging: ion: debugfs to shrink pool
  remotes/origin/brillo-m8-dev                            bd355208 Merge branch 'android-3.18-zram' into android-3.18
  remotes/origin/brillo-m8-release                        bd355208 Merge branch 'android-3.18-zram' into android-3.18
  remotes/origin/brillo-m9-dev                            7682bfa UPSTREAM: HID: hid-input: allow input_configured callback return errors
  remotes/origin/brillo-m9-release                        7682bfa UPSTREAM: HID: hid-input: allow input_configured callback return errors
  remotes/origin/deprecated/android-2.6.39                b8d9d67 Merge remote-tracking branch 'common/linux-bcm43xx-2.6.39' into common-39
  remotes/origin/deprecated/android-3.0                   563897d pstore: selinux: add security in-core xattr support for rootfs, pstore and debugfs
  remotes/origin/deprecated/android-3.10-adf              ac4ddb0 video: adf: expose adf_modeinfo_set_{name,vrefresh} to drivers
  remotes/origin/deprecated/android-3.3                   6c3978c Fix 3.3 merge error in: drivers: power: Add watchdog timer to catch drivers which lockup during suspend.
  remotes/origin/deprecated/android-3.4-compat            a7827a2 Merge branch 'android-3.4' into android-3.4-compat
  remotes/origin/deprecated/coupled-cpuidle               abcca36 cpuidle: coupled: add parallel barrier function
  remotes/origin/deprecated/experimental/android-3.10     3bfc1e9 sync: Fix a race condition between release_obj and print_obj
  remotes/origin/deprecated/experimental/android-3.10-rc5 4dc805f kconfig: Fix defconfig when one choice menu selects options that another choice menu depends on
  remotes/origin/deprecated/experimental/android-3.14     8e9fc46 xt_qtaguid: Fix boot panic
  remotes/origin/deprecated/experimental/android-3.18     1ba6b2f proc: make oom adjustment files user read-only
  remotes/origin/deprecated/experimental/android-3.8      e7f415e netfilter: xt_qtaguid: fix bad tcp_time_wait sock handling
  remotes/origin/deprecated/experimental/android-3.9      c98631b pstore: Update Documentation/android.txt
  remotes/origin/deprecated/experimental/android-3.9-rc2  33bc629 pstore: Update Documentation/android.txt
  remotes/origin/deprecated/experimental/android-3.9-rc7  6bc79ce fuse: Fix build after change to synchronize with library
  remotes/origin/deprecated/linux-bcm43xx-2.6.39          2277c65 net: wireless: bcmdhd: Report proper mcs rate mask
  remotes/origin/experimental/android-4.1                 01873ac trace: cpufreq: Add tracing for min/max cpufreq
  remotes/origin/experimental/android-4.4                 f782fce Revert "HACK: input: evdev: disable EVIOCREVOKE"
  remotes/origin/experimental/android-4.4-llvm            5e501df clang makefile
  remotes/origin/master                                   fe8bf45 empty commit
nawix-osx:common kenmux$ 
{% endhighlight %}
