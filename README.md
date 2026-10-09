# Pico2UltraHiResUSBDDC_VerFinal_11FFT_NoOverSampling

## Description
11-band FFT spectrum display version without oversampling and FIR filtering.

This project is based on the original work by ArqAlice (MIT License).

Originally developed for the PICO_AUDIO_PACK environment (RP2040), this firmware has been reworked for Raspberry Pi Pico2 (RP2350).

It does not support RP2040-based boards.

GPIO assignments have been remapped to GP9 / GP10 / GP11 to maintain compatibility with the original PICO_AUDIO_PACK hardware.

Additional modifications have been implemented to improve audio quality, stability, and compatibility.

Operation has been verified with PCM5100 on PICO_AUDIO_PACK hardware.

## 概要
本プロジェクトは、RP2350（Raspberry Pi Pico2）上で動作する USB Audio Class 1.0 準拠の USB-DDC（Digital to Digital Converter）です。

USB経由で入力された2ch PCMオーディオ信号をオーバーサンプリングやFIRフィルタ処理を行わず、そのままI²Sインターフェイスへ出力します。

また、OLEDディスプレイ上にリアルタイム11バンドFFTスペクトラム表示機能を搭載しています。

## 特長
- USB Audio Class 1.0 準拠
- Raspberry Pi Pico2 (RP2350) 専用
- No Oversampling
- No FIR Filtering
- ダイレクトPCM出力
- Bit-Perfect Playback Compatible
- OLEDリアルタイム11バンドFFT表示
- DMA + PIOによる低遅延I²S伝送
- Dual Core RP2350対応
- PICO_AUDIO_PACK互換

## 対応DAC
- TI PCM5100
  
※ PCM5100はPICO_AUDIO_PACK環境で動作確認済み

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

## Bit-Perfect Playback
ビットパーフェクト再生を行う場合は、Windows側の再生デバイス音量を100%に設定してください。
Windowsの音量を下げるとデジタルボリューム処理が適用され、USBオーディオデバイスへ送られるPCMデータが変更される場合があります。
より正確な再生を行うために、WASAPI Exclusive Mode（排他モード）の使用を推奨します。

## Playback Notes
高サンプリングレート再生時は、ご使用のPC環境や負荷状況により音切れが発生する場合があります。
音切れが発生する場合は、再生ソフトウェアまたはOSのサンプリングレート設定を下げてご使用ください。
システム負荷を低減することで再生が安定する場合があります。

Audio dropouts may occur during high sample-rate playback depending on the host PC performance and system load.
If audio dropouts occur, reduce the playback sample rate in the operating system or audio player settings.
Lower sample rates may improve playback stability.

## FFT Spectrum Display
Real-time 11-band FFT spectrum analyzer.

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

- FFT_LEN = 1024

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
1. VSCodeをインストール
2. Raspberry Pi Pico Extensionをインストール
3. 本リポジトリをクローン
4. VSCodeでビルド
5. 生成されたUF2ファイルを書き込み

## ライセンス
MIT License

Original Copyright (c) 2025 ArqAlice

Additional modifications for RP2350 support, PICO_AUDIO_PACK compatibility, OLED FFT display, and No Oversampling version by Toshimiyu.

## 参考文献
- USB Audio Class 1.0 Specification
- Raspberry Pi Pico SDK
- LUFA USB Framework
