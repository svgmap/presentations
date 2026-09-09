# [Universal Web Video Player](./moviePlayer.html)

A lightweight, single-page video player designed for GitHub Pages and static web hosting. Supports **HLS (.m3u8)** and **MP4 (.mp4)** with URL hash-based parameter controls and multi-language VTT subtitles.

GitHub Pages などの静的ホスティング環境に最適化された軽量な1ファイル完結型Web動画プレイヤーです。**HLS (.m3u8)** および **MP4 (.mp4)** に対応し、URLハッシュパラメータによる動的コントロールや多言語VTT字幕をサポートします。

---

## Features / 特長

- **HLS & MP4 Auto-Detection**: Seamlessly plays both HLS stream files (`.m3u8`) and standard MP4 files (`.mp4`).
  - **HLS / MP4 自動判別**: HLS形式と通常のMP4形式を自動で判別して再生。
- **Bypass GitHub Limits**: Bypass GitHub's 100MB single-file limit by converting large videos into HLS segments using FFmpeg.
  - **GitHub 100MB制限の回避**: FFmpegでHLS化することで、100MBを超える大容量動画もGitHub Pagesで快適に配信可能。
- **URL Hash Control**: Dynamically set video source, start time, document title, and subtitles via URL hash.
  - **URLハッシュ制御**: URLのハッシュパラメータで動画ソース、再生開始位置、ページタイトル、字幕を動的に指定可能。
- **Multi-Language Subtitles**: Supports multiple WebVTT (`.vtt`) subtitle tracks with native browser CC controls.
  - **多言語字幕対応**: ブラウザ標準の字幕切り替えメニュー（CC）に対応した多言語WebVTTをサポート。
- **No Build Tools Needed**: Single vanilla HTML/JS file using `hls.js` via CDN.
  - **ビルド不要**: CDN経由の `hls.js` と Vanilla JS のみで動作する単一HTML構成。

---

## URL Hash Parameters / パラメータ仕様

Parameters are passed using `URLSearchParams` format after `#` in the URL:  
URLの末尾（`#` 以降）に以下のパラメータを指定します。

| Parameter | Description (English) | 説明 (日本語) | Example / 例 |
| :--- | :--- | :--- | :--- |
| `src` / `url` | Path to `.m3u8` or `.mp4` file | 動画ファイル（`.m3u8` / `.mp4`）のパス | `src=stream.m3u8` |
| `t` / `start` | Start position in seconds | 再生開始位置（秒） | `t=90` (1m30s) |
| `title` | Page title shown on browser tab | ブラウザタブに表示するタイトル | `title=Demo` |
| `vtt` | Subtitle tracks (`lang:path:label;...`) | 字幕トラック（`言語:パス:ラベル;...`） | `vtt=ja:ja.vtt:日本語;en:en.vtt:EN` |

---

## Usage Examples / 使用例

### 1. Default (Loads `./stream.m3u8`) / デフォルト再生
https://<username>.github.io/<repo>/

### 2. MP4 Video with Start Time & Title / MP4の特定位置再生
https://<username>.github.io/<repo>/#src=demo.mp4&t=45&title=Demo%20Video

### 3. HLS Video with Multi-Language Subtitles / HLS再生 ＋ 多言語字幕
https://<username>.github.io/<repo>/#src=video/stream.m3u8&title=Lecture&vtt=ja:subs/ja.vtt:日本語;en:subs/en.vtt:English

---

## Preparing Large MP4 Files (FFmpeg) / 大容量MP4のHLS変換方法

To bypass GitHub's 100MB single-file limit, convert your MP4 file into HLS segments without re-encoding (takes only a few seconds):

GitHubの100MB単一ファイル制限を回避するには、FFmpegを使って無再エンコード（数秒で完了）でHLS変換します。

ffmpeg -i input.mp4 -c copy -f hls -hls_time 10 -hls_playlist_type vod stream.m3u8

### Generated Files / 生成されるファイル名

Executing the command generates the following files in the same directory:  
コマンドを実行すると、同じディレクトリ内に以下のファイル群が生成されます。

- `stream.m3u8` : Master playlist file / 目次（インデックス）ファイル
- `stream0.ts`, `stream1.ts`, `stream2.ts` ... : Segmented video chunks (~10s each) / 約10秒ごとに分割された動画本体ファイル群

Upload all generated `.m3u8` and `.ts` files to your repository.  
生成された `.m3u8` とすべての `.ts` ファイルをリポジトリにコミット/Pushして公開してください。

---

## License / ライセンス

This project is licensed under the **Mozilla Public License Version 2.0 (MPL-2.0)**.  
See the LICENSE file for full details.
