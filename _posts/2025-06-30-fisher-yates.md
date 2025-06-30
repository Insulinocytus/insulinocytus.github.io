---
title: シャッフルアルゴリズム「Fisher-Yates」
date: 2025-06-30 17:32:00 +0900
categories: [アルゴリズム]
tags: [ランダム,シャッフル]
pin: false
mermaid: true
---

例えば、カードゲームのデッキを実装する。
デッキからカードをランダムに抽出するためには、
事前にシャッフルする必要がある。

## 私ならこう書く

```csharp
static IList<T> MyShuffle<T>(Random rng, IList<T> dataList)
{
    return dataList.OrderBy(_ => rng.Next()).ToList();
}
```

正直以上のコードで全然足りてる。1行で完結、わかりやすい、カードゲームのデッキぐらいなら余裕だ。
唯一の欠陥は、空間複雑度が`O(n)`になっていること。`rng.Next()`の結果を保存する必要があるから。

## Fisher-Yates

```csharp
static IList<T> MyShuffle<T>(Random rng, IList<T> dataList)
{
    static IList<T> FisherYates<T>(Random rng, IList<T> dataList)
    {
        for (var i = dataList.Count - 1; i > 0; i--)
        {
            var j = rng.Next(i);
            if (i != j)
            {
                (dataList[i], dataList[j]) = (dataList[j], dataList[i]);
            }
        }
        return dataList;
    }
}
```

Listの最後尾から、Indexが指している要素をより小さいIndexの要素と位置交換する、或いは交換しない。
（リアルのシャッフルでも、シャッフル後にカードの位置が変わらなかった可能性があるから）

空間複雑度は`O(1)`、優雅。

## 確率分布テスト

```csharp
var rng = new Random();
var testData = new List<int> { 1, 2, 3, 4, 5 };

List<int[]> myShuffleResult = new();

for (var i = 0; i < 100000; i++)
{
    var shuffled = MyShuffle(rng, testData);
    myShuffleResult.Add(shuffled.ToArray());
}

List<int[]> fisherYatesResult = new();

for (var i = 0; i < 100000; i++)
{
    var shuffled = FisherYates(rng, testData);
    fisherYatesResult.Add(shuffled.ToArray());
}

Console.WriteLine("My Shuffle Result");
PrintPercentage(myShuffleResult);
Console.WriteLine();
Console.WriteLine("Fisher-Yates Result");
PrintPercentage(fisherYatesResult);

static void PrintPercentage(List<int[]> result)
{
    for (var i = 0; i < 5; i++)
    {
        var dataWithIndex = result.Select(shuffled => shuffled[i]).ToArray();
        var percentage1 = dataWithIndex.Count(x => x == 1) / (float)dataWithIndex.Length;
        var percentage2 = dataWithIndex.Count(x => x == 2) / (float)dataWithIndex.Length;
        var percentage3 = dataWithIndex.Count(x => x == 3) / (float)dataWithIndex.Length;
        var percentage4 = dataWithIndex.Count(x => x == 4) / (float)dataWithIndex.Length;
        var percentage5 = dataWithIndex.Count(x => x == 5) / (float)dataWithIndex.Length;
        Console.WriteLine(
            $"Index {i} Percentage:\n1={percentage1}\n2={percentage2}\n3={percentage3}\n4={percentage4}\n5={percentage5}"
        );
    }
}

static IList<T> MyShuffle<T>(Random rng, IList<T> dataList)
{
    return dataList.OrderBy(_ => rng.Next()).ToList();
}

static IList<T> FisherYates<T>(Random rng, IList<T> dataList)
{
    for (var i = dataList.Count - 1; i > 0; i--)
    {
        var j = rng.Next(i);
        if (i != j)
        {
            (dataList[i], dataList[j]) = (dataList[j], dataList[i]);
        }
    }
    return dataList;
}
```

出力

```txt
My Shuffle Result
Index 0 Percentage:
1=0.20083
2=0.20148
3=0.19647
4=0.19926
5=0.20196
Index 1 Percentage:
1=0.20009
2=0.19939
3=0.20069
4=0.19993
5=0.1999
Index 2 Percentage:
1=0.19929
2=0.2005
3=0.20076
4=0.19986
5=0.19959
Index 3 Percentage:
1=0.19929
2=0.19898
3=0.20094
4=0.2011
5=0.19969
Index 4 Percentage:
1=0.2005
2=0.19965
3=0.20114
4=0.19985
5=0.19886

Fisher-Yates Result
Index 0 Percentage:
1=0.19986
2=0.20103
3=0.20058
4=0.20031
5=0.19822
Index 1 Percentage:
1=0.19932
2=0.19905
3=0.20005
4=0.19995
5=0.20163
Index 2 Percentage:
1=0.19915
2=0.20071
3=0.20053
4=0.19992
5=0.19969
Index 3 Percentage:
1=0.20216
2=0.19867
3=0.19963
4=0.19978
5=0.19976
Index 4 Percentage:
1=0.19951
2=0.20054
3=0.19921
4=0.20004
5=0.2007
```
