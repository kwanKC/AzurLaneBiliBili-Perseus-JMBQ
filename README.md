fork 自 https://github.com/CapitalGrin/AzurLaneBiliBili-Perseus
## 目的
主要是想解决模拟器闪退的问题
根据[issue](https://github.com/CapitalGrin/AzurLaneBiliBili-Perseus/issues/6#issuecomment-2368122043)中提到的替换Egoistically原本的[Perseus](https://github.com/Egoistically/Perseus)为[JMBQ的库](https://github.com/JMBQ/azurlane)来解决模拟器闪退问题

## 现状
目前仅修改了`patch_perseus.sh`脚本中的内容，其中
```shell
if [ ! -d "Perseus" ]; then
    echo "Downloading Perseus"
    git clone https://github.com/kwanKC/JMBQ_Perseus
fi
```
