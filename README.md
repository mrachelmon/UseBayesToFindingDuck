# UseBayesToFindingDuck
### [HackMD網址](https://hackmd.io/@fsxCZX7iQ3aJ2sYH5olAYw/ry3B7FbpK)
> 因為公式跑版，以防萬一我還是附上我Hackmd的連結。
### [GitHub網址](https://github.com/mrachelmon/UseBayesToFindingDuck)
## 專案描述:
從教授提供的無人機空拍的鴨場圖中獲取training的dataset，並利用貝葉斯分類器(Bayes classifier)，從整張圖像中提取出鴨子本體像素。

#### 教授給的步驟建議:
##### 步驟 1：
從給定圖像中手動收集盡可能多的鴨體樣本像素。您可以使用一些圖像處理軟件在給定圖像上剪下鴨子的身體。此外，您可能需要從圖像中刪除一些非鴨體像素。

##### 步驟 2：
使用鴨體像素和非鴨體像素的 3 維（[red,green,blue]）特徵向量，須建立兩個Gaussian probabilistic likelihood models，
> $P(x|\omega0)$ 和 $P(x|\omega1)$

非鴨類models $\omega0$，鴨類models $\omega1$，兩個models的參數（均值向量 $\mu$和協方差矩陣 $\Sigma$）可以通過從最大似然估計得出的公式進行估計。

##### 步驟 3：
導出兩個高斯分佈模型$P(x|\omega0)$和$P(x|\omega1)$後，應用貝葉斯決策規則對給定圖像上的每個像素進行分類。在這裡，我們假設$P(\omega0)\;=\;P(\omega1)$ .因此，只需要在應用貝葉斯決策規則時比較似然值。

##### 步驟 4：
為了方便可視化，須輸出一個圖像，該圖像將每個鴨體像素都用您的分類器進行分類，並將所有非鴨體像素替換為黑色像素。
## 專案環境:
* Window10
* Python
* Jupyter

## 執行步驟:
### Step1 取得Dataset:
* 使用工具:Krita
* 鴨子樣本圖(duck.jpg):產一個500pixel $\times$ 220pixel的底圖，再擷取鴨子的部分貼上去。因為我有產底圖，為了防止學習到的底圖特徵會跟實際上鴨子的特徵相差太多，我將底圖的顏色調成跟鴨子身上相近的顏色。
    > ![](https://i.imgur.com/p5sDYkh.png)

* 非鴨子樣本圖(unduck.jpg):產一個大概1200pixel $\times$ 800pixel的底圖，再把原圖裡沒有鴨子的部分貼上去。
    > ![](https://i.imgur.com/qYaiIGI.jpg)

### Step2 將Dataset從圖檔轉成CSV檔:
* 分別讀取鴨子樣本圖跟非鴨子樣本的圖檔圖，依序存取每個pixel的RGB值與類別值，最後將兩個樣本圖的讀取值寫入同個csv檔中，格式如下:

![](https://i.imgur.com/ncu7GUN.png)

> label = 1 $\rightarrow$ 鴨子 \
> label = 2 $\rightarrow$ 非鴨子

### Step3 準備 training data 與 testing data:
* training data: 為step2的CSV檔。
* testing data: 為原圖。

### Step4 對像素點做預測:
* 從training data中計算R,G,B這三個數值欄各自的均值與標準差。
* 將這些值利用append組成該label的vector。
* 再算出各自label的條件機率。
* 最後用Bayes Classifier做預測。

### Step5 將預測的結果做可視化處理:
* 預測的結果為1，代表預測的類別為鴨子，則將當個像素點設為白色。
* 預測的結果為非1，代表預測的類別為非鴨子，則將當個像素點設為黑色。

## 結果展示:
#### 拆成上半部跟下半部的原因:
下圖為我直接用原圖去跑，跑出來的結果。我現在還沒想出來為甚麼會被壓縮成這樣的原因，而且整體跑的時間比拆成上半部與下半部的時間加起來還久。
![](https://i.imgur.com/UhTxWch.png)

#### 原圖上半部vs結果圖上半部:
![](https://i.imgur.com/sKOG7Ra.jpg)

#### 原圖下半部vs結果圖下半部:
![](https://i.imgur.com/fSMYMRL.jpg)

## 結論:
我覺得整體而言大致上還是好的，基本上都有抓出來，只是可能因為我鴨子樣本圖(duck.jpg)有其他底色的關係以及我非鴨子樣本中石頭樣本不夠多，導致某些小石頭會被判斷成鴨子而呈現白點，因為時間關係我就沒有修改鴨子樣本圖了，但等我寒假有時間我會再來試試看針對上面兩點做更改能否改善我的準確度。
## 心得感想:
我覺得這次的的作業對我來說是一大挑戰，因為我上學期才提早入學轉進來，再加上我並不是資工背景出生，所以對於python跟很多本科生比相對起來沒那麼熟練，加上我上學期的課以及研究的內容基本上都是深度學習為主，雖然有一點點相關，但大還是有很相異的地方，且這堂課有很大一部份是機率統計，因此對我來說有一定的難度。但再跟同學指教以及自己摸索研究後，還是完成了這次的作業，真的很感謝同學們給我的幫助。我深知自己還有很多的不足，我會努力在畢業前將這些不足的地方慢慢填補，希望在畢業前可以有所成長。



