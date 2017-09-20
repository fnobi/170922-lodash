# <span>細かすぎるけど</span><span>伝わって欲しい</span><span>lodash.jsの話</span>

## lodash.jsとは

[https://lodash.com/](https://lodash.com/)

> A modern JavaScript utility library delivering modularity, performance & extras.

- ユーティリティー（なんか便利）関数を集めたやつ
- めっちゃかるい
- 多少なりデータを取り回す案件では、 **個人的には必須のライブラリ**


## 主な利用ケース

- 配列やオブジェクトをほしい形に整形する
- イデオム系の実装を借りてくる
- テンプレートエンジン


## <span>配列やオブジェクトを</span><span>ほしい形に整形する</span>

※痒いところに手が届く便利メソッドが超いっぱいあるぞという話と、<br />似てるけど微妙に違うメソッドが超いっぱいあるぞという話をします。

### each

```
_.each(bigArray, (item) => {
    console.log(item);
});
```

- `_.xxx()` のかたちで呼び出す
- さすがに中身は説明しません

### map

```
const charaArray = [{
    id: 25,
    name: 'ピカチュウ',
    type: [ 'でんき' ]
},{
    id: 35,
    name: 'ピッピ',
    type: [ 'ノーマル' ]
},{
    id: 39,
    name: 'プリン',
    type: [ 'ノーマル' ]
}];
```

```
_.map(charaArray, (chara) => {
    return chara.id;
});
// => [ 25, 35, 39 ]
```

- 整形の基本。配列に入ったオブジェクトを一つずつループして、欲しい値だけ取り出してreturn

### mapValues

```
const charaData = {
    25: {
        name: 'ピカチュウ', type: [ 'でんき' ]
    },
    35: {
        name: 'ピッピ', type: [ 'ノーマル' ]
    },
    39: {
        name: 'プリン', type: [ 'ノーマル' ]
    }
};
```

```
_.mapValues(charaData, (chara) => {
    return chara.name;
});
// => { 25: 'ピカチュウ': 35: 'ピッピ': 39: 'プリン' }
```

- オブジェクトの値をひとつずつループして、欲しい値をつくってreturn

- mapとの違いは配列ではなく、**オブジェクトを受け取ってオブジェクトを返す**こと

- 配列を渡すと暗黙的にオブジェクトに変換してくるので注意

```
// オブジェクトを渡してオブジェクトを返す
_.mapValues(charaData, (chara) => {
    return chara.name;
});
// => { 25: 'ピカチュウ': 35: 'ピッピ': 39: 'プリン' }

// 配列を渡されたらオブジェクトとして解釈
_.mapValues(charaArray, (chara) => {
    return chara.name;
});
// => { 0: 'ピカチュウ', 1: 'ピッピ', 2: 'プリン' }
```

```
// 配列を渡して配列を返す
_.map(charaArray, (chara) => {
    return chara.id;
});
// => [ 'ピカチュウ', 'ピッピ', 'プリン' ]

// オブジェクトを渡されたら配列として解釈
_.map(charaData, (chara) => {
    return chara.name;
});
// => [ 'ピカチュウ', 'ピッピ', 'プリン' ]
```

### mapKeys

```
_.mapKeys(charaArray, (chara) => {
    return chara.id;
});
// => { 25: { id: 25, name: 'ピカチュウ', type: [ 'でんき' ] },
35: { id: 35, name: 'ピッピ', type: [ 'ノーマル' ] },
39: { id: 39: name: 'プリン', type: [ 'ノーマル' ] } }
```

- オブジェクトのvalueをひとつずつループして、欲しい **keyを**つくってreturn
- 上記はむしろ、暗黙的にオブジェクトに変換してくるのを上手く利用できるケース

### groupBy / invertBy

- xxxをキーにしたオブジェクトをつくりたいシリーズ
- このへんまで来るとかなりマニアック

```
_.mapKeys(charaArray, 'id');
// => { '25': { id: 25, name: 'ピカチュウ', type: [ 'でんき' ] },
'35': { id: 35, name: 'ピッピ', type: [ 'ノーマル' ] },
'39': { id: 39, name: 'プリン', type: [ 'ノーマル' ] } }

// valueに入るのは元と同じオブジェクト（もしキーが被っていたら片方しか残らない）
```

```
_.groupBy(charaArray, 'id');
// => { '25': [ { id: 25, name: 'ピカチュウ', type: [Array] } ],
'35': [ { id: 35, name: 'ピッピ', type: [Array] } ],
'39': [ { id: 39, name: 'プリン', type: [Array] } ] }
）

// valueに入るのは元と同じオブジェクトの配列（キーが被っているものをグルーピング
```

```
_.invertBy(charaArray, 'id');
// => { '25': [ '0' ], '35': [ '1' ], '39': [ '2' ] }

// valueに入るのは元のキーにあたるもの
```

### （休憩）

マニアックなやつ、覚えてれば救われることはあるけど、調べるよりは手書きした方が速い時もあるぞ！！

## 絞込系

### filter

- 配列を、条件に合う（第２引数で渡した関数がtrueを返す）ものだけに絞り込んで返す

- ついでで例示しちゃったけど `_.includes` は第１引数の配列に第２引数の要素が含まれてるかを返します 絞込系と相性よし

```
_.filter(charaArray, (item) => {
    return _.includes(item.type, 'でんき');
});
// => [ { id: 25, name: 'ピカチュウ', type: [ 'でんき' ] } ]
```

### find

- 条件に合った **最初の要素** を返す

```
_.find(charaArray, (item) => {
    return _.includes(item.type, 'ノーマル');
});
// => { id: 35, name: 'ピッピ', type: [ 'ノーマル' ] }
```

### pickBy

- `_.filter` のオブジェクト用
- map / mapValues のときと似たような型変換も起きる

```
_.pickBy(charaData, (item) => {
    return _.includes(item.type, 'でんき');
});
// => { 25: { name: 'ピカチュウ', type: [ 'でんき' ] } }
```

### compact

- 配列からfalsyな要素を削って返す

```
_.compact([ 0, 1, 2, '', 'a', 'ab', null, undefined ]);
// => [ 1, 2, 'a', 'ab' ]
```

```
// 複数行テキストから空行を除く例
_.compact(longText.split(/\n/g));
```

## 集合系

### union / intersection / difference

- それぞれ和集合・積集合・差集合

```
_.union([2], [1, 2]);
// => [2, 1]

// 集合として扱うので重複は弾かれている
```

```
_.intersection([2, 1], [2, 3]);
// => [2]

// 両方に含まれる要素の配列を返す
```

```
_.difference([2, 1], [2, 3]);
// => [1]

// 前者にだけ含まれる要素の配列を返す
```

## 並び替え系

### sortBy / orderBy

- 関数あるいはプロパティ名で並び替え
- orderByの方が、昇順降順が選べてすごい
- **破壊的**（元の配列の順番を変えてしまう）なので注意


```
// 名前昇順
_.sortBy(members, 'name'); // => membersが書き換わる

// 名前の長さ昇順
_.sortBy(members, (item) => {
    return item.name.length;
});

// 名前・作成日昇順（名前のほうが優先）
_.sortBy(members, [ 'name', 'created_at' ]);
```

```
// 名前降順
_.orderBy(members, 'name', 'desc'); // => membersが書き換わる

// 名前の長さ昇順
_.orderBy(members, (item) => {
    return item.name.length;
}, 'desc');

// 名前昇順・作成日降順（名前のほうが優先）
_.orderBy(members, [ 'name', 'created_at' ], [ 'asc', 'desc' ]);
```

### shuffle / sample

- shuffleは見ての通り
  - **破壊的**（元の配列の順番を変えてしまう）なので注意
- sampleは適当に１個取ってきてくれる

```
// メンバーをランダムに並び替える
_.shuffle(members); // => membersが書き換わる

// メンバーからランダムに１人選ぶ
_.sample(members);
```

## wrapper記法

- wrapper記法を使うことで、複数のlodash関数を **メソッドチェーンで繋ぐことができる** (第１引数が配列/オブジェクトなもの)
- ここまで使いこなすと、複雑なデータ変換もかなりスマートに書けるようになる

```
// キャラの名前一覧をシャッフルして取得
_(charaArray).map((chara) => {
    return chara.name;
}).shuffle().value();

// 最終的な配列をとるために .value() というメソッドを叩く必要がある
```

```
// メンバーのスコアを合計して、スコア上位５名を割り出した上で、
// idをキーにしたオブジェクトにして返す
_(members)
    .map((member) => {
        member.scoreSum = _.sum(member.scoreList);
        return member;
    })
    .orderBy('scoreSum', 'desc')
    .slice(0, 5)
    .mapKeys((member) => {
        return member.id;
    })
    .value();
```

## イデオム系の実装を借りてくる

- padStart
- throttle / debounce

## テンプレートエンジン

:sugoi:

## 注意点

- array用とobject用が別関数になっていたりするので注意

- 破壊的な変更をするものもあるので注意
  - wrapperでつなぐと気にならないパターンもある

