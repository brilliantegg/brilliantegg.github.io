---
title: "Stable Matching Problem (Gale–Shapley Algorithm)"
date: 2023-04-12T00:05:21+08:00
draft: false
description: 
---

*此為觀看開放式課程後自行實作練習的紀錄*

## Propose and Reject (Gale–Shapley, Nobel Prize 2012)

假設今天有一樣數量的男女要進行戀愛配對，每個人都有對異性的喜好排序

Psuedocode:
```
設定每個人當前狀態為未配對
while (有單身男性且尚未追求過所有女性) {
	m = 當前男性
	w = 該男性尚未追求過且喜好排序中最高的女性
	if (w為單身) {
		(m, w) 配對
	} else if (w不是單身但比起原本對象m'更喜歡m) {
		(m, w) 配對
		m' 變回單身
	}
}
return 結果
```

## Proof

常用：反證法、數學歸納法

- Termination: worst case takes O(n^2 )  
	- 每次輪到的男性都會追求一位女性、可能的配對有n^2 對  
- Perfection: everyone finds their love interest  
	- 假設最後跑完還有一位單身男性m
	- 結束時，m已經追求過所有女性；不然無法離開while迴圈
	- 但是男女人數相同，所以一定有一個女性w也是單身；代表沒有人追求過該女性
	- 由此可反證該男m必定追求過所有女性
- Stability: matches are stable  
	- 假設目前得到的結果並不是stable matching，有一對(m1, w2)偏好對方更勝於目前的伴侶
	- 根據目前結果(m1, w1) & (m2, w2)可知最後一次的追求結果應該為m1追求w1
	- 討論以下兩種情況：
		1. 有追求過 w2 => 代表w2有更喜歡的對象更勝於m1，矛盾
		2. 沒有追求過 w2 => 代表其實m1更偏好w1，矛盾
- Uniqueness: NO!  
	可能找到不同的配對結果
- Male Optimality: best possible outcome for all men
	- 假設有一次執行結果S中得到的結果E中有一組配對中的男性沒有配對到可追求到的最喜歡的對象
	- 在過程中該男一定被valid partner拒絕過
		- 假設m是過程中第一個被valid partner拒絕的人，拒絕他的是他最喜歡的w
		- 根據定義，在某一次的stable matching結果 S' 中w應該會跟m在一起
	- 根據定義，存在一個 w 更喜歡的 m' 導致 m 被拒絕
		- 在 S' 中，m’ 跟 w' 在一起
	- 因為 m' 尚未被拒絕過，可推得 m' 比起 w' 應該更喜歡 w
	- 但是在 S' 中不包含 (m', w)，可認為與假設矛盾

## Proper Data Structure for Gale–Shapley Algorithm Implementation

Male's preference list: can use linked-list, O(1) find current most favor female  
Female's preference list: can use array, store rankings based on index(male's number), O(1) get certain male ranking

## Code Implementation

```csharp
public Dictionary<int, int> GaleShapley(int[,] malePreferenceInput, int[,] femalePreferenceInput) {
        int setNum = malePreferenceInput.GetLength(0);
        var stableSet = new Dictionary<int, int>();
        var malePreference = new LinkedList<int>[setNum];
        var femalePreference = new int[setNum][];

        for(int i=0; i<setNum; i++)
        {
            malePreference[i] = new LinkedList<int>();
            for(int w=0; w<setNum; w++)
            {
                malePreference[i].AddLast(new LinkedListNode<int>(malePreferenceInput[i,w]));
            }

            femalePreference[i] = new int[setNum];
            for(int j=0; j<setNum; j++)
            {
                femalePreference[i][femalePreferenceInput[i,j]] = i;
            }
        }

        var maleQueue = new Queue<int>();
        for(int i=0; i<setNum; i++)
        {
            maleQueue.Enqueue(i);
        }

        int currMale;
        int currFemale;
        var femalePartner = new int[setNum];
        Array.Fill(femalePartner, -1);
        while (maleQueue.Any()) {
            currMale = maleQueue.Peek();
            currFemale = malePreference[currMale].FirstOrDefault();


            if (femalePartner[currFemale] < 0)
            {
                maleQueue.Dequeue();
                stableSet.Add(currMale, currFemale);
                femalePartner[currFemale] = currMale;
            }
            else if (femalePreference[currFemale][currMale] < femalePreference[currFemale][femalePartner[currFemale]])
            {
                maleQueue.Dequeue();
                maleQueue.Enqueue(femalePartner[currFemale]);
                stableSet.Remove(femalePartner[currFemale]);
                stableSet.Add(currMale, currFemale);
                femalePartner[currFemale] = currMale;
            }
            else
            {
                malePreference[currMale].RemoveFirst();
            }
            Console.WriteLine(string.Join(',', stableSet));
        }

        return stableSet;
    }
```

---

Ref

交大開放式課程—演算法／楊蕙如