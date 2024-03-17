# Github-Auto-Release
Githubの特定のブランチにプッシュを行った際、リポジトリ内のファイルを圧縮したzipファイル作成し、自動でReleaseを作成します。

## 使い方
1. ワークフローをコピーする  
このリポジトリにある`.github/workflows/create-release.yml`をコピーして、使用したいリポジトリに貼りつける。

2. トークンを生成する  
[Fine-grained personal access tokens](https://github.com/settings/tokens?type=beta) にアクセスして、新しくトークンを作成する。

3. トークンをリポジトリに設定する  
リポジトリの`Setting`から、`Security`セクションにある`Secret and variable`の`Actions`に移動し、`New Repository secret`から以下のシークレットを作成する。

| シークレット名 | 値 |
| --- | :---: |
| `TOKEN` | ステップ1で作成したトークンの値 |

## カスタマイズ
* zipファイルに含めないファイルを追加する  
Create zip fileステップ内の`grep -vE ".gitattributes"`にファイル名を追加することで、zipファイルに特定のファイルを含めないようにすることができます。

```yml
      # ブランチに含まれるファイルをzipファイルにまとめる
      # README.mdと.gitattributesは含めない
      - name: Create zip file
        run: |
          git ls-files | grep -vE ".gitattributes|README.md" | zip -q -@ release.zip
```