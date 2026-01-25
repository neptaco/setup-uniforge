---
name: release-version
description: GitHub Action のリリース手順をガイドする。「リリース」「バージョン更新」「v1 更新」と言われた場合に使用
disable-model-invocation: true
argument-hint: "[version e.g., v1.1.0]"
---

# GitHub Action リリース手順

このスキルは setup-uniforge のリリース手順をガイドします。

## 引数

`$ARGUMENTS` にバージョン番号を指定できます（例: v1.1.0）。指定がない場合は確認します。

## 手順

### 1. 現在の状態を確認

以下を実行して現状を把握してください：

```bash
# 現在のタグ一覧
git tag -l | sort -V

# v1 が指しているコミット
git rev-parse v1

# 最新コミット
git rev-parse HEAD

# v1 と HEAD の差分
git log v1..HEAD --oneline
```

### 2. セマンティックバージョンを決定

変更内容に応じてバージョンを決定：
- **パッチ (v1.0.1)**: バグ修正、ドキュメント修正
- **マイナー (v1.1.0)**: 新機能追加（後方互換性あり）
- **メジャー (v2.0.0)**: 破壊的変更

### 3. リリースを作成

GitHub CLI でリリースを作成します：

```bash
# タグを作成してプッシュ
git tag v1.x.x
git push origin v1.x.x

# GitHub リリースを作成（これにより自動的に v1 が更新される）
gh release create v1.x.x --generate-notes
```

または GitHub Web UI から:
1. https://github.com/neptaco/setup-uniforge/releases/new
2. 新しいタグを作成（例: v1.1.0）
3. "Generate release notes" をクリック
4. "Publish release" をクリック

### 4. メジャーバージョンタグの更新確認

リリース公開後、`.github/workflows/update-major-version.yml` が自動実行されます。

Actions タブで実行状況を確認：
```bash
gh run list --workflow=update-major-version.yml --limit=1
```

### 5. 確認

更新が正しく行われたか確認：

```bash
# v1 が新しいリリースを指しているか
git fetch --tags --force
git rev-parse v1
git rev-parse v1.x.x  # 同じコミットを指すはず
```

## 手動で v1 を更新する場合

ワークフローが失敗した場合やワークフローを追加する前のリリースの場合：

```bash
git tag -fa v1 -m "Update v1 tag to v1.x.x"
git push origin v1 --force
```

## 注意事項

- メジャーバージョンタグ（v1）の force push は正常な動作です
- ユーザーは `@v1` で常に最新の v1.x.x を取得できます
- 破壊的変更がある場合は新しいメジャーバージョン（v2）を作成してください
