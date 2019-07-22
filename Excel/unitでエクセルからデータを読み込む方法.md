<link href="./mdcss.css" rel="stylesheet"></link>
# unity-excel-importerの使い方

#### Unity でエクセルファイルをそのまま読み込む方法をネットで調べて、自分用にまとめました。

unity-excel-importer を使用して取り込みます。

*[unity-excel-importer](https://github.com/mikito/unity-excel-importer)はこちらからダウンロードします。

---

|作業順|内容|
|---|---|
|1. |エクセルを作成します。|
|2. |シート名はエクセルファイル名と違う名前にします（必ず設定してくださいこの名前がそのまま変数名になります）。|
|3. |１行目に半角英数で、スペースを入れないようにして名前を定義（ラベル・この値は ScriptableObjectのメンバーにーなります）。|
|4. |各定義の下に必要なデータを入れます。|
|1. |エクセルの最初の行に「#」の入っている行はコメントとして扱われて取り込み対象にはなりません。|
|1. |保存後、エクセルファイルを Unity にドラッグ＆ドロップしてファイルを選択して右クリックします。|
|1. |「Create」→「ExcelAssetScript」を選択します。保存先フォルダを選択して保存します（管理ファイル・このファイルを修正します[^1]）。|
|1. |ScriptableObject で管理するデータを保存するデータを記入するファイルを用意する（データリストテーブル[^3]）。|
|1. |データリストテーブルには通常の型のほか enum も使用できる。|
|1. |管理ファイルを修正する。<br>public List<データリストテーブル> エクセルシート名;|
|1. |一通りファイルを生成したら、エクセルを右クリックして Reimport を選択すると ScriptableObject が生成される。|
|1. |呼び出しは次の通り。[^2]<br>ScriptableObjectファイル名 変数;<br>変数.エクセルシート名[選択番号].メンバー|

---

## 例）excel ファイル名が sample.xlsx の場合<br>

シート名は sampleEntitis;<br>
データは id(int), name(string)とします。

[^3]<details><summary>定義ファイル</summary>
<div>

```
[SampleEntity.cs（定義ファイル）]

using System;

[Serializable]
public class SampleEntity
{
    public int id;
    public string name;
}
```

</div>
</details>

[^1]<details><summary>管理ファイル</summary>
<div>

```
[sample.cs(生成されるファイル)]

using System.Collections.Generic;
using UnityEngine;

[ExcelAsset]
public class sample : ScriptableObject
{
	//public List<EntityType> Sheet1; // Replace 'EntityType' to an actual type that is serializable.
	public List<SampleEntity> SampleEntities;
}
```

</div>
</details>

[^2]<details><summary>呼び出し方法</summary>
<div>

```
[gameControlなどの呼び出すファイル]

public class gameControl : Monobehavior
{
	[SeritalizeField]
	private sample _sampleObj;

	void Start()
	{
		//	id:name のリストをコンソールに表示
		_sampleObj.ForEach(x => Debug.Lod(x.id + ":" + x.name));
	}
}

```
</div>
</details>
