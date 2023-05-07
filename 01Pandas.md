今回は以下の内容を説明していきます


---

#### Pandasのインポート

まず、使うにはインポートします
 
```python
import pandas as pd
```
### 最初の行だけ表示
```python
#指定しないと5行
print(df.head())
#3行のみ
print(df.head(3))
display(df.head())  #displayをつけると綺麗に表示できる
df.tail() #末尾
```
csvの取り込み
```python
df = pd.read_csv('test.csv')
#日本語のファイル名の場合
data = pd.read_csv('test.csv', engine='python', encoding='utf-8')
```
csvの保存
```python
df.to_csv('test1.csv')
#indexなし
df.to_csv('test1.csv',index=False)
```
読み込んだdfの確認
行数、列数、欠損値、タイプの確認
```python
df.info()
```

excelの取り込み
```python
df = pd.read_excel("test.xlsx")
```
欠損値の数を数える
```python
df.isnull().sum()  #列方向
df2.isna().sum(axis=1) #行方向
```
列ごとにTrue/Falseを返して、Trueの数を合計する

欠損値の削除
```python
#欠損値を含む行の削除
df_drop= df.dropna()
#欠損値を含む列の削除
df_drop= df.dropna(axis=1)
```
欠損値を埋める
```python
df.fillna(df.mean())  #列平均でうめる
```


列/行の削除
```python
#列名testの削除
df_drop = df.drop("test",axis = 1)
df = df.drop(columns='test')
df = df.drop(index=101)
```
pandasのDataFrameからnumpy配列に変換
```python
arr = df.values
```

列を１つだけ取り出す
```python
df['test']
```
列を2つ取り出す
```python
df[['test','test2']]
```

条件指定による要素の選択
```python
mask = df['test']==0
df[mask]

#test列の中から1234を探して表示
mask = df['test'].str.contains('1234')
df[mask]
```

行列の指定による要素の選択
```python
df.loc['test']
df.test
#列も選択
df.loc[:10,['test']]　#注意10以下を選択
df.iloc[:10,[1,3]]  #注意10未満を選択

```


行の追加
```python
df = pd.DataFrame([[1,2,3]], columns=["test1", "test2", "test3"])
df2 = df2.append(df, ignore_index=True, sort=False)
#エラーが出るとき、順番が変わるときsortを追加
#つけてもpandasのバージョンによってはエラーが出る
#ignore_indexで行番号を付け直し
```



データの結合(縦方向)
```python
df_concat = pd.concat([df_1,df_2],ignore_index=True)
df_concat = pd.concat([df_1,df_2],sort=False) #エラーが出るとき、順番が変わるときsortを追加
#つけてもpandasのバージョンによってはエラーが出る

```
ignore_index=Trueでは、縦方向の連結のときにindexを振りなおす

データの結合(横方向)
```python
#column1をキーとして、leftのデータフレームに結合
join_data = pd.merge(df_1,transaction[["column1",'column2,"column3"]],on="column1",how="left")
#leftのキー、rightのキーそれぞれでjoin
join_data = pd.merge(df_1,transaction,left_on="test2",right_on="test1",how="left")
```

データ列のタイプを変更
結合でタイプエラーのとき行う
```python
#'test'列のタイプを調べる
df['test'].dtype
#astypeを使ってタイプを変える
df['test']=df['test'].astype(int)
```

個数、平均、最大最小などの統計量を計算
```python
df.describe()
df.corr() #相関
```
最大,最小を計算
```python
pd["test"].max()
pd["test"].min()
```
日付に変更
```python
pd.to_datetime(df["test"])
pd["test"].dt.strftime("%Y%m")  #年と月のみ
```
グループ化
```python
df.groupby("test1").sum()["test2"]
df.groupby(["test1","test2"]).sum()[["test3","test4"]]
```
ピボットテーブルを使って並べかえ
```python
pd.pivot_table(df,index='test1',columns="test2", values=["test3","test4"],aggfunc="sum")
```
ピボットテーブルをグラフ化
```python
for num in df.columns:
    plt.plot(df.index,df[num],label=num)
plt.legend()
```
データの編集
```python
df["test"]=df["test"].str.upper()  #小文字を大文字に変換
df["test"]=df["test"].str.replace("a","b")  #aをbに変換
df = df.replace({"a":1,"b":0})
```
データをダミー変数化
```python
df=pd.get_dummies(df['test'])
```

データの並べ替え
```python
df.sort_values(by=["test"],ascending=True) 
df.sort_values("test",ascending=False)  #大きい順
df.sort_values(["test","test2"],ascending=[False,True])  #testを大きい順、Test2を小さい順 

```
ユニークなデータ数を数える
```python
df.unique(uriage_data["test"])
len(df.unique(uriage_data["test"]))

```
辞書からDataFrame
DataFrameから辞書
```python
test = {0:[234,345],1:[245,432]}
dftest = pd.DataFrame(test)
test = dftest.to_dict()
```
pickleを使ってpandas形式で保存
```python
#picleを保存
df.to_pickle('test.pkl')
#picleを読み込み
df= pd.read_pickle('test.pkl')
```

列名をリネーム
```python
#inplace=Trueにすると元のデータフレームが変更される
df.rename(columns={"test":"test2"},inplace=True)
```

列を削除
```python
del df["test"]
df = df.drop(columns='test')
```
indexをリセット
```python
#drop=Falseでindexを残す
df=df.reset_index(drop=False)
```

条件式入れてfalseのとき変更
```python
df["test"] = df["test"].where(df["test2"]<10,5)
```
累積和
```python
df["test"].cumsum()
```

ランダムにサンプル
```python
df.sample(3) #3つをサンプルリング
```

列から数文字抽出
```python
df['test'].str[:3] #3つをサンプルリング
```
