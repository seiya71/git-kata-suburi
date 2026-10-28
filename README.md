# git-kata-suburi

## このリポジトリの意味
- 自分用のGit、GitHubの作業の回し方を反復で覚える為の場所です。
- 型は使うごとに洗練されることでしょう。つまり、変化します。

## １周の流れ
- issue → ブランチ → コミット → push → PR → マージ → 後片付け

### 1. issueを書く

```
[1028-1] fix: 昨日のファイル名を日付に直す
[1028-2] refactor: 昨日のファイルを年/月ディレクトリへ移す
[1028-3] feature: 今日の素振り会場を作る
[1028-4] chore: 素振り記録(連続日数・累計)を更新する
[1028-5] docs: log.mdに完了を記録する
※日付を4桁にする、例：10月28日→1028
```
### 2. ブランチ → コミット → push → PR → マージ → 後片付け
```bash
# 1. main を最新にしてからブランチを切る
git switch main          # または git checkout main
git pull
git switch -c feature/1028-3-add-today   # または git checkout -b ...

# 2. 修正して、変更を確かめる
git status
git diff  # 長いと less で開く（スペースで進む、q で終わる）

# 3. コミット
git add README.md  # README.mdの部分は更新するファイルの名前にする
git commit -m "feature: 今日の素振り会場を作る"
                     # コメントは「分類:やったこと」にする

# 4. push（初回は -u を付けると、次から git push だけで済む）
git push -u origin feature/1028-3-add-today

# 5. GitHub で PR を作る（base = main）→ マージ

# 6. 後片付け
git switch main
git pull
git branch -d feature/1028-3-add-today              # 手元のブランチ
git push origin --delete feature/1028-3-add-today   # GitHub のブランチ
git fetch --prune                                    # 消えた origin/... の表示を整理
```
## PRの書き方
- **base（どこに入れるか）と compare（どのブランチから）を必ず確認する**
- 1つの PR では1つのことだけ。数分で読める大きさにする
- 本文の形：
```
## 変更内容
- Today.md を作成。

## 理由
- 今日の素振り会場を用意するため。

## 変えていないもの
- 一昨日より前の記録は素振りの記録なので変えてません。

Closes #issue番号
```
## それぞれのPRで具体的にやること
- fix: Today.mdというファイルを昨日の日付にリネームする。
```
git mv Today.md <昨日の日付.md>
```
- refactor: 日付.mdのファイルを年/月ディレクトリへ移す
```
mkdir -p 2026/10
git mv <昨日の日付.md> 2026/10/<昨日の日付.md>
```
- feature: Today.mdというファイルを作成する。
```
touch Today.md
```
- chore: record.jsonの書き換えを行う
```
{
  "lastPractice": "2026-10-28",  # 今日の日付に書き換える
  "streak": 1,
  "totalDays": 1  # それぞれ+1だけする
}
```
- docs: log.mdに完了を記録する
```
- 2026-10-28 完了
```
