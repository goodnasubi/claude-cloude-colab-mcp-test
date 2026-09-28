# claude-cloude-colab-mcp-test

[googlecolab/colab-mcp](https://github.com/googlecolab/colab-mcp) を Claude Code から使うためのセットアップです。
colab-mcp は、ローカルで動くエージェント（Claude Code）とブラウザで開いている Google Colab のセッションを橋渡しする MCP サーバーです。

## 前提条件

- **ローカル環境で動く Claude Code**（CLI / デスクトップ / IDE 拡張）
  - colab-mcp は「クライアントがユーザーの端末上で動いていること」と `notifications/tools/list_changed` のサポートを必要とします。
  - Colab を開いたブラウザと同じマシンで動かす必要があるため、クラウド上の Claude Code（claude.ai/code）では実際の Colab 接続はできません。
- [`uv`](https://docs.astral.sh/uv/)（`uvx` コマンド）
  ```sh
  pip install uv
  # または
  curl -LsSf https://astral.sh/uv/install.sh | sh
  ```

## セットアップ

### 方法 1: このリポジトリの `.mcp.json` を使う（プロジェクトスコープ）

このリポジトリを clone して、そのディレクトリで Claude Code を起動するだけです。

```sh
git clone https://github.com/goodnasubi/claude-cloude-colab-mcp-test.git
cd claude-cloude-colab-mcp-test
claude
```

初回起動時にプロジェクトの MCP サーバー `colab-mcp` を承認するか聞かれるので、承認してください。
`/mcp` で `colab-mcp` が `connected` になっていれば OK です。

### 方法 2: CLI でユーザースコープに追加する（どのディレクトリからでも使う）

```sh
claude mcp add --scope user colab-mcp -- uvx git+https://github.com/googlecolab/colab-mcp
```

> Google 社内など、デフォルトのパッケージインデックスが PyPI でない場合は
> `uvx --index https://pypi.org/simple git+https://github.com/googlecolab/colab-mcp` のように `--index` を追加してください。

## 使い方

1. ブラウザで [Google Colab](https://colab.research.google.com/) のノートブックを開く
2. Claude Code で「Colab に接続して」などと依頼する
3. Claude が `open_colab_browser_connection` ツールを呼ぶと Colab との接続が確立され、ノートブック編集・実行用のツールが追加で有効になります

## 動作確認

サーバー単体の起動確認:

```sh
uvx --from git+https://github.com/googlecolab/colab-mcp colab-mcp --help
```

ログはデフォルトで `/tmp/colab-mcp-logs/` に出力されます（`--log <dir>` で変更可）。
