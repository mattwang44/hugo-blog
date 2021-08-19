---
title: "MTIMESX: MATLAB快速多維矩陣乘積"
subtitle: ""
description: "MTIMESX: reduce the execution time enumerously for multi-dimension array multiplication with MATLAB"
excerpt: "MTIMESX: reduce the execution time enumerously for multi-dimension array multiplication with MATLAB"
date: 2018-04-20 15:00:00
author: "Matt Wang"
image: "https://opendatascience.com/wp-content/uploads/2019/04/banner2-750x350.jpg"
published: true
tags:
  - MATLAB
URL: "/2018/04/20/matlab-mtimesx/"
categories: ["Tech"]
---

在多維矩陣運算當中，雖然用多個 for-loops 處理較為直觀易懂，但其實非常耗時。在我修習台大醫工所的[人體動做分析電腦計算方法][hma]時，發現了 MATLAB 有[MTIMESX][mtimesx]這個多維矩陣乘積函式，底層為 C 語言，大幅減低了運算時間。

# **安裝步驟**

工具包作者已經寫好安裝執行檔，只需要做幾個設定便能開始自動安裝。以下是在 MATLAB2017b 版本上的範例安裝過程。

1. 首先，先從[MTIMESX 之 Mathworks 頁面][mtimesx]上將其下載、解壓縮，並將資料夾改名為`mtimesx`:
   ![](/img/posts/MATLAB/MTIMESX/0.png)

2. 將`mtimesx_build.m`檔案中第 166 行改成 `mexopts = [prefdir '\mex_C_win64.xml'];`:
   ![](/img/posts/MATLAB/MTIMESX/1.png)

3. 將 `mtimesx`資料夾移動到 MATLAB 安裝位置中的 toolbox 資料夾，它的位置大概會像是 `C:\Program Files\MATLAB\R2017b\toolbox`:
   ![](/img/posts/MATLAB/MTIMESX/2.png)

4. 開啟 MATLAB，在 command window 中輸入`mex -setup`會顯示電腦中目前有的 Compiler。下圖是預設顯示 C compiler，輸入`mex -setup C++`則可以查看 C++ compiler:
   ![](/img/posts/MATLAB/MTIMESX/3.png)

5. 輸入上一步驟中顯示的指令，選擇 C/C++ Compiler(若電腦沒有安裝 C/C++則必須先去安裝 Microsoft Visual C++)，我是選擇我電腦當中 C compiler 中最新版本:
   ![](/img/posts/MATLAB/MTIMESX/4.png)

6. 執行 `mtimesx_build.m` 檔案，一段時間過後最後看到"you may now use mtimesx"代表安裝成功:
   ![](/img/posts/MATLAB/MTIMESX/5.png)

7. 在其他位置試用 `mtimesx()`，會顯示函式還沒被加到 MATLAB 路徑中，點擊下圖中綠框字樣便會自動加入:
   ![](/img/posts/MATLAB/MTIMESX/6.png)

# **使用方法與範例**

必須先將兩個輸入矩陣中要做乘積的兩個維度移動到最前面(例如使用[permute][permute])，且其他對應的維度值要相同，mtimesx 會在其他維度上自動作二維乘積。

例如說現在要計算矩陣`A`(大小為`[a,b,d1,d2,d3,...]`)和矩陣`B`(大小為`[b,c,d1,d2,d3,...]`)，而 `C=mtimesx(A,B)`，矩陣`C`的大小便為`[a,c,d1,d2,d3,...]`。

另外，也可輸入矩陣後面加上'T','C','G'來分別為輸入矩陣做轉置、共軛轉置、共軛，格式為`mtimesx( A [,transa] ,B [,transb] )`:

|       MATLAB 函式        |     同義公式     |
| :----------------------: | :--------------: |
|     C = mtimesx(A,B)     |    C = A \* B    |
|   C = mtimesx(A,'T',B)   |   C = A.' \* B   |
|   C = mtimesx(A,B,'g')   | C = A \* conj(B) |
| C = mtimesx(A,'c',B,'C') |   C = A' \* B'   |

# **實際測試**

當輸入矩陣的維度越多且維度值越大，`mtimesx()`能夠節省下來的時間就越多，如果是做 2~3 維的運算，使用`mtimesx()`反而會比較慢。以下是測試五維矩陣運算的執行時間比較:
![](/img/posts/MATLAB/MTIMESX/try.png)

[hma]: https://github.com/mattwang44/Human-Motion-Analysis-MATLAB
[mtimesx]: https://www.mathworks.com/matlabcentral/fileexchange/25977-mtimesx-fast-matrix-multiply-with-multi-dimensional-support
[permute]: https://www.mathworks.com/help/matlab/ref/permute.html
