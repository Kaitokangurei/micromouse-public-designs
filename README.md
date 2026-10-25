# マイクロマウス公開製作データ集

全日本マイクロマウス大会の出場記録・テクニカルデータを手がかりに調査した、公開GitHubリポジトリの一覧です。

整理日：2026-10-25（日本時間）

## この資料の見方

- 大会出場記録と公開データの対応を示しています。大会当日の設計・プログラムと、現在の公開版が一致することを保証するものではありません。
- 「未確認」は、今回の調査で公開ファイルを確認できなかった項目です。非公開・不存在という意味ではありません。
- 全日本大会と全日本学生大会は区別して記載しています。年度は大会の公式表記です。
- 本資料はリンクと調査概要をまとめたものです。元のソースコード・CAD・基板データの再配布はしていません。利用条件は各リポジトリのライセンスを確認してください。

## 一覧

| 機体 | 製作者 | 大会記録・対応関係 | 公開GitHub | ソフトウェア | 回路・基板 | 機構・CAD |
|---|---|---|---|---|---|---|
| Astraea / AstraeaNova | 平松 直人 | [2024年度全日本・ファイナル15位](https://www.ntf.or.jp/mouse/micromouse2024/Robot/AllJapan/TechMM07.html)。公式紹介からGitHubへリンク | [Naophis/Astraea](https://github.com/Naophis/Astraea) | ESP32-S3向け全ソース、ログ解析・調整ツール | 基板ファイル | 機構解説あり。CAD一式は未確認 |
| Banshee / Banshee.SSS | 平松 直人 | [2023年度全日本・ファイナル6位](https://www.ntf.or.jp/mouse/micromouse2023/Robot/AllJapan/TechMM16.html)。公式紹介からGitHubへリンク | [Naophis/Banshee](https://github.com/Naophis/Banshee) | ESP32-S3向け全ソース、ログ・調整関連 | 基板ファイル | 足回り・吸引機構の説明あり。CAD一式は未確認 |
| KERISE v3 | 大貫 椋太郎 | 公開READMEに2017年全日本・決勝13位、特別賞を記載（この行の戦績の根拠は作者README） | [kerikun11/micromouse-kerise-v3](https://github.com/kerikun11/micromouse-kerise-v3) | ESP32ファームウェア、MATLAB、ログ | KiCad | Fusion 360 |
| KERISE v4 | 大貫 椋太郎 | [2019年度全日本の公式記録集](https://www.ntf.or.jp/mouse/micromouse2019/result/mm2019recode_v104.pdf)に掲載 | [kerikun11/micromouse-kerise-v4](https://github.com/kerikun11/micromouse-kerise-v4) | ESP32ファームウェア、MATLAB、記録データ | KiCad | Fusion 360 |
| type6 | 浜砂 智 | [2023年全日本学生大会・優勝](https://www.ntf.or.jp/student2023/大会結果/)。[関西地区大会の公式紹介](https://www.ntf.or.jp/mouse/micromouse2023/Robot/kansai/TechMM31.html)からGitHubへリンク | [satoshihamasuna/micromouse_type6](https://github.com/satoshihamasuna/micromouse_type6) | STM32F411向けプログラム | 未確認 | 未確認 |
| MicroNaos | 沼田 楽来 | [2022年度全日本・クラシック競技出場](https://www.ntf.or.jp/mouse/micromouse2022/MM2022_Recodes_V100.pdf)。公式記録にGitHubを掲載 | [rn-t/MicroNaos_open](https://github.com/rn-t/MicroNaos_open) | STM32向けC++ファームウェア | KiCad回路・基板 | 基板外形DXF |
| M5Mouse | 石川 真也 | [2020年度全日本・クラシック競技完走](https://www.ntf.or.jp/mouse/micromouse2020/MM2020_Robots/index.html) | [meganetaaan/M5Mouse](https://github.com/meganetaaan/M5Mouse) | STM32制御、M5Stack側UI・通信 | 回路図・基板設計 | 外装データ |
| ExtraICE4 | 飯田 一輝 | [旧機体ExtraICEが2014年全日本出場](https://ntf.or.jp/mouse/micromouse2014/20141114contest.pdf)。公開データは同じ製作者の後継機。[ExtraICE4の関西地区大会公式紹介](https://www.ntf.or.jp/mouse/micromouse2023/Robot/kansai/TechMM22.html) | [elkel53930/extraice4](https://github.com/elkel53930/extraice4) | Rust。関連ソフトは別リポジトリ | KiCad 7 | STL、Fusion 360リンク |
| Zirconia v2.0（公開改訂版） | 中川 範晃による改訂 | [2025年度全日本に初期版ベースのZirconia-Blueが出場](https://www.ntf.or.jp/mouse/micromouse2025/Robot/AllJapan/TechMM02.html)（操作者：合田 直史）。公開v2.0と出場機は別仕様 | [viola0222/zirconia](https://github.com/viola0222/zirconia) | STM32F411の動作確認サンプル。迷路完走コードなし | 回路図、Gerber、部品表 | STEP閲覧リンク、製作マニュアル |

## 各リポジトリの読みどころ

### Astraea / Banshee

全ソースと基板ファイルのほか、部品選定、足回り、電源やノイズへの対処などの説明があります。吸引機・ログ解析・調整方法を調べる資料になります。同じ製作者のシリーズなので、設計の変化も比較できます。

### KERISE v3 / v4

ファームウェア、KiCad、Fusion 360が同じリポジトリにあります。回路・機構・ソフトを横断して調べる際の候補です。

### type6

STM32F411を使った旧ハーフサイズ機のプログラムです。全日本学生大会の優勝記録と対応付けられますが、公開コードが大会当日の版と同一かは未確認です。

### MicroNaos

クラシックマウスの回路・基板・C++コードを確認できます。作者READMEでは、公開時の編集により回路シンボルが基板発注時と一部異なることが説明されています。

### M5Mouse

STM32とM5Stackを組み合わせた構成です。[作者の製作まとめ](https://rt-net.jp/mobility/archives/16375)から、回路・外装・プログラムの説明をたどれます。[全日本完走報告](https://rt-net.jp/mobility/archives/15848)も公開されています。

### ExtraICE4

KiCadの設計と3Dプリント向けSTLを確認できます。ソフトについては、作者の記事で次の公開先が案内されています。

- [ESP32向けソフトウェア](https://github.com/elkel53930/micromouse-std-esp32/)
- [迷路探索アルゴリズム](https://github.com/elkel53930/mm_maze_solver)
- [作者による公開先の説明](https://gist.github.com/elkel53930/0267b5a0c3a6483003aa259fa5eb4644)

### Zirconia v2.0

部品表、製作マニュアル、回路・製造データ、デバイス動作確認コードをまとめて確認できます。READMEに、サンプルコードのみでは迷路を完走できないことが明記されています。

## 調査の起点となった参考リポジトリ

[ToraNeko.Mk3](https://github.com/Tokoro0917/ToraNeko.Mk3) はハーフサイズ機のソフト、KiCad回路・基板、機構設計、設計資料をまとめた公開リポジトリです。今回の資料を探す起点として記載しています。名称が似た大会出場機との同一性は、この資料では断定しません。

## 関連する公開ライブラリ・迷路データ

| 分類 | リンク | 内容 |
|---|---|---|
| 車体制御 | [micromouse-control-module](https://github.com/kerikun11/micromouse-control-module) | C++の加減速・スラローム軌道生成、軌道追従、フィードバック制御 |
| 迷路処理 | [micromouse-maze-library](https://github.com/kerikun11/micromouse-maze-library) | C++の迷路ライブラリ |
| 迷路データ | [micromouse-maze-data](https://github.com/kerikun11/micromouse-maze-data) | 迷路データ、表示・編集関連 |
| 迷路データ | [micromouseonline/mazefiles](https://github.com/micromouseonline/mazefiles) | クラシック・ハーフサイズ・練習用のテキスト迷路。作者は誤りや重複の可能性を明記 |
| シミュレータ | [mackorone/mms](https://github.com/mackorone/mms) | 実機なしで迷路探索プログラムを試すシミュレータ |

## 更新時の確認項目

1. 大会公式記録で機体名・製作者・競技・年度を確認する。
2. 作者の公開リポジトリと機体の対応を確認する。
3. 公開ファイルを見て、ソフト・回路・機構のどこまで含まれるか記録する。
4. 大会当日の版、後継機、派生版、参考ライブラリを区別する。
5. 出典リンクと整理日を更新する。
