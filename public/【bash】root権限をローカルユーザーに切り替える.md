---
title: 【bash】root権限をローカルユーザーに切り替えるコマンド
tags:
  - 'Linux'
  - 'Linuxコマンド'
  - 'root'
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

# Command

:::note alert
用法・用量を守って正しくお使いください。
責任は負いかねます。
:::

指定したディレクトリorファイルの所有権を、現在のユーザーに変更します。

```bash
sudo chown -R $(whoami):$(whoami) <ディレクトリ or ファイル名>
# ex.) sudo chown -R $(whoami):$(whoami) root_file.txt
```
