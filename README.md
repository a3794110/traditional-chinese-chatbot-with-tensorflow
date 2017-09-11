# TensorFlow實現聊天機器人

語料文件是db/dgk_shooter_min.conv，來在於 https://github.com/rustch3n/dgk_lost_conv

參考論文：

Sequence to Sequence Learning with Neural Networks

A Neural Conversational Model

# 依賴

python3  

numpy  

sklearn  

tqdm  

tensorflow  

 
# 使用說明

## 第一步

取得dgk_shooter_min.conv(有附上)
放在目錄中

## 第二步

執行decode_conv.py

輸入：python3 decode_conv.py

輸出：sqlite3格式的數據庫文件db/conversation.db

## 第三步

執行data_utils.py

輸入：python3 data_utils.py

輸出：會生成bucket_dbs目錄，包含多個sqlite3格式數據庫，這是將數據按照大小分到不同的buckets裡面

例如問題ask的長度小於等於5，並且，輸出答案answer長度小於15，就會被放到bucket_5_15_db裡面

## 第四步 訓練

下面參數為測試，可自行調整，下面參數不大，故不會是好模型

size: 每層LSTM神經元數量

num_layers: 層數

num_epoch: 訓練多少倫（回合）

num_per_epoch: 每輪（回合）訓練多少樣本


輸入：

```
./train_model.sh
```

上面相當於運行：

```
python3 s2s.py \
--size 1024 \
--num_layers 2 \
--num_epoch 5 \
--batch_size 64 \
--num_per_epoch 500000 \
--model_dir ./model/model1
```

輸出：在 model/model1 目錄會輸出模型，上面的參數大概會生成700MB的模型

## 第五步 測試 

加了--test true 進入測試模式

輸入：

```
./train_model.sh test
```

上面腳本相當於運行：

```
python3 s2s.py \
--size 1024 \
--num_layers 2 \
--num_epoch 5 \
--batch_size 64 \
--num_per_epoch 500000 \
--model_dir ./model/model1 \
--test true
```

輸出：命令列輸入問題，機器人就會回答

db/dictionary.json 字典

# 測試結果(參數不一樣結果會不一樣喔,像我只是測試結果不要太期待)

> 你好
你好

> 你是誰
我是說，我們都是朋友

> 你從哪裡來
我不知道

> 你到哪裡去
你不是說你不是我的

> 你喜歡我嗎？
我喜歡你

> 你吃了嗎？
我還沒吃飯呢

> 你喜歡喝酒嗎？
我不知道

> 你討厭我嗎？
我不想讓你失去我的家人

> 你喜歡電影嗎？
我喜歡

> 陪我聊天吧
好啊

> 千山萬水總是情
你不是說你不是我的

> 你說話沒有邏輯啊
沒有


# 其他

很多論文進行 bleu 測試，這本來是測試翻譯模型的，對於對話沒太大的意義
不過如果想要，可以加 bleu 參數進行測試，例如

```
./train_model.sh bleu 1000
```
