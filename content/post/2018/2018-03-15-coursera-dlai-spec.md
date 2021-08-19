---
title: "吳恩達 Deep Learning Specialization -- 課程介紹與心得"
subtitle: ""
description: "Brief introduction to the deep learning specialization on Coursera"
excerpt: "Brief introduction to the deep learning specialization on Coursera"
date: 2018-03-15 15:00:00
author: "Matt Wang"
image: "https://opendatascience.com/wp-content/uploads/2019/04/banner2-750x350.jpg"
published: true
tags:
  - course
URL: "/2018/03/15/deep-learning-specialization-coursera/"
categories: ["Tech"]
---

All photos credit to [deeplearning.ai](https://www.deeplearning.ai)

## 1. 修課背景

我自己是機械背景，在機械所碩士班一年級期間修習了台大資工林軒田教授開設的[機器學習基石][1]和[機器學習技法][2]兩門課
(也是從那個時候才開始用 Python3)，深度學習在該課程內容中只用了一個章節做簡單介紹(FC&Backprop)。課程結束後對於 ML/DL 的學習就這樣中斷了半年，
2018 一月底才在朋友介紹下得知 Stanford 教授同時也是 Coursera 創辦人吳恩達開的 Deep Learning Specialization(深度學習專項課程)，五門課程共 16 周份量，但實際上是很輕鬆的課程安排，最後我花了三周不到，學費也只花了約台幣 1500 元。

## 2. 課程資訊

[Coursera][4]上完整課程教材有:

1. 課程影片:

- 可單獨在[YouTube][3]上觀看
- 在平台上觀看則可開啟字幕

2. 小考:

- 每周十或二十題選擇題，有認真上課應該可在半小時內完成
- 80 分為及格分
- 未及格可以重複作答，但一定時間內有次數限制

3. 程式作業(個人覺得是最珍貴的教材):

- 每周 1~3 份，需作答部分只有實現該週教學核心的程式碼，程式其餘部分已經事先建立好
- 在線上 Jupyter Notebook 撰寫與運行，Notebook 中會把課程影片再次說明
- 從最基礎的 Python3 和 Numpy 開始教起，也會學到 Tensorflow 和 Keras 兩大熱門框架
- 可以在討論區找到很多提示，但直接放上解答是不被允許的
- 有 GPU 的話可把 code 載到本機跑，部分作業在線上跑頗慢(e.g. 第五課的 style transfer)
- 請注意，於網路散布解答違反 Coursera 規定 (不過中國網站還是一堆...不能偷看)

P.S. 課程影片進度, 小考前次作答結果, 作業進度都可以儲存，只要肯花時間都一定會通過

## 課程內容

這裡僅列出大綱，詳情請見[Coursera 網站][4]

#### 第一課: Neural Networks and Deep Learning

四週份量，前兩週都在講基礎機器學習會用到的東西，程式則由 Python, Numpy 開始教，最後實現 Logistic Regression。後兩週開始進入簡單的神經網路。

{{< figure src="/img/posts/coursera-dl/c1.jpg" >}}

#### 第二課: Improving Deep Neural Networks

三週份量講述 Regularization, Gradient, Noramlization, Dropout 等主題，最後講到一點調參數的概念。程式部分除了實現前述主題，也開始使用 Tensorflow。
{{< gallery >}}
{{< figure src="/img/posts/coursera-dl/c2.gif" >}}
{{< figure src="/img/posts/coursera-dl/opt1.gif" >}}
{{< /gallery >}}

#### 第三課: Structuring Machine Learning Projects

兩週份量，無程式作業，感覺是第二週講述調參數概念的延伸，如何從 train set, dev set, test set 的 error rate 判斷該如何繼續調整 model。

{{< figure src="/img/posts/coursera-dl/c3.jpg" >}}

#### 第四課: Convolutional Neural Networks

四週份量。第一週講述組成一層 CNN 的基本用法(kernal, padding, stride, pooling)，第二週講述經典架構 LeNet, ResNet, Inception Network 並開始用 Keras。
第三週是物體辨識技巧，程式會實現 YOLO(最一開始是被 YOLO 吸引來上這門課，上完後才去補齊其他門課)。第四周臉部辨識和 style transfer(讓圖片渲染畫作風格的演算法)。

{{< figure src="/img/posts/coursera-dl/c4.png" >}}

#### 第五課: Sequence Models

三週份量，在我上這套課程到一半時才出爐的新課，程式作業都很有趣。第一週是基本型與雙向型 RNN 架構及兩大運作單元 GRU 和 LSTM 的介紹，
程式學自動生成爵士樂、恐龍名字(XXXXXsaurus)和莎士比亞風格文章。第二週基礎 NLP/word embedding，程式作業會學到如何不讓演算法學到男女或種族所形成的語意差異，
以及判斷一句話並生成適合的表情符號。第三週真的學完了還是不知道在幹嘛，之前為了強迫自己看懂就自願去幫忙把第五課翻成繁中，結果進度緩慢......之後再補上。

{{< figure src="/img/posts/coursera-dl/c5.png" >}}

## 3. 平台選擇

課程教材在[Coursera][4]和[中國網易雲課程][5]都有發布(可以同時使用)，但我是在 Coursera 上完後才知道後者，如果不需要中文字幕的話用 Coursera 就夠了，以下依個人看法做點比較：

介面比較:

{{< figure src="/img/posts/coursera-dl/preview.jpg" >}}

| 平台     | Coursera                                 | 網易雲[註 ¹]                         |
| -------- | ---------------------------------------- | ------------------------------------ |
| 教材     | **課程影片, 小考, 程式作業**             | 課程影片(小考和作業未發布)           |
| 價格     | 免費, 有付費方案[註 ²]                   | 免費                                 |
| 中文字幕 | 全都有英文字幕, 但僅前三門課有繁簡字幕   | **大部分影片有簡中字幕**，無英文字幕 |
| 討論區   | **每項程式作業有獨立討論區, 有專人解答** | 課程討論混雜各種跪求文和教材失效回報 |
| 其他優點 | 付費方案可拿證書[註 ³ ⁴]                 | 課程投影片和筆記也有放在使用介面上   |

註 ¹: 註冊需要中國手機號碼收驗證碼

註 ²: USD49/月，有七天免費試用期，可隨時取消，使用付費方案才能線上批改每周小考和程式作業。

註 ³: 分數達標後會拿到可放 Linkedin 的證書(我的[證書連結][6]和[放上 Linkedin 的樣子][7])

註 ⁴: 可以全部作答完之後再開試用期批改但不推崇這種作法，好課還是付點小錢支持吧！

## 4. 修課心得

這真的是品質非常高的線上課程，教學內容都是從最為基礎的東西開始講，講解影片語速不快、邏輯清晰(我其實都開兩倍速看)。
程式作業設計得非常好，感覺連程式設計風格也傳授了不少，
該用甚麼 function 也都會提示，如果真的看不懂也有討論區可以查，有來自全世界的學生會參與討論。

對已經有一點 ML 基礎的我最大的幫助是，讓我非常快速的認識當今 DL 領域的基礎技術大概有哪些東西，現在再去看 DL 的論文也不會有太多的障礙。
今天甚至在討論區上看到有人沒大學文憑，但是靠著上完這門課就拿到灣區 intern。

但畢竟只是入門程度的課程(推廣 DL 性質)，程式作業有很大一部分是已經寫好的，去把檔案 copy 回來看其實就會發現不少沒講解過的東西，
很多時候作業也只是直接丟訓練好的參數來直接用(畢竟如果是用他們 server 跑，真的 train 可能需要數天)。
上完這五門課雖然可以說是對 DL 有更進一步的認識，感覺和真正熟悉並流暢運用的程度還是有不小的距離。

不過只花三週、台幣 1500 元，學到的份量已經超過我原先所能想像，非常滿足了！

[1]: https://www.csie.ntu.edu.tw/~htlin/course/mlfound16fall/
[2]: https://www.csie.ntu.edu.tw/~htlin/course/mltech17spring/
[3]: https://www.youtube.com/channel/UCcIXc5mJsHVYTZR1maL5l9w/playlists
[4]: https://www.coursera.org/specializations/deep-learning
[5]: https://mooc.study.163.com/smartSpec/detail/1001319001.htm
[6]: https://www.coursera.org/account/accomplishments/specialization/certificate/M73YAN978SSX
[7]: https://www.linkedin.com/in/wei-hsiang-wang-60841b108/
