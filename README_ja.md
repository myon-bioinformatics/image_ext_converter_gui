
画像をまとめて **別拡張子へ一括変換**する軽量 GUI ツールです。  

Pillow ベースで **PNG / JPEG / WEBP / AVIF / HEIC / TIFF / BMP / ICO** に対応（AVIF/HEIC は任意プラグインが必要）。

---

## 特徴

- 🖱️ **GUI だけで完結**（ファイル/フォルダ指定）
- 🗂️ **フォルダ再帰・拡張子パターン**（例：`*.png;*.jpg`）
- 🧰 **形式ごとの保存オプション**（横並びの簡素 UI、拡張子に応じて自動有効化）
- 🧠 **EXIF 自動回転・EXIF 継承**（JPEG/TIFF/HEIC）
- 🫥 **透過の安全処理**（アルファ非対応形式は白背景で合成）
- 🧵 **並列ジョブ**で高速変換
- 🔌 **任意プラグイン**：`pillow-heif`（HEIC/HEIF）、`pillow-avif-plugin`（AVIF）

---

## 対応形式と主なオプション

| 出力形式 | 主なオプション | 備考 |
|---|---|---|
| PNG  | `optimize`, `compress_level` | ロスレス。高圧縮は時間増 |
| JPEG/JPG | `quality`, `progressive`, `subsampling` | 透過は白背景で合成 |
| WEBP | `lossless` または `quality`, `method` | `lossless=true` で `quality` 無効 |
| AVIF | `quality` | `pillow-avif-plugin` 必要 |
| HEIC/HEIF | `quality` | `pillow-heif` 必要 |
| TIFF | `compression` | 簡略化のため透過は白背景合成 |
| BMP | （なし） | 透過は白背景合成 |
| ICO | `sizes`（例：`16 32 48 64`） | 複数サイズ同梱可 |

---

## 必要環境

- Python 3.8+
- 必須: `Pillow`
- GUI: `FreeSimpleGUI` **または** `PySimpleGUI`（どちらか片方でOK）
- 任意（形式拡張）:
  - `pillow-heif`（HEIC/HEIF 出力）
  - `pillow-avif-plugin`（AVIF 出力）

```bash
# 最小
pip install Pillow FreeSimpleGUI
# あるいは
# pip install Pillow PySimpleGUI

# 形式拡張（任意）
pip install pillow-heif
pip install pillow-avif-plugin
```

## 使い方
```bash
python image_ext_converter_gui.py
```

フォルダは ; 区切りのパターン、サブフォルダ再帰 に対応（例：*.png;*.jpg;*.webp）

ターゲット拡張子、出力先（未指定なら元と同じ場所）、上書き、並列ジョブ数を設定

下部の 保存オプション を調整（対象拡張子に関係ある項目だけが有効化）

「変換開始」を押すと変換・進捗・ログが表示されます

## FAQ
Q. 透過 PNG を JPEG にしたら白背景になった
JPEG はアルファを持てないため、白背景で合成して保存しています。背景色を変えたい場合は flatten_alpha() の bg=(255, 255, 255) を変更してください。

Q. EXIF は維持される？
JPEG/TIFF/HEIC 保存時に EXIF を継承します。読み込み時は ImageOps.exif_transpose() で 自動回転を適用します。

Q. AVIF/HEIC を選べない
プラグイン未導入です。pillow-avif-plugin（AVIF）、pillow-heif（HEIC/HEIF）をインストールしてください。

Q. 保存に失敗する
Pillow の版差で未対応パラメータがあるときは、未対応キーを自動スキップして再保存を試みます。解決しない場合はログを Issue に貼ってください。

## スクリーンショット
docs/screenshot_main.png（メイン画面）

docs/screenshot_options_jpeg.png（JPEG オプション）

docs/screenshot_options_png.png（PNG オプション）

## 構成例
```
.
├─ image_ext_converter_gui.py
├─ README.md
├─ README_ja.md
└─ docs/
   ├─ screenshot_main.png
   ├─ screenshot_options_jpeg.png
   └─ screenshot_options_png.png
```

## 注意点
PySimpleGUI v4 では Multiline の placeholder_text は使えません（本ツールは未使用）。

高圧縮（例：PNG compress_level=9、WEBP method=6）は 処理時間が増加します。

TIFF は簡略化しています（透過は白背景合成）。

変更履歴
v0.1.0：初公開（GUI 一括変換、形式別の横並びオプション、EXIF 自動回転/継承、並列ジョブ）
