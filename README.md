# <span>lodash.jsを</span><span>本当に使いこなす</span>

## lodash.jsとは

> A modern JavaScript utility library delivering modularity, performance & extras.

- ユーティリティー（なんか便利）関数を集めたやつ
- めっちゃかるい
- 多少なりデータを取り回す案件では、 **個人的には必須のライブラリ**

## 主な利用ケース

- 配列やオブジェクトをほしい形に整形する
- イデオム系の実装を借りてくる
- テンプレートエンジン

## <span>配列やオブジェクトを</span><span>ほしい形に整形する</span>

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
const ids = _.map(charaArray, (chara) => {
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
const idCharaName = _.mapValues(charaData, (chara) => {
    return chara.name;
});
// => { 25: 'ピカチュウ': 35: 'ピッピ': 39: 'プリン' }
```

- オブジェクトの値をひとつずつループして、欲しい値をつくってreturn

- mapとの違いは配列ではなく、**オブジェクトを受け取ってオブジェクトを返す**こと

<hr>

- 配列を渡すと暗黙的にオブジェクトに変換してくるので注意

```
const ids = _.mapValues(charaArray, (chara) => {
    return chara.id;
});
// => { 0: 25, 1: 35, 2: 39 }
```

### mapKeys

```
const idCharaData = _.mapKeys(charaArray, (chara) => {
    return chara.id;
});
```

- オブジェクトのvalueをひとつずつループして、欲しい **keyを**つくってreturn
- こちらはむしろ、暗黙的にオブジェクトに変換してくるのを上手く利用できるケース

### filter
### pickBy
### invertBy / groupBy
### shuffle / sample
### sortBy / orderBy

## wrapper記法

- wrapper記法を使うことで、複数のlodash関数を **メソッドチェーンで繋ぐことができる**
- ここまで使いこなすと、複雑なデータ変換もかなりスマートに書けるようになる

## イデオム系の実装を借りてくる

- padStart
- throttle / debounce

## テンプレートエンジン

:sugoi:

## 注意点

- array用とobject用が別関数になっていたりするので注意

- 破壊的な変更をするものもあるので注意
  - wrapperでつなぐと気にならないパターンもある

