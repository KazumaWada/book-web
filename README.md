# book-web

customedomain
https://vercel.com/kazumawadas-projects/book-web/settings/domains
に飛んで、domainボタン->
Domain:ホステル日記.com
enviroment: production
redirectto: book-web-versel.com
ってやればいける。


```shell
npm run serve
```
- なぜnpm run serve コマンドなの?



---
.mdをAUckland Contentにloopさせる
```md
<h1> タグの下に、src/contents/Auckland 配下にある .md ファイルへのリンクを追加するためには、まずその .md ファイルを Vue コンポーネント内で参照できるようにする必要があります。その後、各 .md ファイルをリンクとして表示し、クリック時に適切な場所に遷移するようにします。

手順は次のようになります。

1. src/contents/Auckland 配下の .md ファイルのパスを取得する
Vue では、require.context を使って動的にファイルを読み込むことができます。この方法を使って、Auckland フォルダ内の .md ファイルを取得し、そのファイルのリンクを表示します。

2. .md ファイルを表示するリンクを作成
以下のように、require.context を使って .md ファイルを読み込み、そのファイルを <a> タグでリンクとして表示するように修正します。

修正後の AucklandContent.vue のコード:
vue
Copy
<template>
  <main class="mt-3">
    <h1>オークランド編(2023~)</h1>

    <ul>
      <!-- 動的に読み込んだMarkdownファイルをリンクとして表示 -->
      <li v-for="file in markdownFiles" :key="file.name">
        <a :href="file.path" target="_blank">{{ file.name }}</a>
      </li>
    </ul>
  </main>
</template>

<script>
export default {
  name: 'AucklandContent',
  data() {
    return {
      markdownFiles: []
    };
  },
  created() {
    // Aucklandフォルダ内のmdファイルを動的に読み込む
    const requireContext = require.context('@/contents/Auckland', false, /\.md$/);
    this.markdownFiles = requireContext.keys().map(fileName => {
      return {
        name: fileName.replace('./', ''),  // ファイル名を表示
        path: requireContext(fileName)     // リンクのパス
      };
    });
  }
}
</script>

<style scoped>
/* 必要に応じてスタイルを追加 */
</style>
説明:
require.context
require.context('@/contents/Auckland', false, /\.md$/) を使って、Auckland フォルダ内の全ての .md ファイルを動的にインポートします。この関数は、Auckland フォルダのファイルを相対パスで返すので、keys() メソッドでファイル名のリストを取得します。

markdownFiles 配列
markdownFiles 配列にファイル名とそのパスを格納し、テンプレート内で v-for を使って、各ファイルの名前とリンクを表示しています。

リンク
<a> タグの :href 属性に、インポートした .md ファイルのパスを渡して、クリックするとそのファイルに遷移できるようにしています。target="_blank" を追加して、リンクを新しいタブで開くようにしています。

これで、Auckland フォルダ内にある .md ファイルをリンクとして表示することができます。
```

↓そして、次はデザインを整えて、そのファイルに各mdをレンダリングする

```md
背景
あなたがやりたいことは、AucklandContent.vue というページに、src/contents/Auckland フォルダにある .md ファイル（Markdown ファイル）をリンクとして並べ、それらのファイルをクリックすると、デザインが整えられたページに内容が表示されるようにすることです。そのためには、以下の 3 つのステップで進めます。

ステップ 1: BaseDesign.vue コンポーネントの作成
まず、BaseDesign.vue というコンポーネントを作って、その中にデザインを整えるためのスタイルを入れます。そして、Markdown ファイルの内容は BaseDesign.vue で受け取って表示します。

ステップ 2: AucklandContent.vue で .md ファイルを読み込む
次に、AucklandContent.vue で、src/contents/Auckland フォルダ内にある Markdown ファイルを動的に読み込み、そのファイルをクリックした時に BaseDesign.vue に内容を表示させます。

ステップ 3: ファイルをクリックしたときにデザインを適用して表示する
Markdown ファイルをクリックした時に、その内容を BaseDesign.vue コンポーネントの中に表示し、デザインを整えて見せます。

実装の流れ
1. BaseDesign.vue を作成
BaseDesign.vue コンポーネントは、Markdown の内容を受け取り、それをきれいに表示するためのコンポーネントです。

vue
Copy
<!-- src/components/BaseDesign.vue -->
<template>
  <div class="base-design">
    <!-- Markdown の内容を表示するための div -->
    <div class="content" v-html="mdContent"></div>
  </div>
</template>

<script>
export default {
  name: 'BaseDesign',
  props: {
    mdContent: {
      type: String,
      required: true
    }
  }
}
</script>

<style scoped>
/* デザインを整えるための CSS */
.base-design {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.content {
  font-size: 16px;
  line-height: 1.6;
}
</style>
ポイント:

mdContent というプロパティを使って、外部から渡された Markdown の内容を表示します。
v-html="mdContent" で、Markdown の内容を HTML として表示します。
2. AucklandContent.vue で .md ファイルを読み込んで表示
次に、AucklandContent.vue で、src/contents/Auckland フォルダ内の .md ファイルを動的に読み込み、リンクとして表示します。そのリンクをクリックすると、選んだ .md ファイルの内容を BaseDesign.vue に渡して表示します。

vue
Copy
<template>
  <main class="mt-3">
    <h1>オークランド編(2023~)</h1>

    <!-- Markdown ファイルへのリンク -->
    <ul>
      <li v-for="file in markdownFiles" :key="file.name">
        <a @click.prevent="loadMarkdown(file.name)">{{ file.name }}</a>
      </li>
    </ul>

    <!-- BaseDesign に Markdown の内容を渡して表示 -->
    <BaseDesign v-if="mdContent" :mdContent="mdContent" />
  </main>
</template>

<script>
// BaseDesign.vue をインポート
import BaseDesign from '@/components/BaseDesign.vue';

export default {
  name: 'AucklandContent',
  components: {
    BaseDesign
  },
  data() {
    return {
      markdownFiles: [],  // ファイルのリスト
      mdContent: ''       // 表示する Markdown の内容
    };
  },
  created() {
    // Auckland フォルダ内の Markdown ファイルを動的に読み込む
    const requireContext = require.context('@/contents/Auckland', false, /\.md$/);
    this.markdownFiles = requireContext.keys().map(fileName => {
      return {
        name: fileName.replace('./', ''),
        path: requireContext(fileName) // ファイルのパス
      };
    });
  },
  methods: {
    loadMarkdown(fileName) {
      // クリックされたファイルを読み込んで、mdContent に内容をセット
      const file = this.markdownFiles.find(file => file.name === fileName);
      if (file) {
        this.mdContent = require('@/contents/Auckland/' + file.name);
      }
    }
  }
};
</script>

<style scoped>
/* 必要に応じてスタイルを追加 */
</style>
ポイント:

require.context を使って、src/contents/Auckland フォルダ内の .md ファイルを自動的にリストアップします。
リストに表示されたファイル名をクリックすると、そのファイルの内容が mdContent に格納され、BaseDesign コンポーネントに渡されます。
BaseDesign コンポーネントは、渡された mdContent を HTML として表示します。
3. 結果
AucklandContent.vue に表示されたリストのファイルをクリックすると、クリックされた .md ファイルの内容が BaseDesign.vue の中できれいに表示されます。
それぞれのファイルは、BaseDesign.vue で設定したデザイン（背景色、ボーダー、シャドウなど）で表示されます。
まとめ
BaseDesign.vue: Markdown の内容をデザイン通りに表示するためのコンポーネント。
AucklandContent.vue: .md ファイルのリンクを表示し、クリックされたファイルを BaseDesign.vue に渡して表示。
Markdown ファイル: src/contents/Auckland フォルダに保存された .md ファイルをクリックで表示。
これで、Markdown ファイルをクリックして、デザインを整えた状態で表示できるようになります！



```

↓こうやって、レンダリングする仕組みになっている

```md
簡単な流れまとめ
リンクをクリック → クリックされたリンクに対応する .md ファイルを読み込む。
ファイルの内容を取得 → その .md ファイルの内容を mdContent に格納。
BaseDesign.vue に表示 → mdContent を BaseDesign コンポーネントに渡し、デザインを適用して表示。
なぜ「リンク先は .md だけど、表示されるのは BaseDesign」？
リンク先（.md ファイル）は 内容 を提供するだけで、ページ遷移は起こりません。Markdown ファイルはただのデータ で、その内容を表示するのは Vue コンポーネント（この場合は BaseDesign.vue）です。リンクのクリックで「遷移する」のではなく、クリックされたファイルの内容が「読み込まれ」て、デザイン通りに表示される仕組みなのです。

これでイメージがつかめましたか？
```
つまり、ページが遷移(せんい)起きない！

↓
起こさせるためには、vue routerが必要になってくる。