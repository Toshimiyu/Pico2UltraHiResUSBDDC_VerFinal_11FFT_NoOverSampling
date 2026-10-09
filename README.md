# Pico2UltraHiResUSBDDC_VerFinal_11FFT_NoOverSampling
## Description
- 11-band FFT spectrum display version without oversampling and FIR filtering.
- This project is based on the original work by ArqAlice (MIT License).
- Originally developed for the PICO_AUDIO_PACK environment (RP2040), this firmware has been reworked for Raspberry Pi Pico2 (RP2350).
- It does not support RP2040-based boards.
- GPIO assignments have been remapped to GP9 / GP10 / GP11 to maintain compatibility with the original PICO_AUDIO_PACK hardware.
- Additional modifications have been implemented to improve audio quality, stability, and compatibility.
- Operation has been verified with PCM5100 on PICO_AUDIO_PACK hardware.

# Pico2UltraHiResUSBDDC_VerFinal_11FFT_NoOverSampling
## Description
- 11-band FFT spectrum display version without oversampling and FIR filtering.
- This project is based on the original work by ArqAlice (MIT License).
- Originally developed for the PICO_AUDIO_PACK environment (RP2040), this firmware has been reworked for Raspberry Pi Pico2 (RP2350).
- It does not support RP2040-based boards.
- GPIO assignments have been remapped to GP9 / GP10 / GP11 to maintain compatibility with the original PICO_AUDIO_PACK hardware.
- Additional modifications have been implemented to improve audio quality, stability, and compatibility.
- Operation has been verified with PCM5100 on PICO_AUDIO_PACK hardware.

## 概要

- 本プロジェクトは、RP2350（Raspberry Pi Pico2）上で動作する USB Audio Class 1.0 準拠の USB-DDC（Digital to Digital Converter）です。
- USB経由で入力された2ch PCMオーディオ信号をオーバーサンプリングやFIRフィルタ処理を行わず、そのままI²Sインターフェイスへ出力します（NOS化）。
- OLEDディスプレイ上にリアルタイム11バンドFFTスペクトラム表示機能を搭載しています。

## 特長

- USB Audio Class 1.0 準拠

- Raspberry Pi Pico2 (RP2350) 専用

- No Oversampling (NOS)

- No FIR Filtering

- ダイレクトPCM出力

- Bit-Perfect Playback Compatible

- OLEDリアルタイム11バンドFFT表示

- DMA + PIOによる低遅延I²S伝送

- Dual Core RP2350対応

- PICO_AUDIO_PACK互換

- PICO_AUDIO_PACK互換


## 対応DAC

- TI PCM5100

※ PCM5100はPICO_AUDIO_PACK環境で動作確認済み

## ハードウェア改修履歴

### Ver1:

Pico2側

- MPUにヒートシンク追加

- VSYS/GND間に X7R 10μF + C0G 0.1μF 追加

PicoAudioPack側 

- LDO入力部に X7R 10μF + C0G 0.1μF 追加

- LPF定数 C 2200pF → 1200pF へ変更

### Ver2:

PicoAudioPack側

- LDO入力部に PLMCAP 1μF 追加

- AVDD/GND間に C0G 0.1μF 追加

- CVDD/GND間に C0G 0.1μF 追加

### Ver3:

PicoAudioPack側

- AVDD/GND間に PLMCAP 3.3μF 追加

###  Ver4 (最新):

Pico2側

- 3V3/GND間に X7R 3.3μF + C0G 0.1μF 追加

- PCM5100A DVDD/GND間に PLMCAP 0.1μF 追加

- ソフトウェアオーバーサンプリング解除

- ソフトウェアFIRフィルタ解除（NOS化）

## 測定実績・パフォーマンス評価 (Ver4)

※本測定結果は、標準状態の Pico2 および Pico Audio Pack ではなく、 実際に運用している上記モディファイ済み構成に対して実施されたものである。

また、電源やUSB接続環境についても、特別な処置を施した環境で測定しているため、全ての結果や動作を保証するものではありません。

### 測定について
- SB-1240改によるループバック測定（24bit / 96kHz、RMS -10dB入力、300回平均）において、電源デカップリングの最適化とNOS化により以下の優れた特性を確認しています。

- 無音時ノイズフロアの改善: 1kHz付近の不要成分（スプリアス）がVer3と比較して約10dBの大幅な低下（-108.52dB → -118.44dB）を達成。

- 歪み特性の改善: 1kHz THDが 0.00110% へ改善（2次高調波が約3dB減少）。20kHz THD+Nが約13%改善（0.03719% → 0.03220%）。

- 混変調歪み (IMD 19kHz + 20kHz): 差周波（1kHz成分）が約8.5dB大幅に改善（-107.70dB → -116.18dB）。

- 矩形波応答: FIRフィルタおよびオーバーサンプリングの解除により、20kHz以降の高調波成分がしっかりと保持され、高域の再現性と生々しさが向上。

### 改修に至った理由
- ノーオーバーサンプリング化とソフトウェアFIRフィルターの削除により、サンプルレート切り替え時の、ノイズ削減と周波数特性の改善を行っております。

- DAC側でオーバーサンプリング処理＆FIRフィルターが実装されており、必要ないとの判断に至ったため。

- オーディオストリームをなるべく加工せず送る方が音質的に有利と判断したため。

- 元ソースコード状態において、MPUがオーバークロック状態に設定されているため、ヒートシンクが必要と判断して取り付けています。（発熱による動作停止を防ぐのが目的）

## USB入力仕様

- USB Audio Class 1.0
- 2ch Stereo
- 16bit / 24bit PCM
- 44.1kHz
- 48kHz
- 88.2kHz
- 96kHz

## I²S出力仕様
- I²S 32bit Format
- Stereo
- Input Sample Rate Follow Mode
- No Sample Rate Conversion
- No Oversampling
- Bit-Perfect Playback

※　ビットパーフェクト再生を行う場合は、Windows側の再生デバイス音量を100%に設定してください。
- Windowsの音量を下げるとデジタルボリューム処理が適用され、USBオーディオデバイスへ送られるPCMデータが変更される場合があります。
- より正確な再生を行うために、WASAPI Exclusive Mode（排他モード）の使用を推奨します。

## Playback Notes
- 高サンプリングレート再生時は、ご使用のPC環境や負荷状況により音切れが発生する場合があります。
- 音切れが発生する場合は、再生ソフトウェアまたはOSのサンプリングレート設定を下げてご使用ください。
- システム負荷を低減することで再生が安定する場合があります。

- Audio dropouts may occur during high sample-rate playback depending on the host PC performance and system load.
- If audio dropouts occur, reduce the playback sample rate in the operating system or audio player settings.
- Lower sample rates may improve playback stability.

## FFT Spectrum Display
### Real-time 11-band FFT spectrum analyzer.

Center frequencies:

- 45Hz

- 90Hz

- 125Hz

- 360Hz

- 700Hz

- 1.4kHz

- 2.8kHz

- 5.6kHz

- 9.8kHz

- 18kHz

- 22kHz

FFT Configuration:
- FFT\_LEN = 1024

The OLED display provides a real-time 11-band spectrum visualization optimized for RP2350 performance and low display latency.

## 使用技術
- RP2350 (Raspberry Pi Pico2)

- DMA

- PIO

- Dual Core Processing

- LUFA USB Audio Class

- I²S Digital Audio Output

## ピンアサイン
### I²S
- DATA : GP9

- BCLK : GP10

- LRCK : GP11

### I²C
- SDA : GP6

- SCL : GP7

### Control
- DAC ENABLE : GP5

- POWERMODE SW : GP0

## ビルド方法
- VSCodeをインストール
- Raspberry Pi Pico Extensionをインストール
- 本リポジトリをクローン
- VSCodeでビルド
- 生成されたUF2ファイルを書き込み

## ライセンス
- MIT License
- Original Copyright (c) 2025 ArqAlice
- Additional modifications for RP2350 support, PICO_AUDIO_PACK compatibility, OLED FFT display, and No Oversampling version by Toshimiyu.

## 参考文献
- USB Audio Class 1.0 Specification
- Raspberry Pi Pico SDK
- LUFA USB Framework

