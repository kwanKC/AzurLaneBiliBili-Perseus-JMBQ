fork 自 https://github.com/CapitalGrin/AzurLaneBiliBili-Perseus
## 目的
主要是想解决模拟器闪退的问题 - 目前暂时用不了

根据[issue](https://github.com/CapitalGrin/AzurLaneBiliBili-Perseus/issues/6#issuecomment-2368122043)中提到的替换Egoistically原本的[Perseus](https://github.com/Egoistically/Perseus)为[JMBQ的库](https://github.com/JMBQ/azurlane)来解决模拟器闪退问题

## 现状
目前仅修改了`patch_perseus.sh`脚本中的内容
```shell
# 修改获取Perseus的仓库地址
if [ ! -d "Perseus" ]; then
    echo "Downloading Perseus"
    git clone https://github.com/kwanKC/JMBQ_Perseus    # 原本是https://github.com/Egoistically/Perseus
fi

# 修改git clone后的目录名
echo "Copy Perseus libs"
rm JMBQ_Perseus/1    # 空文件，可忽略，之前尝试直接用改好的UnityPlayerActivity.smali来替换的时候用的
cp -r JMBQ_Perseus/. com.bilibili.AzurLane/lib/

# 修改sed追加写入的内容
echo "Patching Azur Lane with Perseus=========="
oncreate=$(grep -n -m 1 'onCreate' com.bilibili.AzurLane/smali_classes2/com/unity3d/player/UnityPlayerActivity.smali | sed  's/[0-9]*\:\(.*\)/\1/')
sed  -i '/\.method protected onCreate/{N;s/\(.*\)\n\(.*\)/\1\n\2\n    const-string v0, "JMBQ"\n\n    invoke-static {v0}, Ljava\/lang\/System;->loadLibrary(Ljava\/lang\/String;)V/}' com.bilibili.AzurLane/smali_classes2/com/unity3d/player/UnityPlayerActivity.smali
cat com.bilibili.AzurLane/smali_classes2/com/unity3d/player/UnityPlayerActivity.smali
echo "Patching Azur Lane with Perseus done===="
```
根据JMBQ的使用指南，需要修改UnityPlayerActivity.smali的位置与Egoistically原本的[Perseus](https://github.com/Egoistically/Perseus)不同，所以换了`sed`语句
通过Actions里workflow的执行结果来看，成功的按照JMBQ指定的方式，在`.locals 2`的下方添加了指定的代码（[Perseus Build #15](https://github.com/kwanKC/AzurLaneBiliBili-Perseus-JMBQ/actions/runs/11249131313/job/31275472265)）

## 问题
- 将原有的`Perseus.ini`改名为`JMBQ.ini`后，闪退的情况依然存在，由于打开即闪退，判断应该是修改不完整
