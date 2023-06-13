# practice-typescript

TypeScript の練習．以下メモは上から下に実行した順となる．

## 参考書籍

- [プロを目指す人のためのTypeScript入門 安全なコードの書き方から高度な型の使い方まで](https://gihyo.jp/book/2022/978-4-297-12747-3)

## 環境準備

node.js をインストール（方法は調べる）

```bash
node -v
```

でバージョン確認する．

複数の node.js のバージョンをインストールしたい場合もあるので，nvm という node.js のバージョン管理ソフトを入れる．

```bash
sudo apt install curl
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile
```

入れたら，

```bash
nvm install v16.8.0
```

のように特定の node.js のバージョンをインストールできる．

## 環境構築（TypeScript のインストール）

作業フォルダの作成を行ったら，

```bash
npm install --yes
```

とすると，```package.json``` が生成される．

これは node.js のプロジェクトには必ず存在するファイル．主な機能はプロジェクトの依存関係の記録，プロジェクトの設定の記録．

上記コマンドで ```--yes``` と書いたが，普通は初期化時にいくつか質問がされるが，今回はパッケージを公開するわけじゃないので，全て yes でいい場合は上記のように書く．

デフォルトの設定から ```package.json``` を編集して，例えば "main": "index.js" の下に，

```json
"main": "index.js",
"type": "module",
```

と記載する．これは，プロジェクト内の .js ファイルをスクリプトとして解釈するか，モジュールとして解釈するかの違い．

### TypeScript の Install

```bash
npm install --save-dev typescript@4.6.2 @types/node@14.14.10
```

2つのパッケージをインストールする．```--save-dev``` オプションはインストールされるパッケージが devDependencies (プログラムの実行ではなくビルドなどその他開発時に必要であること)を示す．

インストールすると以下のフォルダ構成になる

```bash
.
├── LICENSE
├── node_modules
├── package.json
├── package-lock.json
└── README.md
```

npm でインストールされたものは ```node_modules``` に入る．

また，```package-lock.json``` は現在インストールされているパッケージを記載したもので，npm が自動管理する．

***```node-modules``` は .gitignore に書くことを推奨．***

### tsconfig.json の準備

```tsconfig.json``` は TypeScript コンパイラに対する設定を記載したファイル．TypeScript コンパイラは様々なオプションがあるが，引数で指定するか json ファイルで指定するか選べる．json が便利なので，```tsconfig.json``` を書くことを推奨．

```tsconfig.json``` は以下でつくれる

```bash
npx tsc --init
```

```npx``` は ```npm``` に付属するプログラムで，```node_modules``` 内にインストールされたコマンドラインプログラムを実行してくれるツール．```tsc``` はよく使うコマンドだが，```node_modules``` 内にあるので，```npx``` を使うことになる．

```tsc --init``` は TypeScript コンパイラの初期化（```tsconfig.json``` の作成）・

```tsconfig.json``` はいろんな設定が書いてあるが，デフォルトではオフ（コメントアウト）してある．必要に応じて書き換えやコメントアウトを削除する必要がある．

参考書籍に合わせて以下を行う

- target コンパイラオプションを変更

```json
"target": "es2016"
↓
"target": "es2020"
```

これでトランスパイルの程度を指定できる（コンパイル後の JavaScript のバージョン的な意味）．

- module コンパイラオプションを変更

```json
"module": "commonjs"
↓
"module": "esnext"
```

モジュールに関連する構文の扱いを指定．新しい node.js では新しい構文を解釈できるらいしので変更する．

- moduleResolution コンパイラオプションを node にする

```json
// "moduleResolution": "node"
↓
"moduleResolution": "node"
```

これは ```npm``` がインストールしたモジュールを TypeScript が認識できるようにするオプション．

- outDir コンパイラオプションを設定

```json
// "outDir": "./",
↓
"outDir": "./dist",
```

コンパイル結果の出力先の指定．

- include オプションの設定

```json
{
    "compilerOptions": {
        // some compiler options
    },
    // add below
    "include": ["./src/**/*.ts"]
}
```

これで，```src``` ディレクトリ以下すべての .ts ファイルがコンパイル対象となる．

## Hello, World

```src``` ディレクトリを作成し，以下のコード ```index.ts``` を作成する．

```typescript
// src/index.ts
const message: string = "Hello, World!";
console.log(message)
```

プロジェクトのディレクトリで以下のコマンドを実行．

```bash
npx tsc
```

すると，```dist``` フォルダ以下に ```index.js``` が作られる．

```bash
.
├── dist
│   └── index.js
├── LICENSE
├── node_modules
│   ├── @types
│   └── typescript
├── package.json
├── package-lock.json
├── README.md
├── src
│   └── index.ts
└── tsconfig.json
```

```index.js``` を ```node``` で実行すると，以下のような出力が得られる．

```bash
node dist/index.js
Hello, World!
```

```index.ts``` の中身を以下のように変えるとコンパイルエラーとなる．

```typescript
// src/index.ts
const message: number = "Hello, World!";
console.log(message)
```

```bash
src/index.ts:1:7 - error TS2322: Type 'string' is not assignable to type 'number'.

1 const message: number = "Hello, World!";
        ~~~~~~~


Found 1 error in src/index.ts:1
```

以下は書籍を写経したり，ドットインストールの内容を試して勉強する．
