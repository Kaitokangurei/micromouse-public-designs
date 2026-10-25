# ハーフサイズ機のモータドライバ調査

調査日：2026-10-25（日本時間）

## 結論

とらねこ（ToraNeko.Mk3）に構成を合わせる場合、**走行用はMP6551を左右各1個、合計2個**を第一候補にする。公開概要とmotor.cで採用・制御方法を確認した。

対象はブラシ付きDCモータNFP-D0812-1-3.7×2、2セルLiPo、STM32F411CEU6の3.3V制御。吸引ファンは公開概要では別のNチャネルMOSFETで駆動する方針であり、走行用Hブリッジ2個とは別に扱う。

出典：[ToraNeko概要](https://github.com/Tokoro0917/ToraNeko.Mk3/blob/main/docs/overview.md)、[motor.c](https://github.com/Tokoro0917/ToraNeko.Mk3/blob/main/software/main-controller/TORANEKO.Mk3/App/drv/motor.c)

## 候補の比較

電源範囲は動作条件。最大電流・ピーク電流は連続して流せる保証値ではない。左右同時駆動時の熱、基板銅箔、電流波形で確認する。

| 型番 | 正逆転できるモータ数 / IC | モータ電源 | 電流の目安・表記 | パッケージ | 今回の位置付け |
|---|---:|---|---|---|---|
| **MP6551** | 1 | 2.5〜14V | ピーク±5A | QFN14、2.5×3mm | **とらねこ準拠。左右で2個** |
| DRV8835 | 2 | VM 0〜11V、別途ロジック2〜7V | 最大1.5A / ブリッジ | WSON12、2×3mm | 左右を1個に集約する候補 |
| DRV8836 | 2 | 2〜7V | 最大1.5A / ブリッジ | WSON12、2×3mm | 大会採用例あり。**2S直結は対象外** |
| DRV8837 | 1 | VM 0〜11V、別途ロジック1.8〜7V | 最大1.8A | WSON8、2×2mm | 小型の左右独立構成。2個必要 |
| DRV8838 | 1 | VM 0〜11V、別途ロジック1.8〜7V | 最大1.8A | WSON8、2×2mm | PH/EN制御。とらねこのコード変更が必要 |
| TC78H660FTG | 2 | 2.5〜16V | 最大2A、放熱条件に依存 | QFN16、3×3mm | 定電流制御を組み込む候補 |

メーカー出典：[MP6551製品情報](https://www.monolithicpower.com/en/products/motor-drivers-and-motor-controllers/mp6551.html)、[MP6551データシート](https://www.monolithicpower.com/en/documentview/productdocument/index/version/2/document_type/Datasheet/lang/en/sku/MP6551GQB/document_id/9795/)、[DRV8835](https://www.ti.com/product/DRV8835)、[DRV8836](https://www.ti.com/product/DRV8836)、[DRV8837](https://www.ti.com/product/DRV8837)、[DRV8838](https://www.ti.com/product/DRV8838)、[TC78H660FTGデータシート](https://toshiba.semicon-storage.com/info/TC78H660FTG_datasheet_en_20200714.pdf?did=68604&prodName=TC78H660FTG)。

DRV8835とDRV8836は似た名称でも電源条件が異なる。DRV8836の製品ページには見出しと詳細仕様に食い違いがあるため、特徴・本文の動作電源2〜7Vを採用した。大会機の電池が2Sでも、ドライバへそのまま接続しているとは判断できない。

## MP6551を選ぶ理由と制御条件

- 2S LiPoの満充電8.4Vが動作範囲内。
- 入力Highしきい値の最小値1.5Vから、STM32の3.3V出力で制御できる。
- 1個でブラシ付きDCモータ1個の正転・逆転・ブレーキを扱える。2つの半ブリッジを持つことと、2台をそれぞれ正逆転できることは別。
- 注文型番の基本はMP6551GQB。メーカー資料のテープ＆リール例はMP6551GQB-Z。ToraNekoの概要は梱包末尾までは指定していない。

上記の電気仕様は[メーカー製品情報](https://www.monolithicpower.com/en/products/motor-drivers-and-motor-controllers/mp6551.html)と[データシート](https://www.monolithicpower.com/en/documentview/productdocument/index/version/2/document_type/Datasheet/lang/en/sku/MP6551GQB/document_id/9795/)による。

公開motor.cではTIM4の100kHz PWMを使い、片方のINをHigh固定、もう片方をPWMにする。PWMの休止区間はブレーキ、停止も両INをHighにする方式。これはEN1・EN2で両出力が有効になっている前提である。ENをLowにすると該当出力はHi-Zになるので、INの2本だけで接続が完結するとは考えない。

| 接続 | STM32側 |
|---|---|
| 左IN1 | PB6 / TIM4_CH1 |
| 左IN2 | PB7 / TIM4_CH2 |
| 右IN1 | PB8 / TIM4_CH3 |
| 右IN2 | PB9 / TIM4_CH4 |

出典：[実際のPWM実装](https://github.com/Tokoro0917/ToraNeko.Mk3/blob/main/software/main-controller/TORANEKO.Mk3/App/drv/motor.c)。

100kHzを使う場合は、SR端子のスルーレート設定も確認する。データシートの500kHz表記はRSR=0Ω条件であり、全設定で同じ波形を得られる意味ではない。低デューティ時の波形・電流・発熱は実機で確認する。

## モータと電池の適合

NFPの公開仕様ではNFP-D0812-1-3.7は定格3.7V、端子抵抗4.4Ω、拘束電流840mA、無負荷電流30mA。拘束電流は定格電圧時の値であり、2SからPWM駆動した際の瞬時電流の保証値ではない。[モータメーカー仕様](https://nfpmotor.com/8mm-coreless-dc-motor-12mm-type-model-nfp-d0812)

ToraNekoの公開コードは電池電圧を読み、「指令電圧÷電池電圧」で駆動時間比を決め、指令を3.7V以下に制限している。

| 電池電圧 | 3.7V指令時の駆動時間比（計算値） |
|---|---:|
| 8.4V | 約44% |
| 7.4V | 50% |

**MP6551はモータ電圧を自動で3.7Vに降圧する部品ではない。** PWM中の駆動パルスは電池電圧相当になる。上表は平均電圧の近似であり、巻線電流・発熱まで保証しない。MP6551の過電流保護はIC保護であり、840mAのモータ用電流制限としては扱えない。始動・拘束時の電流と停止までの時間を実測して設計する。

## 周辺部品・回路で確認するもの

| 部品・信号 | 必要性 |
|---|---|
| VIN直近のセラミックコンデンサ | メーカー指定は100nF以上。配線を短くする |
| 入力バルクコンデンサ | 始動・PWM・回生時の電源変動から容量と耐圧を決める |
| EN1 / EN2の設定 | 出力有効化、起動・停止時の状態を決める |
| SR-GND間抵抗 | スイッチング速度とノイズの調整 |
| nFAULTプルアップ・監視 | オープンドレイン。使用する場合は3.3V系へプルアップ |
| CM1 / CM2の電流測定回路 | 使用する場合は外付け抵抗・ADC範囲などを設計。公開motor.cには電流制限制御はない |
| 電源・GND・出力の銅箔 | 放熱と電圧降下を考慮。モータ電流の戻り道を壁センサの受光回路から離す |

これは設計確認用の一覧。ToraNekoの実装済み部品値を全て転記したBOMではない。メーカーの推奨回路・PCBレイアウトと、使用する回路図を照合して値を確定する。[MP6551データシート](https://www.monolithicpower.com/en/documentview/productdocument/index/version/2/document_type/Datasheet/lang/en/sku/MP6551GQB/document_id/9795/)

## 大会で確認できた採用例

| 機体 | 公式テクニカルデータの記載 | 読み取れる範囲 |
|---|---|---|
| Spangle v5.4 / 徳永弦久 | 2024全日本ファイナル4位、NFP-D0812-1-3.7、DRV8836、電池2S | ドライバ供給電圧・降圧回路はこの表から未確認。2S直結可能という根拠にはしない |
| 金風 / 福田真悟 | 2025全日本ファイナル9位、NFP-D0812-1-3.7、DRV8835DSSR、電池2S | 同じモータで2chドライバを使う参考例 |

出典：[Spangle v5.4](https://www.ntf.or.jp/mouse/micromouse2024/Robot/AllJapan/TechMM12.html)、[金風](https://www.ntf.or.jp/mouse/micromouse2025/Robot/AllJapan/TechMM13.html)。採用型番・電池・成績は公式掲載内容。内部回路や連続電流の保証を示すものではない。

今回の選定は**MP6551×2でToraNekoに合わせる**。1個に集約する設計へ変更する場合はDRV8835、定電流機能を重視する場合はTC78H660FTGを比較対象にする。互換品としてそのまま差し替えるものではなく、回路・基板・制御を変更する。
