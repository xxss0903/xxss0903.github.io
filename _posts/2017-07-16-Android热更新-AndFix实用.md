
--
layout: blog
title: 'AndFix简单使用'
date: 2017-07-16 12:11:34
categories: blog
tags: code
image: ''
lead-text: 'AndFix使用'
---

```
最近使用了AndFix来进行热更新的入门使用，AndFix接入简单，理解容易，所以做个总结和使用
```

## 接入AndFix

* 添加依赖
[AndFix官方地址](https://github.com/alibaba/AndFix)
在依赖中添加
```gradle
dependencies {
	compile 'com.alipay.euler:andfix:0.5.0@aar'
}
```

* 代码中添加打布丁的代码
在程序的入口Application中添加代码

```java
private void initAndFix() {
        String appversion = null;
        try {
            appversion = getPackageManager().getPackageInfo(getPackageName(), 0).versionName;
            mPatchManager = new PatchManager(this);
            mPatchManager.init(appversion);//current version
            mPatchManager.loadPatch();

            // 加载布丁文件
            String patchPath = Environment.getExternalStorageDirectory().getAbsolutePath() + AND_FIX_PATCH_PATH;
            File patchFile = new File(patchPath);
            L.D("MainApplication#initAndFix " + "布丁文件路径 = " + patchPath);
            if (patchFile.exists()) {
                Toast.makeText(sContext, "布丁文件存在，开始加载布丁", Toast.LENGTH_SHORT).show();
                mPatchManager.addPatch(patchPath);
            } else {
                Toast.makeText(this, "布丁文件不存在", Toast.LENGTH_SHORT).show();
            }
        } catch (PackageManager.NameNotFoundException e) {
            Toast.makeText(sContext, "加载布丁失败 :" + e, Toast.LENGTH_SHORT).show();
            e.printStackTrace();
        } catch (Exception e) {
            Toast.makeText(sContext, "加载布丁失败 :" + e, Toast.LENGTH_SHORT).show();
            e.printStackTrace();
        }
    }

```

上面Java代码将先将布丁文件找到，然后加载到程序中，这里要注意布丁文件名字；使用AndFix修复在项目中要添加的代码就这么多了<br>


## 根据修复后apk和老的apk生成布丁文件

* 使用apatch脚本生成布丁文件
根据上面在github上面下载的 [生成布丁文件](https://github.com/alibaba/AndFix/raw/master/tools/apkpatch-1.0.3.zip)，解压文件获取脚本<br>

```sh
./apkpatch.sh -f app-release-fix.apk -t app-release-bug.apk -o apkpatch-output -k 123456.keystore -p 123456 -a 123456 -e 123456


./apkpatch.sh 具体的命令脚本 
-f new_fixed_apk 修复完成的apk 
-t old_bug_apke 线上待修复的apk 
-o patch_output 补丁patch的输出目录 
-k keystore 签名文件 
-p pwd 签名密码 
-a alias 别名 
-e alias_pwd 别名密码

```
将生成的文件夹下的 *.apatch  文件更名为在MainApplication中加载的方法一样名字

* 下发布丁文件
如果是在本地调用,通过adb将apatch文件push到手机里，下次开启就会调用加载布丁的方法了

如果是网络，则从后台下载到布丁文件然后保存到对应位置并开始加载布丁




