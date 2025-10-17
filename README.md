# Proof-oriented programming in F*（日本語訳）

このリポジトリは、F* 公式ドキュメント  
[**Proof-oriented programming in F\***（原文）](https://github.com/FStarLang/PoP-in-FStar)  
の **日本語翻訳版** です。

---

## 📄 原文について
- **原著作者**: The F* Project Authors  
- **原文リポジトリ**: https://github.com/FStarLang/PoP-in-FStar  
- **ライセンス**: Apache License 2.0

---

## 🇯🇵 翻訳について
- **翻訳者**: timeler0  
- **翻訳ライセンス**: 本リポジトリの翻訳物は Apache License 2.0 の下で提供されます（詳しくは [LICENSE](./LICENSE) を参照）。  
- ※ 翻訳は正確性に最大限配慮していますが、内容の正確性は保証しません。**最新かつ正確な情報は必ず英語版を参照**してください。

---

## 🌐 公開ページ（GitHub Pages）
- https://timeler0.github.io/FStarDocumentJP/

> Sphinx 由来の `_static/` などを正しく配信するため、公開ルートに `.nojekyll` を配置しています。

---

## 🧾 ライセンス表示（抜粋）

Original work © 2016–2024 The F* Project Authors  
Original source: https://github.com/FStarLang/PoP-in-FStar.git  

Japanese translation © 2025 timeler0  
Licensed under the Apache License, Version 2.0  

- ライセンス全文はリポジトリ直下の [LICENSE](./LICENSE) をご覧ください。

---

## 🛠️ ビルド手順（任意：ローカルでSphinxビルドする場合）
この翻訳は [Sphinx](https://www.sphinx-doc.org/) でビルドしています。  
ローカルで再ビルドする際の簡易手順例です。

### 環境構築

ドキュメント作成環境のインストール
```Bash
sudo apt install -y python3-sphinx python3-venv
```

### 前準備
オリジナルドキュメントのクローン
```Bash
# ドキュメントの作成するディレクトリは任意だが、本ドキュメントでは ~/doc で以下作業を進めるものとする

mkdir fstar
cd fstar

# すでに clone 済みならスキップ
git clone https://github.com/FStarLang/PoP-in-FStar.git
# FStar と pulse もこの兄弟ディレクトリに置く
git clone https://github.com/FStarLang/FStar.git
git clone https://github.com/FStarLang/pulse.git
```

コピー元のディレクトリを登録
```Bash
cd ~/doc/fstar
export FSTAR_HOME=~/doc/fstar/FStar
export PULSE_HOME=~/doc/fstar/pulse
```

F* と Pulse 由来の .rst を book/ 配下にコピーします。
```Bash
cd ~/doc/fstar/PoP-in-FStar
make -C book prep
```

i18n 用の仮想環境 & ツール
```Bash
cd ~/doc/fstar/PoP-in-FStar/book
python3 -m venv .venv
source .venv/bin/activate   # 実行後、コマンド行の先頭に(venv)が追記される。
pip install -U sphinx sphinx-intl sphinx-rtd-theme
```

Sphinx 設定（conf.py）
book/conf.py に追記：
```python
# --- i18n settings ---
locale_dirs = ['locales']   # locales/ja/LC_MESSAGES/*.po をここで探す
gettext_compact = False     # 各 .rst ごとに分割（推奨）
```

翻訳元の抽出（.pot 生成）
```Bash
sphinx-build -b gettext . _build/gettext
```

日本語 .po を作成
```Bash
# cd ~/doc/fstar/PoP-in-FStar/book
sphinx-intl update -p _build/gettext -l ja
```

### 翻訳作業

仮想環境にない場合、i18n 用の仮想環境 & ツールの項目を実行し、仮想環境にする。

ここで作成された.poを下記に従って編集する
* #, fuzzy が付いていたら外す(当該行を削除)
* msgidの内容は触ってはならない。msgstrに訳文を記述する

### ビルド
作業が終わったら下記コマンドで html ドキュメントを作成する
```Bash
sphinx-build -b html -c . -D language=ja -E . _build/html
```

