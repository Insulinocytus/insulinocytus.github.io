---
title: シャッフルアルゴリズム「Fisher-Yates」
date: 2025-06-30 17:32:00 +0900
categories: [アルゴリズム]
tags: [ランダム,シャッフル]
pin: false
mermaid: true
---

例えば、カードゲームのデッキを実装する場合を考えてみよう。  
デッキからカードをランダムに抽出するためには、  
事前にシャッフルする必要がある。  

## 私ならこう書く

```csharp
static T[] MyShuffle<T>(Random rng, IEnumerable<T> dataList)
{
    return dataList.OrderBy(_ => rng.Next()).ToArray();
}
```

正直、以上のコードで十分である。1行で完結し、わかりやすく、カードゲームのデッキ程度なら問題ない。  
唯一の欠陥は、空間計算量が`O(n)`になっていること。`rng.Next()`の結果を保存する必要があるから。  

## Fisher-Yates

```csharp
static T[] FisherYates<T>(Random rng, IEnumerable<T> dataList)
{
    var result = dataList.ToArray();
    for (var i = result.Length - 1; i > 0; i--)
    {
        var j = rng.Next(i + 1);
        (result[i], result[j]) = (result[j], result[i]);
    }
    return result;
}
```

配列の最後尾から、現在のインデックスが指している要素をより小さいインデックスの要素と位置交換する、または交換しない。  
（実際のシャッフルでも、シャッフル後にカードの位置が変わらない可能性があるため）  
  
空間計算量は`O(1)`、優雅。  

## 確率分布テスト

```csharp
var rng = new Random();

var testData = new[] { 1, 2, 3, 4, 5 };
List<int[]> myShuffleResult = new();

for (var i = 0; i < 1000000; i++)
{
    var result = MyShuffle(rng, testData);
    myShuffleResult.Add(result);
}

List<int[]> fisherYatesResult = new();

for (var i = 0; i < 1000000; i++)
{
    var result = FisherYates(rng, testData);
    fisherYatesResult.Add(result);
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

static T[] MyShuffle<T>(Random rng, IEnumerable<T> dataList)
{
    return dataList.OrderBy(_ => rng.Next()).ToArray();
}

static T[] FisherYates<T>(Random rng, IEnumerable<T> dataList)
{
    var result = dataList.ToArray();
    for (var i = result.Length - 1; i > 0; i--)
    {
        var j = rng.Next(i + 1);
        (result[i], result[j]) = (result[j], result[i]);
    }
    return result;
}
```

```console
My Shuffle Result
Index 0 Percentage:
1=0.199332
2=0.199736
3=0.200812
4=0.199802
5=0.200318
Index 1 Percentage:
1=0.200738
2=0.200232
3=0.199358
4=0.199915
5=0.199757
Index 2 Percentage:
1=0.200108
2=0.199867
3=0.200504
4=0.199291
5=0.20023
Index 3 Percentage:
1=0.199323
2=0.199954
3=0.199346
4=0.200855
5=0.200522
Index 4 Percentage:
1=0.200499
2=0.200211
3=0.19998
4=0.200137
5=0.199173

Fisher-Yates Result
Index 0 Percentage:
1=0.200182
2=0.200447
3=0.200087
4=0.199704
5=0.19958
Index 1 Percentage:
1=0.199758
2=0.199465
3=0.200224
4=0.200461
5=0.200092
Index 2 Percentage:
1=0.199969
2=0.199617
3=0.200217
4=0.200174
5=0.200023
Index 3 Percentage:
1=0.199866
2=0.200018
3=0.199653
4=0.199981
5=0.200482
Index 4 Percentage:
1=0.200225
2=0.200453
3=0.199819
4=0.19968
5=0.199823
```
