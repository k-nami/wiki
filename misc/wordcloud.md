# wordcloud

## 参考
https://qiita.com/hotpot/items/caaa10f31875433ef874

## setup

### install mecab
[参考](https://self-development.info/mecab%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%97%E3%81%A6python%E3%81%A7%E4%BD%BF%E3%81%86%E3%80%90windows%E3%80%91/)

1. [リンク](https://github.com/ikegami-yukino/mecab/releases)からインストーラー入手
2. インストール．辞書の文字コードをUTF-8に指定．その他はデフォルト設定．
3. 環境変数のPATHにC:\soft\MeCab\bin￥を追加
4. コマンドプロンプトでmecab -v，表示されることを確認

### 出力画像用の日本語フォント
1. [サイト](https://moji.or.jp/ipafont/ipaex00401/)からフォントを入手．今回はIPAex明朝 (Ver.004.01)．
2. 解凍して，.pyと同じ階層に置く

### pythonパッケージ
```
pip install wordcloud mecab
```

## code
```
import MeCab
from wordcloud import WordCloud

file = "<DIR2TEXT>"

# encoding指定しないと止まる
# https://itips.krsw.biz/python-decode-error-cp932-illegal-multibyte/
with open(file, encoding="utf-8") as f:
    texts = f.readlines();
text = " ".join(texts)

# 除外したいワード．何度か実行して手作業設定
exclude_keys = ["てる", "の", "ある", "ない", "する", "こと", "できる", "いる"];
exclude_keys = exclude_keys + ["なる", "やる", "れる", "そう", "ため", "られる", "それ", "これ"]

m = MeCab.Tagger("-Ochasen")
node = m.parseToNode(text)
words = []
word = ""
candidate = ["名詞", "動詞", "形容詞"]
while node:
    hinshi = node.feature.split(",")[0]
    if hinshi in candidate:
        origin = node.feature.split(",")[6]
        if not origin in exclude_keys:
            word = word + " " + origin
    node = node.next

fpath = "./ipaexm.ttf"
wordcloud = WordCloud(
    background_color = "white", 
    font_path = fpath, 
    width = 600, 
    height = 400,
    min_font_size = 15
)
wordcloud.generate(word)
wordcloud.to_file("./wordcloud.png")
```