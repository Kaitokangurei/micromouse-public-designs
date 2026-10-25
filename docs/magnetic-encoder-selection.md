# 磁気式エンコーダの選定：ToraNeko.Mk3と同じ構成

調査日：2026-10-25（日本時間）

## 選定結果

**AS5047Pを左右各1個、合計2個採用する。** ユーザー指定の「とらねこと一緒」を優先し、ToraNeko.Mk3の公開BOM・接続設定・実装コードを基準にする。以前提示した最高速度4m/sなどの仮条件は採用しない。

| 項目 | 採用する構成 | 根拠 |
|---|---|---|
| 磁気式角度センサ | AS5047P ×2 | 公開BOM |
| 検出軸 | 左右モータ軸（資料・コードの記載） | 概要、encoder.h |
| 接続 | SPI2、左右のCSを個別制御 | 確定ピン表、CubeMX設定 |
| 電源系 | 3.3V | 機体のMCU・センサ用電源方針。ICの3.3V接続はメーカー指定に従う |
| 読み取り | 14bit、0〜16383、モータ1回転あたり16384段階 | encoder.h / encoder.c |
| 読み取り周期 | 2kHz（0.5ms） | robot_params.h、メイン制御README |
| SPI設定 | モード1、MSB先頭、6.25MHz | CubeMX設定 |
| 通信単位 | 16bitフレームを8bit×2バイトで送受信 | CubeMX設定、encoder.c |
| 磁石 | 左右各1個、直径3mmの設計 | 機構README、基板配置ガイド |
| エンコーダ基板 | 左右各1枚、6×8.5mm、厚さ0.6mm、メイン基板に垂直にはんだ付け | 基板配置・接続用フットプリントの説明 |
| 磁石とICの隙間 | 設計値0.5mm | 基板配置ガイド。磁気的な成立を保証する値ではない |

出典：[BOM](https://github.com/Tokoro0917/ToraNeko.Mk3/blob/main/hardware/kicad/TORANEKO.Mk3/BOM.csv)、[概要](https://github.com/Tokoro0917/ToraNeko.Mk3/blob/main/docs/overview.md)、[基板配置・エンコーダ接続](https://github.com/Tokoro0917/ToraNeko.Mk3/blob/main/hardware/kicad/TORANEKO.Mk3/README.md)、[機構](https://github.com/Tokoro0917/ToraNeko.Mk3/blob/main/mechanical/exported/README.md)、[encoder.h](https://github.com/Tokoro0917/ToraNeko.Mk3/blob/main/software/main-controller/TORANEKO.Mk3/App/drv/encoder.h)、[encoder.c](https://github.com/Tokoro0917/ToraNeko.Mk3/blob/main/software/main-controller/TORANEKO.Mk3/App/drv/encoder.c)、[CubeMX設定](https://github.com/Tokoro0917/ToraNeko.Mk3/blob/main/software/main-controller/CUBEMX_SETUP.md)、[制御周期定数](https://github.com/Tokoro0917/ToraNeko.Mk3/blob/main/software/main-controller/TORANEKO.Mk3/App/config/robot_params.h)。

## 必要な部品

| 部品 | 数量 | 選定状態 |
|---|---:|---|
| AS5047P | 2個 | IC型番を確定。ToraNekoのBOMは梱包区分を含む注文末尾型番を記載していない |
| 回転検出用磁石 | 2個 | 外径3mmの機構に合わせる。購入型番、材質、着磁仕様は確認した資料では未特定 |
| エンコーダ用小基板 | 2枚 | ToraNekoの外形・接続用フットプリントを参照 |
| 電源デカップリング部品 | 左右各一式 | メーカーの3.3V動作用接続図と実際の回路図に合わせて値・数量を確定 |

AS5047Pは14bitの軸上磁気角度センサで、SPIとABIなどの出力を持つ。メーカー製品ページには3.3V/5V電源、最大28000rpm、磁場範囲35〜70mTが記載されている。これはICの仕様であり、機体の最高速度や実測精度ではない。[メーカー製品情報（Infineon）](https://www.infineon.com/part/AS5047P)

3.3V動作では内部レギュレータ関連端子の接続が5V動作と異なるため、3.3Vを電源端子の1本につなぐだけで済むと考えず、該当版の推奨回路を確認する。[メーカーAS5047Pデータシート](https://look.ams-osram.com/m/d05ee39221f9857/original/AS5047P-DS000324.pdf)

**磁石は外径が同じだけでは同等品にならない。** 着磁方向・磁界・偏心・取付位置を確認してから発注する。0.5mmは機構の設計隙間であり、任意の直径3mm磁石に適用できる推奨距離ではない。

## STM32との接続

| 信号 | STM32F411CEU6側 |
|---|---|
| SCK（左右共通） | PB13 / SPI2_SCK |
| MISO（左右共通） | PB14 / SPI2_MISO |
| MOSI（左右共通） | PB15 / SPI2_MOSI |
| 左CS | PA8 |
| 右CS | PC15 |
| 電源・GND | 3.3V・共通GND |

IMUはSPI1、エンコーダ2個はSPI2を使う。基板README冒頭に残る「IMUと共通SPIバス案」より、確定ピン表とCubeMX設定を優先する。[確定ピン表](https://github.com/Tokoro0917/ToraNeko.Mk3/blob/main/software/main-controller/PIN_REQUIREMENTS.md)、[CubeMX設定](https://github.com/Tokoro0917/ToraNeko.Mk3/blob/main/software/main-controller/CUBEMX_SETUP.md)

## 分解能とソフトウェア

ToraNekoの公開実装はABIパルスのカウントではなく、SPIで14bit角度を読み、前回との差分を積算する。したがって、ABIの最大4096カウント/回転とSPIの16384段階/回転を混同しない。

機構資料の減速比41/11、概要のタイヤ径14mm、モータ軸検出という前提からの計算値：

- 車輪1回転相当の積算値：16384×41/11 ≈ **61068カウント**
- 1カウントの移動距離換算：π×14÷61068 ≈ **0.000720mm**
- この値は量子化の換算値。実際の走行精度はタイヤ変形、滑り、ギアの遊び、磁石の偏心、センサ誤差に左右される。

概要ではギア比がTODOのままだが、機構READMEとencoder.hには41/11の記載があるため、ここでは後者を根拠とする。完成機の実寸・減速比との照合は別途必要。

公開コードは角度要求0xFFFFを2フレーム送り、後のフレームで応答を取得する。左右の前進方向に合わせて左-1、右+1としている。これはToraNekoの取付方向での値なので、組立後に前へ転がして確認する。作者READMEには2026-10-22に角度読み取りを実機確認した旨がある。こちらで実機検証したという意味ではない。[公開実装](https://github.com/Tokoro0917/ToraNeko.Mk3/blob/main/software/main-controller/TORANEKO.Mk3/App/drv/encoder.c)、[作者の実装状況](https://github.com/Tokoro0917/ToraNeko.Mk3/blob/main/software/main-controller/README.md)

差分の折返し処理は、正常に読めたサンプル間の回転量が半回転未満という前提。2kHzでは理論境界が60000rpmとなるが、読み取り欠落時は間隔が延びる。これはICの最大28000rpmとは別の条件である。encoder.hの約16000rpmというコメントはこの式の境界値ではないため、ここでは流用しない。

## 組立時に確認する項目

1. 磁石の回転中心とICの検出中心を合わせる。磁石の購入仕様を確定する。
2. 起動時のDIAAGC（AGC、MAGL、MAGH、COF、LF）を読み、磁石の強弱や検出状態を確認する。公開encoder.cに診断表示がある。
3. 手回しで角度の連続性と左右の前進方向を確認する。
4. モータを回した状態で通信エラー、読み取り欠落、磁石の偏心による周期誤差を確認する。

**今回確定するのはAS5047P×2とToraNeko準拠のSPI構成。磁石の購入型番と着磁仕様は未確定として管理する。**
