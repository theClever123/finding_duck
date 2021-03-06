---
title : 圖形識別(finding duck)
---
[TOC]

## **0.使用的工具**
這次使用jupyter，語言為python



## **1.描述問題**
請從以下圖片找出鴨子(白色的鴨身)的位置，並且輸出一張圖片，是鴨子的位置塗成白色，而不是鴨子的位置則是塗成黑色
![](https://i.imgur.com/SGwNnGV.jpg)




## **2.使用方法**
一開始先放入full_duck.jpg，並且以陣列的方式讀取它，求得一個5946*13816的三維陣列(RGB)，可以發現總像數點有接近2億5千萬之多，

![](https://i.imgur.com/Gr13VJb.png)

然後根據一樣的方式分別讀入有鴨子的模型和沒有鴨子的模型，有鴨子的模型是從full_duck圖片中，利用小畫家切割一個小範圍得出的一張圖片

* testduck.jpg長跟寬分別為40和44(RGB三維陣列)
* noducktest.jpg長跟寬分別為28和32(RGB三維陣列)

![](https://i.imgur.com/GAtGLf9.png)

以下為testduck.jpg(有鴨子的模型)、
noducktest.jpg(沒有鴨子的模型)的圖片

* testduck.jpg
![](https://i.imgur.com/RpgnHOP.jpg)

* noducktest.jpg
![](https://i.imgur.com/aW0YIa8.jpg)

再來按照老師上課所提到的Maximum Likelihood Estimator (MLE)算出mu和sigma(共變異矩陣)，這邊sigma是除以n-1的原因是因為模板並不是full_duck的所有模板，只是圖片中"部分"的模板，所以要除以n-1作為分母

![](https://i.imgur.com/UFJ90J3.png)
![](https://i.imgur.com/kgZF7Hk.png)

以下為程式碼，算出鴨子模型的mu、sigma，得出duck_mu、covar_x
![](https://i.imgur.com/6V6cH2i.png)
![](https://i.imgur.com/M0SmWAr.png)

計算出鴨子模型的mu、sigma之後，也要計算非鴨子模型的mu、no_duck_mu、no_covar_x

![](https://i.imgur.com/h3uY8zS.png)

然後問題目標是找出鴨子並且標記為白色，所以我們需要一個能夠"挑選"的辦法，這次我選擇了貝氏分類器，因為我們都是在同一張圖片，並且假設出現鴨子的機率和不會出現鴨子的機率一樣等於0.5，所以後續我只需要得出full_duck圖片中根據以下公式得出鴨子機率和非鴨子機率進行比較，如果鴨子機率比較大，那麼就會被我標註為有鴨子(白色)

![](https://i.imgur.com/GPxYGvX.png)

為了保留原圖(full_duck.jpg)，所以我先製作出一個和原圖一樣大小，且預定全部像數點為黑色，轉換成陣列，準備改寫陣列內容

![](https://i.imgur.com/Z4DI2DP.png)

下圖為程式碼，full_duck每一個像數點進行計算機率，然後根據前面貝氏分類器的說明，得出機率比較大的塗顏色為黑色或白色
![](https://i.imgur.com/k9Y7ili.png)

最後，再將陣列轉換成圖片，顯示出來，就是做為這次輸出的結果(圖片)
![](https://i.imgur.com/igQ0hdD.png)


## **3.结果說明**
### 最佳輸出的圖片:
![](https://i.imgur.com/d4BQSWg.png)
![](https://i.imgur.com/y3I57fm.png)
最開始的兩張圖的模板進行結合得出的圖片，可以發現陸地的問題依然沒有辦法正確判斷，而水池中的鴨子大部分有正確判斷出來
![](https://i.imgur.com/DKIl6ZS.png)

### 問題說明
透過紅色框框顯示，可以發現有很多地方實際上是陸地，但是我錯誤分類為鴨子，不過在中間水池裡面的鴨子有分得比較明確，這有可能是因為陸地的顏色和鴨子的顏色很接近(都是白色)，而水池的顏色比較深，和鴨子可以明確分開，所以在水池的鴨子比較容易被標記
![](https://i.imgur.com/WyoVgxl.png)

這張圖是我選擇了比較多的非鴨子模型(大概3、4個)最後得出的輸出結果，可以發現中間水池的鴨子點變得比較明顯，但是外圍陸地和鴨子就沒辦法顯示差別，導致只有中間水池的部分較為精確
![](https://i.imgur.com/y3I57fm.png)



## **4.總結**
這次的輸出中，在水池的部分比較精確，而在陸地上的鴨子很困難完全確定位置



## **5.遇到的困難以及突破**
1.從前面full_duck的圖片大小可以發現，大約2億多個像數點要計算，我光是一個鴨子模型和一個非鴨子模型下去跑就需要大概1小時半的時間，所以在確認程式碼沒有錯誤前，我是用了替代圖片進行下去的，等到確認程式碼無誤我才選擇使用原圖。
![](https://i.imgur.com/raddE2p.jpg)

2.後來跟同學討論才發現陣列和陣列的"內積"是有特定公式的(math 套件)，而不是使用" * "就可以，所以在撰寫程式碼時才會得出很多錯誤的答案。

3.鑑於最後輸出的圖片中，陸地和鴨子的區別其實並沒有很大，所以我有多做了很多陸地的模板進行比較，但是得出的結果都是沒辦法明確分辨，以下為使用過的陸地模板(少數舉例)

![](https://i.imgur.com/2lgOoCT.jpg)

![](https://i.imgur.com/qk4Xp4h.jpg)

![](https://i.imgur.com/9eXAANl.jpg)

4.有時候電腦跑著就掛了，所以有使用到實驗室的電腦，但是原圖計算量真的太大了，需要跑很久，其中第二個程式(finding duck(more_model))就跑了將近3個小時，所以只能選擇使用較少的模板進行計算。

5.每一次割下的模板都不太一樣，導致得出的結果有很大不同，但是陸地的部分依舊沒辦法處理。


## **6.未來討論**
如果未來有需要增加準確度的話，我覺得朝向明確陸地和鴨子的位置能夠增加正確率，並且如果情況允許，可以試著增加更多的模板進行比較


## **7.心得**
原本一開始聽到從圖片要去尋找鴨子是很困難的地方，至少那時候我完全沒有想法可以實行，但是透過這堂課，老師讓我們使用MLE、貝氏分類器的方式，經過這次實作，我也完成了尋找鴨子，雖然成效不太好，但是勉強有完成這個作業，這次我學到了很多東西，例如python，之前兩年都是使用C和C++，很少使用到python，需要重新學習一下，還學到了math 套件的部分指令。
原本對於MLE、貝氏分類器等等的課程也只是學習到了表面的運作，這次實作能夠更加的了解它們，程式前前後後花了很久，最後才得出結論。
