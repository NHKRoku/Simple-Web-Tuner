# Simple Web Tuner

M3U と XMLTV をバックエンドとして利用し、ブラウザ上でテレビ番組を簡単に視聴できる Web アプリケーションです。

## 📌 注意事項

### 1. URL の指定方法

Simple Web Tuner は、バージョンに応じて以下の URL 形式に対応しています。

#### シングルチューナー

ストリーム URL、チャンネル ID、ロゴ URL、XMLTV の URL を指定します。

**URL 形式**

```text
?m3u8=<ストリームURL>&tvgid=<チャンネルID>&logo=<ロゴURL>&xmltv=<EPG_URL>
```

**設定例**

```text
?m3u8=http://192.168.1.10:8000/nhk.m3u8&tvgid=nhk.jp&logo=https://www.nhk.jp/favicon.ico&xmltv=http://192.168.1.10/epg.xml
```

各パラメーターの説明：

| パラメーター | 説明 |
|---|---|
| `m3u8` | 視聴するストリームの URL |
| `tvgid` | XMLTV のチャンネル ID |
| `logo` | チャンネルロゴの URL |
| `xmltv` | XMLTV 形式の番組表データの URL |

#### その他のバージョン

M3U プレイリストのパスまたは URL を指定します。

**URL 形式**

```text
?m3u=<M3UファイルのパスまたはURL>
```

**ローカルパスの例**

```text
?m3u=/playlist.m3u
```

**リモート URL の例**

```text
?m3u=http://192.168.1.10:8000/playlist.m3u
```

### 2. 対応ストリーム形式

| バージョン | 対応形式 |
|---|---|
| シングルチューナー | HLS (`.m3u8`) |
| V1 | HLS (`.m3u8`) |
| V2 | HLS (`.m3u8`) |
| V1.5 | MPEG-TS (`.ts`) |
| V2.5 | MPEG-TS (`.ts`) |

**注意:** V1.5 および V2.5 では、MPEG-TS 形式のストリームを使用してください。

### 3. V1.5 / V2.5 の要件

#### 3.1. ストリーム形式

ストリームは MPEG-TS (`.ts`) 形式である必要があります。

#### 3.2. ARIB-B24 字幕の利用要件

ARIB-B24 字幕機能を完全に利用するには、以下の条件を満たす必要があります。

- 入力ストリームに ARIB 字幕データが含まれていること
- 字幕ストリームの形式が `0x0006`（Private Data）であること

#### 3.3. ストリームの確認例

以下は、BS アニマックス（BS236）のストリーム情報の例です。

```text
Program 1
  Metadata:
    service_name    : Service01
    service_provider: FFmpeg

  Stream #0:0[0x100]:
    Data: bin_data ([6][0][0][0] / 0x0006)

  Stream #0:1[0x101]:
    Video: h264 (High) ([27][0][0][0] / 0x001B),
    yuv420p(tv, bt709, progressive),
    960x720 [SAR 4:3 DAR 16:9],
    29.97 fps, 29.97 tbr, 90k tbn

  Stream #0:2[0x102]:
    Audio: aac (LC) ([15][0][0][0] / 0x000F),
    48000 Hz, stereo, fltp, 194 kb/s

  Stream #0:3[0x103]:
    Subtitle: arib_caption (Profile A) ([6][0][0][0] / 0x0006)

  Stream #0:4[0x104]:
    Data: bin_data ([6][0][0][0] / 0x0006)
```

**ポイント：**

- 映像ストリーム：H.264
- 音声ストリーム：AAC
- 字幕ストリーム：ARIB Caption（Profile A）
- 字幕データのストリームタイプ：`0x0006`

※ 字幕の表示可否は、ストリームに含まれる字幕データや配信環境にも依存します。

#### 3.4. EPGStation でのストリーム生成

上記の形式のストリームは、EPGStation のエンコードプロファイル **`M2TS-LL`** を使用して生成できます。

V1.5 / V2.5 で ARIB-B24 字幕を利用する場合は、生成されたストリームに必要な字幕データが含まれていることを確認してください。

---

## 📖 関連プロジェクト

- [EPGStation](https://github.com/l3tnun/EPGStation)

## 📝 補足

- URL 内の各パラメーターは、使用するバージョンの仕様に従って指定してください。
- M3U プレイリストや XMLTV データにアクセスできることを確認してください。
- リモートストリームを使用する場合は、ブラウザからのアクセスやネットワーク設定にも注意してください。
