---
title: Git 常見狀況題
categories: Git develop
---

當做了多個檔案變動，但希望分兩個branch時

最近工作在補之前model的test，但我是很晚期才加入project，所以很多model我其實很不熟。
今天我在看model A1, A2, A3 時看不懂，亂改一陣後發現不行，需要前人的智慧或是從熟悉的下手。
想要去看之前碰過的model B1, B2 來參考，但卻發現B系列被改壞了，決定先把B系列model tests 補齊。
忙了一陣總算補完後，git 中充滿了modified files。
但又不想全部都在同個branch中，這樣會造成之後merge管理的困難，於是這篇就產生拉：

其實超級簡單

假設現在長這樣：

branch: feature/revising-As

| States | files           |
| -------| ----------------|
| Repo   |  Other Files    |
| Staging|                 |
| Working|  A1 A2 A3 B1 B2 |

希望A, B 分兩個branch
因為B已經做完了，我們把branch name 換掉，將Bs commit
查查rename的方式
```git
git branch (-m | -M) [<oldbranch>] <newbranch>
```
所以
```git
git branch -m feature/revising-As feature/revising-Bs
git add B1
git add B2
git commit -m "Finish Bs"
git push
```
現在長這樣

branch: feature/revising-Bs

| States | files             |
| -------| ------------------|
| Repo   |  Other Files B1 B2|
| Staging|                   |
| Working|  A1 A2 A3         |

As 也不會push到remote，很好

然後新開branch，再繼續修改A就行
```git
git branch feature/revising-As
git add .
git commit -m "tmp commit"
```
之後改完A再用rebase調整

好像怪怪的，之後再補充
因為這樣B的變動會進到branch A中
不太對

感謝jyt大大的指導，雖然寫完發現跟這篇沒關係，但這篇真的很精闢，如果是中文母語的git使用者都推薦看一下
[@jyt0532](https://www.jyt0532.com/2017/03/30/git-reset/)
（這種複雜的問題居然會發生在我身上，有種真的進步的味道）
