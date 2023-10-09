# YouTube-Uploads-Notifyer

YouTubeの新着アップロードをX(=Twitter)とDiscordで自動通知します。  
Automatically notifying new YouTube uploads on X(=Twitter) and Discord.

## Parameters
- `youtube_channel_id`
  - YouTubeのチャンネルIDです。カスタマイズページの「チャンネル URL」の欄に「UC」から始まる文字列があります。
  - Ex. `UCx8MhUwaY1_7C45USYgzVeg`
- `youtube_playlist_id` 
  - YouTubeのプレイリストIDです。プレイリストのURLに「PL」から始まる文字列があります。
  - Ex. `PLGF8z568UBSJLFXYGS4FabcKT_66Wv_ug`
- `hash_tag`
  - 通知を行う時のハッシュタグです
- `x_consumer_key`
  - Xのコンシューマキーです。Freeのプランで良いので、Xの開発者登録を行う必要があります。Xに通知する時は入力してください。
- `x_consumer_secret`
  - Xのコンシューマシークレットです。Freeのプランで良いので、Xの開発者登録を行う必要があります。Xに通知する時は入力してください。
- `x_access_key`
  - Xのアクセスキーです。Freeのプランで良いので、Xの開発者登録を行う必要があります。Xに通知する時は入力してください。
- `x_access_secret`
  - Xのアクセスシークレットです。Freeのプランで良いので、Xの開発者登録を行う必要があります。Xに通知する時は入力してください。
- `discord_webhook_url`
  - Discordのウェブフックです。Discordに通知する時は入力してください。

## Example

このアクションはリポジトリにいくつかのファイルを保存します。
コミットをするように設定が必要です。

```yml
name: Sample
on:
  schedule:
    - cron: "0 * * * *" #毎時0分
  # 「アクション」タブからこのワークフローを手動で実行できます。
  workflow_dispatch:

jobs:
  samplejob:
    runs-on: ubuntu-latest
    steps:
      - name: checkout repo content
        uses: actions/checkout@v3 # checkout the repository content to github runner.
      - name: execute YouTube-Uploads-Notifyer
        uses: Twilight-Wings-Studio/YouTube-Uploads-Notifyer/@main
        with:
          youtube_channel_id: ${{vars.YOUTUBE_CHANNEL_ID}}
          youtube_playlist_id: ${{vars.YOUTUBE_PLAYLIST_ID}}
          hash_tag: ''
          x_consumer_key: ${{secrets.X_CONSUMER_KEY}}
          x_consumer_secret: ${{secrets.X_CONSUMER_SECRET}}
          x_access_key: ${{secrets.X_ACCESS_KEY}}
          x_access_secret: ${{secrets.X_ACCESS_SECRET}}
          discord_webhook_url: ${{secrets.DISCORD_WEBHOOK_URL}}
      - name: execute commit
        run: |
          git config user.name github-actions
          git config user.email github-actions@github.com
          git add .
          git commit -m "crongenerated"
          git push
        continue-on-error: true
      - if: ${{ steps.git-pr-release.outputs.error != '' && !contains(steps.git-pr-release.outputs.error, 'No pull requests to be released') }}
        run: |
          echo '::error ::git-pr-release failed with "${{ steps.git-pr-release.outputs.error }}"'
          exit 1
```

## Twilight Wings Studio Streamer Apps

[なりきりVTuber](https://store.steampowered.com/app/1632350?utm_source=GitHub&utm_content=YouTube-Uploads-Notifyer)  
[自動切るクリップ](https://store.steampowered.com/app/1632800?utm_source=GitHub&utm_content=YouTube-Uploads-Notifyer)
