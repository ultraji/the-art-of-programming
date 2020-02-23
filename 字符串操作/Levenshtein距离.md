# Levenshtein距离

- **关键字**：动态规划

## 问题描述

Levenshtein 距离，又称编辑距离，指的是两个字符串之间，由一个转换成另一个所需的最少编辑操作次数。许可的编辑操作包括将一个字符替换成另一个字符，插入一个字符，删除一个字符。编辑距离的算法是首先由俄国科学家Levenshtein提出的，故又叫Levenshtein Distance。  
Ex：  
字符串A: abcdefg  
字符串B: abcdef  
通过增加或是删掉字符”g”的方式达到目的。这两种方案都需要一次操作。把这个操作所需要的次数定义为两个字符串的距离。  
要求：  
给定任意两个字符串，写出一个算法计算它们的编辑距离。

## 算法思路

1. lev[i][j]用来表示字符串a的[1...i]和字符串b[1...j]的levenshtein距离；
2. 插入和删除操作互为逆过程：a删除指定字符变b等同于b插入指定字符变a； 
3. 如果a[i] == b[j]，则说明a[i]和b[j]分别加入a，b之后不会影响levenshtein距离，lev[i][j] = lev[i-1][j-1] + 0;
4. 如果a[i] != b[j]，则需要考虑3种情况的可能：
    1. a中插入字符，即lev[i][j] = lev[i-1][j] + 1;
    2. b中插入字符，即lev[i][j] = lev[i][j-1] + 1;
    3. a[i]替换成b[j]，lev[i][j] = lev[i-1][j-1] + 1;
5. 取这4种情况的最小值。

## 代码实现

```c++
int findMin(int a, int b, int c)
{
    a = min(a, b);
    b = min(b, c);
    return min(a, b);
}

int levenshtein(string a, string b)
{
    a.insert(0, 1,' ');
    b.insert(0, 1, ' ');
    int n = a.size(), m = b.size();
    int cost, lev[n][m];
    for(int i = 0; i < n; i++) lev[i][0] = i;
    for(int j = 0; j < m; j++) lev[0][j] = j;
    for(int i = 1; i < n; i++)
    {
        for(int j = 1; j < m; j++)
        {
            if(a[i] == b[j]) cost = 0;
            else cost = 1;
            lev[i][j] = findMin(lev[i][j-1]+1, 
                lev[i-1][j]+1, lev[i-1][j-1]+cost);
        }
    }
    return lev[n-1][m-1];
}

```