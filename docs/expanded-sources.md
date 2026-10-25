# 大会・ブログ・X・SNSから探した追加資料

調査日：2026-10-25（日本時間）

既存の[全日本大会関連の機体一覧](../README.md#一覧)を補う資料です。追加した項目は、公開プロジェクト6件、技術ブログ6件、大会・講演資料6件、SNS・動画4件の計22件です。同じプロジェクトについて複数媒体を掲載した場合があります。22機体という意味ではありません。

## 確認範囲

- 公式大会記録、作者のGitHub・ブログ、本人による公開投稿を優先しました。
- 大会成績が作者README・SNSの申告にとどまる場合は、その旨を記載しています。全日本出場歴と混同しません。
- ソフトや設計ファイルの存在・説明を確認した資料集であり、ビルド・製造・実機走行の検証は行っていません。
- Xの投稿本文を直接取得できないものは、作者ブログに掲載された埋め込み投稿を確認した記録です。Xの直接閲覧済みとは扱いません。
- 元の設計データは複製せず、公開先へのリンクを収録しています。ライセンスや部品の入手性は利用時に確認してください。

## 1. 追加の公開プロジェクト：6件

| プロジェクト | 作者・団体／発見元 | 確認できた公開データ | 大会との関係・確認状況 |
|---|---|---|---|
| [ZoroBot3](https://github.com/OPRobots/ZoroBot3) | OPRobots／[作者団体サイト](https://oprobots.org/) | `source_code`、`pcb_files`、`3d_model`。STM32F405、吸引機構、磁気エンコーダ。GPL-3.0表記 | READMEに2025年Micromouse Portuguese Contest、OSHWDem、RoboChallengeなどの優勝記録。成績は作者の記載で、各大会公式との個別照合は未実施。全日本の当該機出場は未確認 |
| [NeuroMouse26](https://github.com/enkhbold470/neuromouse26) | Enkhbold Ganbold / Yu Hong Chen／Redditでの公開報告 | ESP32-S3・PlatformIOのファームウェア、設定、テスト・解析ツール、製作資料。BFS、PID、IR、エンコーダ処理。MIT表記 | READMEと本人投稿でAAMC 2026総合3位を申告。主催者の順位表との照合は未実施。完成PCB/CAD一式の公開は未確認 |
| [UKMARSBOT](https://github.com/ukmars/ukmarsbot) | UK Micromouse and Robotics Society／団体公式 | 回路設計、ハードウェア、機構、資料。`ecad`、`hardware`、`mechanical`。制御ソフトは[examples](https://github.com/ukmars/ukmarsbot-examples)と[mazerunner](https://github.com/ukmars/ukmarsbot-mazerunner)に分離 | 大会運営団体が公開する競技・学習用プラットフォーム。特定の入賞機の大会当日版とは扱わない |
| [Gemini](https://github.com/ukmars/gemini) | UKMARS／[公式紹介](https://ukmars.org/) | Raspberry Pi Pico系。KiCad、回路図、BOM、機構、サンプルコード。`ecad`、`mechanical`、`sampleCode`。MIT表記 | UKMARSのUKMARSBOT後継プラットフォーム。[組立ガイド](https://ukmars.org/projects/gemini/)あり。迷路探索・壁沿い・ライン競技向け |
| [micromouse_sim_2025](https://github.com/akiaki96/micromouse_sim_2025) | akiaki96／[作者記事](https://akiaki96.hatenablog.com/entry/2025/12/04/192608) | Pythonの最短経路導出デモ。斜め走行、直線加速、ターン時間を考慮するダイクストラ法の説明と実装案内 | 作者記事からコードへのリンクを確認。記事執筆時はREADMEがmain、コードがmasterと記載。現行ブランチ構成・実機版は未確認 |
| [MM Maze Viewer](https://github.com/Naophis/mm_maze_viewer) | 平松直人 / Naophis／[作者ブログ](https://naophis.hatenablog.com/) | VS Code拡張のソース。迷路の壁編集、SVGプレビュー、`.maze`保存、16×16・32×32テンプレート | 大会機そのものではなく、迷路の編集・シミュレーション用ツール。READMEで機能と利用方法を確認 |

## 2. 技術ブログ・設計説明：6件

| 資料 | 内容・使いどころ | データ公開の確認範囲 |
|---|---|---|
| [2019年に設計した機体の紹介 — harattaMouse](https://harattamouse.hatenablog.com/entry/2020/01/25/150801) | 白いハラシーマウスver3.00。STM32F411、MA702、二輪機の足回り、センサ小基板、モータマウントの説明 | 回路図へのリンク・基板レイアウト画像・設計寸法を記事で確認。EAGLE/Fusion 360の編集可能ファイル一式は未確認。壁センサ回路の極性訂正があるため訂正版を参照 |
| [なめらかなスラロームの設計③ C++による実装 — KERI's Lab](https://www.kerislab.jp/posts/2020-04-25-slalom-trajectory-3/) | 加減速とスラローム軌道のC++実装、姿勢・状態・軌道のクラス設計 | [micromouse-control-module](https://github.com/kerikun11/micromouse-control-module)への公開コードリンク。記事は当時の版を説明 |
| [マイクロマウスの壁情報を安全にバックアップする — KERI's Lab](https://www.kerislab.jp/posts/2021-12-12-maze-saving-method/) | 壁観測ログを不揮発メモリに保存する設計。リセット後の探索情報復元を考える資料 | 記事内のコードとGitHub実装への案内を確認。基板設計資料ではない |
| [迷路クラスの設計と迷路エディタの実装 — KERI's Lab](https://www.kerislab.jp/posts/2020-05-04-python-maze-editor/) | Python・matplotlibによる迷路クラス、壁編集、読み書き、保存形式 | [micromouse-maze-data](https://github.com/kerikun11/micromouse-maze-data)に対応。Xの実演投稿を作者記事に掲載 |
| [スラロームのオフセット距離を補正する話 — なおフィス](https://naophis.hatenablog.com/entry/2025/12/08/000000) | スラロームのオフセット距離補正についての技術記事。既存Astraea/Banshee資料の補足 | 技術解説として収録。この記事単独に対応する独立コード一式は未確認 |
| [マイクロマウス（頭はいいけど足腰が弱い版）— Qoosky](https://www.qoosky.dev/techs/23b248e9e0/) | マイコン、センサ、モータードライバ・昇圧回路、配置図、製作過程 | 記事で回路図・配置図のZIPとGitHubプログラムへの案内を確認。GitHubリンク先の現行内容は未確認。大会出場歴は未確認 |

## 3. 大会結果・迷路・講演資料：6件

| 大会・団体 | 取得できる情報 | 用途・注意点 |
|---|---|---|
| [NTF 2025年度 地区大会・学生大会の記録](https://www.ntf.or.jp/mouse/micromouse2025/local.html) | 関西・金沢・北陸信越・東北・九州・中部・東日本・学生大会の結果、走行タイム、テクニカルデータへのリンク | 機体名と製作者を照合する一次資料。大会年度と実際の開催年は公式表記を確認 |
| [マイクロマウス委員会 関西支部](https://mmk.rulez.jp/) | 2026年度を含む大会結果、迷路図、競技動画・写真などの記事 | 一覧から各結果記事を開いて利用。機体コードの公開を保証する資料ではない |
| [APEC MicroMouse公式](https://apec-conf.org/attendees/special-events-social-activities/micromouse/) | APEC 2026の結果概要、詳細報告・YouTubeへのリンク | 公式概要ではDecimus-5A、KOGUMA-CHAN.Mk3、PIGGY-ONEの上位入賞を確認。ページは翌年案内と共用で内容が更新される |
| [UKMARS Contest Results](https://ukmars.org/category/contest-results/) | 英国の春・秋・Minosなどの競技結果アーカイブ | 海外機体・作者名の探索起点。迷路探索以外の競技も含む |
| [UKMARS Autumn 2026結果](https://ukmars.org/2026/10/autumn-contest-results/) | 2026-10-04開催の機体名・参加者・スコア。ハーフサイズ、通常サイズなど | 公式結果表。通常サイズMaze Solver SeniorではMr Fusion Part IIが最上位。設計公開の有無は別途確認が必要 |
| [MINOS 2025](https://ukmars.org/events/minos-2025/) | 技術講演スライドへのリンク。軌道計画、スリップ検出、照明、PCB配線幅、Mazerunner32など | 大会・技術交流会の公式資料案内。個別の外部スライドファイルの中身までは未確認 |

## 4. X・SNS・走行動画：4件

| 媒体・出典 | 確認した内容 | 公開データへのつながり・確認方法 |
|---|---|---|
| X（旧Twitter）：けり / @kerikun11。[作者記事の埋め込み投稿](https://www.kerislab.jp/posts/2020-05-04-python-maze-editor/)／[プロフィール](https://x.com/kerikun11) | 2020-04-30付の迷路エディタ実演投稿が作者記事に掲載されている | 対応コードは[micromouse-maze-data](https://github.com/kerikun11/micromouse-maze-data)。確認根拠は作者ブログ内の投稿掲載。Xの投稿本文を直接取得できたものではない |
| X（旧Twitter）：ししかわ / @meganetaaan。[作者の製作まとめ](https://rt-net.jp/mobility/archives/16375)／[プロフィール](https://x.com/meganetaaan) | 2021-02-10付のスラローム完走報告が製作記事に掲載されている | 対応データは[M5Mouse](https://github.com/meganetaaan/M5Mouse)。確認根拠は作者記事に掲載された投稿で、Xの直接閲覧は未確認 |
| [Reddit：NeuroMouse26公開報告](https://www.reddit.com/r/robotics/comments/1vt5zpv/we_built_a_micromouse_on_perfboard_with_an/) | 2026-08-20の本人投稿。基板の作り直し、ESP32-S3のエンコーダ処理、PID、調整経験など | [公開リポジトリ](https://github.com/enkhbold470/neuromouse26)へ案内。AAMC 3位は本人申告として扱う |
| [YouTube：UKMARS Spring 2025大会映像](https://www.youtube.com/watch?v=SQk96S-ChrM) | 2025-04-05開催の俯瞰映像。説明欄に機体名・タイム・各走行への時刻が掲載 | 軌道・走り方を調べる資料。映像を載せているだけでは、その機体のコードやCADが公開されているとは扱わない |

## 用途別の入口

- **回路と3Dデータも含めて比較**：ZoroBot3、UKMARSBOT、Gemini。
- **実装・調整方法を比較**：NeuroMouse26、既存一覧のAstraea/Banshee、KERIの制御記事。
- **探索・斜め経路を研究**：micromouse_sim_2025、迷路クラスの記事、MM Maze Viewer。
- **大会の機体名・作者名を照合**：NTF、APEC、UKMARSの公式結果。
- **走り方や製作過程を確認**：ブログ、Reddit、Xの作者記事内掲載、YouTube。

## 更新記録

- 2026-10-25：大会・ブログ・SNSへ調査範囲を広げ、22項目を追加。作者申告と公式記録、コードと解説のみの資料、X直接閲覧とブログ内掲載を区別。
