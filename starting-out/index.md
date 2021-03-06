---
layout: page
title: 出發
prev:
    url: introduction
    title: 引言
next:
    url: types-and-typeclasses
    title: 型別與 Typeclass
---

## <a name="ready-set-go">就定位，預備，開始！</a>

<img src="img/startingout.png" alt="egg" style="float:right" />
好吧，讓我們開始！假如你是那種直接跳過介紹不看的討厭鬼，你可能還是需要去閱讀引言的最後一節，因為那裡面講了此教學所需要的東西，以及我們該如何載入 function。我們所要做的第一件事是執行 ghc 的互動模式，然後呼叫一些 function 以對 haskell 有個十分基礎的體驗。開啟你的終端機（terminal）並輸入 `ghci`。你將會看到像這樣的歡迎訊息：

<pre name="code" class="haskell: ghci">
GHCi, version 6.8.2: http://www.haskell.org/ghc/  :? for help
Loading package base ... linking ... done.
Prelude>
</pre>

恭喜，你已經進入 GHCI 了！這裡的命令提示字元是 `Prelude>`，不過因為它會在你載入什麼東西時變長，所以我們想要改成 `ghci>`。假如你想要有一樣的提示字元，只需要輸入 `:set prompt "ghci> "`。

這裡是一些簡單的算術：

<pre name="code" class="haskell: ghci">
ghci> 2 + 15
17
ghci> 49 * 100
4900
ghci> 1892 - 1472
420
ghci> 5 / 2
2.5
ghci>
</pre>

不言自明。我們也可以在一行裡使用多個運算子（operator），其遵守所有常見的優先次序規則（precedence rule）。我們可以使用括號來讓優先次序清楚些，或是改變原本的優先次序。

<pre name="code" class="haskell: ghci">
ghci> (50 * 100) - 4999
1
ghci> 50 * 100 - 4999
1
ghci> 50 * (100 - 4999)
-244950
</pre>

非常酷，對吧？好吧，我知道這不酷，不過耐心聽我說。負數在是這裡一個需要小心的陷阱。假如你想要一個負數，把它用括號包起來是最好的。執行 `5 * -3` 將會使 GHCI 對你大叫，不過 `5 * (-3)` 會運作地很好。

布林（boolean）代數也非常簡單。如同你大概知道的：`&&` 代表布林值 <i>and</i>，`||` 代表布林值 <i>or</i>。`not` 反轉（negate）一個 `True` 或是 `False`。

<pre name="code" class="haskell: ghci">
ghci> True && False
False
ghci> True && True
True
ghci> False || True
True
ghci> not False
True
ghci> not (True && True)
False
</pre>

相等性的測試像這樣：

<pre name="code" class="haskell: ghci">
ghci> 5 == 5
True
ghci> 1 == 0
False
ghci> 5 /= 5
False
ghci> 5 /= 4
True
ghci> "hello" == "hello"
True
</pre>

執行 `5 + "llama"` 或是 `5 == True` 會怎麼樣呢？嗯，假如我們嘗試第一種情況，我們將會得到一個非常可怕的的錯誤訊息：

<pre name="code" class="haskell: ghci">
No instance for (Num [Char])
arising from a use of `+' at &lt;interactive&gt;:1:0-9
Possible fix: add an instance declaration for (Num [Char])
In the expression: 5 + "llama"
In the definition of `it': it = 5 + "llama"
</pre>

呀！這裡 GHCI 告訴我們的是：`"llama"` 並不是一個數字，所以它不知道該如何將它加上 5。即使這裡不是 `"llama"` 而是 `"four"` 或是 `"4"`，Haskell 仍然不能將它辨別成數字。`+` 預期它左右兩邊都是數字。假如我們嘗試執行 `True == 5`，GHCI 會告訴我們型別不相符。`+` 只能在被判別為數字的值上運作，`==` 只能在兩個可以被比較的值上運作。不過要點是它們必須擁有相同的型別：你無法把蘋果拿來跟橘子比。我們將會在稍後細看型別。記住：你可以執行 `5 + 4.0`，因為 `5` 可以作為一個整數（integer）或是一個浮點數（floating-point number）操作。`4.0` 無法作為整數操作，所以 `5` 必須配合它當作浮點數來運算。

也許你還不知道，不過我們現在一直都在使用 function。舉例來說，`*` 是一個將兩個數相乘的 function。如你所見，我們把它夾在兩個數字之間來呼叫它。這被我們稱為<i>中綴（infix）</i> function。大部分不是用在數字的 function 為<i>前綴（prefix）</i> function。讓我們來看看它們。

<img src="img/ringring.png" alt="phone" style="float:right" />
function 通常都是前綴的。所以從現在開始，我們都假設一個 function 是前綴形式，而不會明確陳述這點。在多數命令式語言中，function 藉由寫成 function 名稱接著括號中的參數——通常以逗點隔開——來呼叫的。在 Haskell 裡，function 則是藉由寫成 function 名稱、空白、然後是以空白隔開的參數來呼叫。一開始，我們嘗試呼叫 Haskell 其中一個最無聊的 function：

<pre name="code" class="haskell: ghci">
ghci> succ 8
9
</pre>

`succ` 這個 function 接收任何擁有已定義後繼子（successor）的值，並傳回此後繼子。如你所見，我們僅將 function 名稱與參數以空白來隔開。呼叫多個參數的 function 也很簡單。`min` 與 `max` 這兩個 function 接收兩個可以被排列順序的值（像是數字！）。`min` 傳回比較小的那個，而 `max` 傳回比較大的那個。你自己試看看吧：

<pre name="code" class="haskell: ghci">
ghci> min 9 10
9
ghci> min 3.4 3.2
3.2
ghci> max 100 101
101
</pre>

function application（在一個 function 後面接著空白，然後輸入參數以呼叫它）擁有最高的優先次序。我們所說的意思是，這兩個敘述（statement）是相等的：

<pre name="code" class="haskell: ghci">
ghci> succ 9 + max 5 4 + 1
16
ghci> (succ 9) + (max 5 4) + 1
16
</pre>

然而，假如我們想要取得 9 與 10 乘積的後繼子，我們無法寫作 `succ 9 * 10`，因為這將會取得 9 的後繼子再乘以 10，所以是 100。我們必須寫成 `succ (9 * 10)` 以得到 91。

假如一個 function 接收兩個參數，我們也可以將它用反引號（backtick）包起來，如同前綴 function 一樣呼叫它。舉例來說，`div` 這個 function 接收兩個整數，然後對他們進行整數相除（integral division）。執行 `div 92 10` 結果是 9。不過當我們像這樣呼叫它，可能會對哪個數字是除數、哪個數字是被除數有所混淆。所以我們藉由執行 ``92 `div` 10`` 把它當做中綴 function 來呼叫，立刻變得清楚許多。

許多來自於命令式語言的人往往堅持 function application 需要用括號來表示。例如在 C 語言中，你採用括號來呼叫 function，如： `foo()`、`bar(1)` 或是 `baz(3, "haha")`。如同我們所說，在 Haskell 中的 function application 是使用空白的。所以在 Haskell 中，這些 function 將會是 `foo`、`bar 1` 與 `baz 3 "haha"`。所以你看到像是 `bar (bar 3)` 並不代表 `bar` 被 `bar` 與 `3` 作為參數呼叫。它代表的是：我們首先以 `3` 作為參數呼叫 `bar` 以得到某個數字，然後我們以這個數字再次呼叫 `bar`。在 C 語言中，這就如同 `bar(bar(3))`。

## <a name="babys-first-functions">我們的第一個 function</a>

在前一節我們已經對呼叫 function 有個基本的體驗。現在讓我們來嘗試建立我們自己的 function！開啟你最喜歡的文字編輯器，並輸入這個接收一個數字並將之乘以兩倍的 function。

<pre name="code" class="haskell: hs">
doubleMe x = x + x
</pre>

function 被定義得像是它被呼叫的方式：function 名稱後緊跟著以空白隔開的參數。不過在定義 function 時，還有一個 `=` 接著我們所定義的 function 行為。將它儲存成 `baby.hs` 或是其他名稱。現在導覽到檔案儲存的位置，並在此執行 `ghci`。一旦進入 GHCI，就執行 `:l baby`。現在我們的腳本已經被載入了，我們可以來試試這個我們定義的 function。

<pre name="code" class="haskell: ghci">
ghci> :l baby
[1 of 1] Compiling Main             ( baby.hs, interpreted )
Ok, modules loaded: Main.
ghci> doubleMe 9
18
ghci> doubleMe 8.3
16.6
</pre>

因為 `+` 能夠在整數與浮點數上運作（事實上，任何能被視為數字的值都可以），我們的 function 也能在任何數字上運作。讓我們來建立一個接收兩個數字，將它們各自乘以二，然後再加總起來的 function。

<pre name="code" class="haskell: hs">
doubleUs x y = x*2 + y*2
</pre>

很簡單。我們也可以把它定義成 `doubleUs x y = x + x + y + y`。測試看看，你會得到這個相當顯而易見的結果（記得將這個 function 附加到 `baby.hs`，儲存之後在 GHCI 執行 `:l baby`）：

<pre name="code" class="haskell: ghci">
ghci> doubleUs 4 9
26
ghci> doubleUs 2.3 34.2
73.0
ghci> doubleUs 28 88 + doubleMe 123
478
</pre>

正如預期，你可以從你建立的別的 function 呼叫你自己的 function。考慮到這一點，我們可以像這樣重新定義 `doubleUs`：

<pre name="code" class="haskell: hs">
doubleUs x y = doubleMe x + doubleMe y
</pre>

這是一個非常簡單的範例，此種模式你會經常在整個 Haskell 見到。建立一個明顯是正確的基礎 function，然後將它們結合成更複雜的 function。這種方法也讓你免於重複撰寫。假如某些數學家發覺 2 其實是 3，讓你必須去修改你的程式怎麼辦？你可以只將 `doubleMe` 重新定義成 `x + x + x`，同時因為 `doubleUs` 呼叫了 `doubleMe`，它自動而然地能夠在這個 2 是 3 的奇怪新世界上運作。

在 Haskell 中的 function 並沒有任何特殊的順序，所以它並不在乎你是先定義 `doubleMe` 才定義 `doubleUs`，還是相反過來。

現在我們準備要建立一個將某數乘以二的 function，不過只有在這個數字小於或等於 100 的情況，因為大於 100 的數字已經足夠大了！

<pre name="code" class="haskell: hs">
doubleSmallNumber x = if x > 100
                        then x
                        else x*2
</pre>

<img src="img/baby.png" alt="this is you" style="float:left" />
在這裡我們引入了 Haskell 的 if 敘述。你可能對其他語言裡的 if 敘述很熟悉。Haskell 的 if 敘述與命令式語言中的 if 敘述的不同在於：else 的部份在 Haskell 中是強制的。在命令式語言中，你可以在條件（condition）不滿足的情況下跳過幾個步驟，不過在 Haskell 中，每個 expression 與 function 都必須傳回某值。我們也可以把 if 敘述寫在同一行，不過我發現現在這樣更好讀。另外一件關於 Haskell 中的 if 敘述的事，就是它是一個 expression。一個 expression 基本上是一段傳回一個值的程式碼。`5` 是一個 expression，因為他傳回 5、`4 + 8` 是一個 expression、`x + y` 也是一個 expression，因為它傳回 `x` 與 `y` 的和。因為 else 是強制性的，一個 if 敘述總是會傳回某個值，這就是為什麼它是一個 expression 的原因。假如我們想要將先前 function 產生的每個數字加上一，我們可以把它寫成這樣：

<pre name="code" class="haskell: hs">
doubleSmallNumber' x = (if x > 100 then x else x*2) + 1
</pre>

若是我們省略了括號，這個 function 就只會在 `x` 大於 100 的時候加上一。注意在 function 名稱後面的 `'`。這一撇在 Haskell 的語法中並沒有任何特殊的意義。它是一個用在 function 名稱的合法字元。我們有時候會使用 `'` 來表示一個嚴格版的（非惰性的）function，或是一個 function 或是變數（variable）稍作修改的版本。因為 `'` 是一個在 function 中的合法字元，我們可以建立一個像這樣的 function：

<pre name="code" class="haskell: hs">
conanO'Brien = "It's a-me, Conan O'Brien!"
</pre>

這裡有兩件值得注意的事情。第一件是在這個 function 名稱中，我們並沒有將 Conan 的名字寫成大寫。這是因為 function 不能以大寫字母開頭。我們將會在稍後看到原因。第二件事是這個 function 並不接收任何參數。當一個 function 不接收任何參數，我們通常稱它是一個 definition（或是一個 name）。因為我們無法改變 name（與 function），意味著一旦我們定義了 `conanO'Brien` 與字串（string） `"It's a-me, Conan O'Brien!"`，它們兩者就是等價的。

## <a name="an-intro-to-lists">List 簡介</a>

<img src="img/list.png" alt="BUY A DOG" style="float:left" />
如同真實世界中的購物清單，在 Haskell 中的 list 是非常有用的。它是最常被使用的資料結構（data structure），並且能夠被用於塑模與解決眾多問題的許多不同方法上。list 是如此地不凡。在這一小節，我們將會看到 list 的基礎、字串（它是個 list）與 list comprehension。

在 Haskell 中，list 是*同質的（homogenous）*資料結構。它儲存了許多相同型別的元素。這意味著我們可以有一個整數的 list 或是一個字元（character）的 list，但是我們不能建立一個有一些整數、同時有一些字元的 list。現在，讓我們來看看 list！

<p class="hint">
<em>註記：</em>我們可以使用 <code>let</code> 關鍵字來在 GHCI 中定義一個 name。在 GHCI 中執行 <code>let a = 1</code> 等同於在腳本中撰寫 <code>a = 1</code>，然後載入這個腳本。
</p>

<pre name="code" class="haskell: ghci">
ghci> let lostNumbers = [4,8,15,16,23,42]
ghci> lostNumbers
[4,8,15,16,23,42]
</pre>

如你所見，list 是以方括號來表示，list 中的值以逗號分隔。假如我們試圖建立一個像 `[1,2,'a',3,'b','c',4]` 這樣的 list，Haskell 將會抱怨字元（順帶一提，它被表示為單引號之間的一個字元）並不是數字。說到字元，字串只是一個字元 list。`"hello"` 只是個代表 `['h','e','l','l','o']` 的語法糖衣（syntactic sugar）。因為字串是 list，所以我們可以將 list 相關的 function 使用在字串上面，這是相當方便的。

一個常見的操作是將兩個 list 串在一起。這項操作藉由 `++` 來達成。

<pre name="code" class="haskell: ghci">
ghci> [1,2,3,4] ++ [9,10,11,12]
[1,2,3,4,9,10,11,12]
ghci> "hello" ++ " " ++ "world"
"hello world"
ghci> ['w','o'] ++ ['o','t']
"woot"
</pre>

當心對長字串重複使用 `++` 運算子的時候。當你將兩個字串串接在一起（即使你只是將一個單一元素的 list 附加在另一個 list 之後，比如： `[1,2,3] ++ [4]`），Haskell 必須走遍整個在 `++` 左邊的 list。當處理不太大的 list 時這不是個問題。不過要在一個長度為五千萬的 list 後串接什麼東西將會花上一段時間。然而，利用 `:` （也被稱為 cons 運算子）將某值串在一個 list 前面是立即完成的。

<pre name="code" class="haskell: ghci">
ghci> 'A':" SMALL CAT"
"A SMALL CAT"
ghci> 5:[1,2,3,4,5]
[5,1,2,3,4,5]
</pre>

注意到 `:` 是處理一個數字與一個數字的 list，或是一個字元與一個字元的 list，而 `++` 則是處理兩個 list。即使你只是想用 `++` 附加一個元素到一個 list 的尾端，你還是必須利用方括號把這個元素包起來，以讓它變成一個 list。

`[1,2,3]` 實際上只是個代表 `1:2:3:[]` 的語法糖衣。`[]` 為一個空的 list。假如我們把 `3` 加在前面，它會變成 `[3]`。假如我們再把 `2` 加在前面，它會變成 `[2,3]`，以此類推。

<p class="hint">
<em>註記：</em> <code>[]</code>、<code>[[]]</code> 與 <code>[[],[],[]]</code> 全都是不同的東西。第一個是一個空的 list，第二個是一個包含一個空 list 的 list，第三個是一個包含三個空 list 的 list。
</p>

假使你想要從一個 list 中透過索引（index） 來取出一個元素，使用 `!!` 吧。索引值從 0 開始。

<pre name="code" class="haskell: ghci">
ghci> "Steve Buscemi" !! 6
'B'
ghci> [9.4,33.2,96.2,11.2,23.25] !! 1
33.2
</pre>

不過假使你嘗試從一個只有四個元素的 list 中取出第六個元素，你將會得到一個錯誤，所以小心點吧！

list 也可以包含 list。它也可以包含「包含『包含 list』 的 list」 的 list....。

<pre name="code" class="haskell: ghci">
ghci> let b = [[1,2,3,4],[5,3,3,3],[1,2,2,3,4],[1,2,3]]
ghci> b
[[1,2,3,4],[5,3,3,3],[1,2,2,3,4],[1,2,3]]
ghci> b ++ [[1,1,1,1]]
[[1,2,3,4],[5,3,3,3],[1,2,2,3,4],[1,2,3],[1,1,1,1]]
ghci> [6,6,6]:b
[[6,6,6],[1,2,3,4],[5,3,3,3],[1,2,2,3,4],[1,2,3]]
ghci> b !! 2
[1,2,2,3,4]
</pre>

在一個 list 中的 list 可以是不同長度的，但是它們不能是不同型別的。就像你不能有一個同時包含一些字元與一些數字的 list，你也不能有一個同時包含一些字元 list 與一些數字 list 的 list。

若是 list 內含的東西可以被比較，list 本身也可以被比較。當使用 `<`、`<=`、`>` 與 `>=` 來比較 list 時，其會採用辭典順序（lexicographical order）：首先比較第一個元素。假如它們相等，則比較第二個元素，以此類推。

<pre name="code" class="haskell: ghci">
ghci> [3,2,1] > [2,1,0]
True
ghci> [3,2,1] > [2,10,100]
True
ghci> [3,4,2] > [3,4]
True
ghci> [3,4,2] > [2,4]
True
ghci> [3,4,2] == [3,4,2]
True
</pre>

還有什麼你能夠對 list 做的呢？這裡有一些用以操作 list 的基礎 function。

<code class="label function">head</code> 接收一個 list 並傳回它的 head。一個 list 的 head 基本上是它的第一個元素。

<pre name="code" class="haskell: ghci">
ghci> head [5,4,3,2,1]
5
</pre>

<code class="label function">tail</code> 接收一個 list 並傳回它的 tail。換句話說，它砍掉了一個 list 的 head。

<pre name="code" class="haskell: ghci">
ghci> tail [5,4,3,2,1]
[4,3,2,1]
</pre>

<code class="label function">last</code> 接收一個 list 並傳回它的最後一個元素。

<pre name="code" class="haskell: ghci">
ghci> last [5,4,3,2,1]
1
</pre>

<code class="label function">init</code> 接收一個 list 並傳回除了它最後一個元素以外的所有東西。

<pre name="code" class="haskell: ghci">
ghci> init [5,4,3,2,1]
[5,4,3,2]
</pre>

假如我們將一個 list 想成一個怪物，這就是它了。

<img src="img/listmonster.png" alt="list monster" style="margin:10px auto 25px;display:block">

不過假使我們試圖要從一個空的 list 取得 head 會發生什麼事呢？

<pre name="code" class="haskell: ghci">
ghci> head []
*** Exception: Prelude.head: empty list
</pre>

喔，我的天！我們栽了個跟斗。如果沒有怪獸，它就不會有頭（head）。使用 `head`、`tail`、`last` 與 `init` 的時候，小心別將它們用在空的 list 上頭。這個錯誤無法在編譯期被抓到，所以避免意外地從一個空的 list 裡取得元素總是個好的作法。

<code class="label function">length</code> 接收一個 list 並傳回它的長度，顯而易見。

<pre name="code" class="haskell: ghci">
ghci> length [5,4,3,2,1]
5
</pre>

<code class="label function">null</code> 確認一個 list 是否為空。如果是，它會回傳 `True`，否則它會回傳 `False`。要確認一個 list 是否為空，應該使用這個 function，而非 `xs == []` （假使你有一個叫做 `xs` 的 list）。

<pre name="code" class="haskell: ghci">
ghci> null [1,2,3]
False
ghci> null []
True
</pre>

<code class="label function">reverse</code> 反轉（reverse）一個 list。

<pre name="code" class="haskell: ghci">
ghci> reverse [5,4,3,2,1]
[1,2,3,4,5]
</pre>

<code class="label function">take</code> 接收一個數字與一個 list。它會從 list 的開頭擷取多個元素。看：

<pre name="code" class="haskell: ghci">
ghci> take 3 [5,4,3,2,1]
[5,4,3]
ghci> take 1 [3,9,3]
[3]
ghci> take 5 [1,2]
[1,2]
ghci> take 0 [6,6,6]
[]
</pre>

假如我們試圖從 list 中取得超過其擁有數量的元素，它只會傳回 list 本身。如果我們試圖取得 0 個元素，我們會得到一個空的 list。

<code class="label function">drop</code> 以類似的方式運作，除了它是從一個 list 的開頭丟棄數個元素。

<pre name="code" class="haskell: ghci">
ghci> drop 3 [8,4,2,1,5,6]
[1,5,6]
ghci> drop 0 [1,2,3,4]
[1,2,3,4]
ghci> drop 100 [1,2,3,4]
[]
</pre>

<code class="label function">maximum</code> 接收一個可以被排成某種順序的東西的 list，並傳回最大的元素。

<code class="label function">minimum</code> 回傳最小的元素。

<pre name="code" class="haskell: ghci">
ghci> minimum [8,4,2,1,5,6]
1
ghci> maximum [1,9,2,3,4]
9
</pre>

<code class="label function">sum</code> 接收一個數字 list，並傳回它們的總和。

<code class="label function">product</code> 接收一個數字 list，並傳回它們的乘積。

<pre name="code" class="haskell: ghci">
ghci> sum [5,2,1,6,3,2,5,7]
31
ghci> product [6,2,1,2]
24
ghci> product [1,2,5,6,7,9,2,0]
0
</pre>

<code class="label function">elem</code> 接收某值與一個 list，並告訴我們這個值是否為這個 list 的其中一個元素。它通常以中綴 function 的形式被呼叫，因為這種方式更好讀。

<pre name="code" class="haskell: ghci">
ghci> 4 `elem` [3,4,5,6]
True
ghci> 10 `elem` [3,4,5,6]
False
</pre>

這裡還有一些在 list 上操作的基礎 function。我們將會在[之後](modules#data-list)看到更多的 list function。

## <a name="texas-ranges">Texas ranges</a>

<img src="img/cowboy.png" alt="draw" style="float:right" />
假使你想要一個包含 1 到 20 之間所有數字的 list 呢？當然，你可以把它們全部打出來，但顯然這對於自程式語言中要求卓越的紳士來說並不是個解決方案。取而代之的，我們會使用 range。range 是建立等差數列 list 的方法之一，其中的元素是可以被列舉（enumerate）的。數字可以被列舉：一、二、三、四、等等。字元也是可以被列舉的：字母表是一個從 A 到 Z 的字元列舉（enumeration）。名稱不可以被列舉：「Jonn」後面是什麼呢？我不知道。

要建立一個包含 1 到 20 之間所有自然數的 list，你只要寫作 `[1..20]` 就可以了。這等同於寫作 `[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20]`。這之中沒什麼差別，除了手動寫下長的列舉序列這點還滿蠢的。

<pre name="code" class="haskell: ghci">
ghci> [1..20]
[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20]
ghci> ['a'..'z']
"abcdefghijklmnopqrstuvwxyz"
ghci> ['K'..'Z']
"KLMNOPQRSTUVWXYZ"
</pre>

range 很酷，因為你也可以指定一個 step。假使你想要 1 到 20 之間的所有偶數呢？或是 1 到 20 之間的所有三的倍數？

<pre name="code" class="haskell: ghci">
ghci> [2,4..20]
[2,4,6,8,10,12,14,16,18,20]
ghci> [3,6..20]
[3,6,9,12,15,18]
</pre>

這只是一個以逗號隔開前兩個元素，然後指定上限值的問題。雖然有 step 的 range 很聰明，但是它並不如有些人預期的那般聰明。你無法執行 `[1,2,4,8,16..100]` 並預期得到所有 2 的幂次。首先，因為你只能指定一個 step。其次是因為如果只給定前幾個元素，有些並非等差的序列是很含糊的。

要建立一個所有 20 到 1 數字的 list，你不能僅寫作 `[20..1]`，你必須要寫成 `[20,19..1]` 才行。

在 range 中使用浮點數時得當心！因為（出於定義）浮點數並非完全精確，所以將它們使用在 range 中會產生一些非常糟糕的結果。

<pre name="code" class="haskell: ghci">
ghci> [0.1, 0.3 .. 1]
[0.1,0.3,0.5,0.7,0.8999999999999999,1.0999999999999999]
</pre>

我的忠告是：不要在 list range 中使用浮點數。

只要不指定上限值，你也可以使用 range 來建立無限的 list。之後我們將會看到無限 list 的更多細節。現在，讓我們看看你該如何取得前 24 個 13 的倍數。當然，你可以寫作 `[13,26..24*13]`。不過這裡有個更好的方式：`take 24 [13,26..]`。因為 Haskell 是惰性的，它並不會馬上嘗試去生成無限 list，因為這永遠不會完成。它會等著，看你想要從這個無限 list 中取得什麼。在這裡它發現你僅是想要前 24 個元素，而它樂意幫忙。

以下是一些產生無限 list 的 function：

<code class="label function">cycle</code> 接收一個 list，並將之循環成一個無限 list。假如你嘗試要顯示結果，它將會一直跑下去。所以你必須要指定某個範圍。

<pre name="code" class="haskell: ghci">
ghci> take 10 (cycle [1,2,3])
[1,2,3,1,2,3,1,2,3,1]
ghci> take 12 (cycle "LOL ")
"LOL LOL LOL "
</pre>

<code class="label function">repeat</code> 接收一個元素，並產出一個僅有這個元素的無限 list。它就像是循環一個僅有一個元素的 list。

<pre name="code" class="haskell: ghci">
ghci> take 10 (repeat 5)
[5,5,5,5,5,5,5,5,5,5]
</pre>

然而，如果你只是想要一個某數量個相同元素的 list，使用 <code class="label function">replicate</code> function 是比較簡單的。`replicate 3 10` 回傳 `[10,10,10]`。

## <a name="im-a-list-comprehension">我是一個 list comprehension</a>

<img src="img/kermit.png" alt="frog" style="float:left" />
如果你曾上過一門數學課，你大概接觸過 <i>set comprehension</i>。它們通常被用來建立比一般集合（set）還更特定的集合。對於一個包含前十個偶數的集合，它的一個 comprehension 為 <img src="img/setnotation.png" alt="set notation" style="margin:0" />。在 `|` 之前的部份被稱為輸出函數、`x` 是變數、`N` 是輸入集合而 `x <= 10` 為述部（predicate）。這代表集合包含滿足述部的所有雙倍自然數。

如果我們將此寫在 Haskell 中，我們可以寫成像是 `take 10 [2,4..]`。不過如果我們要的不是將前十個自然數乘以兩倍，而是要將某種更複雜的 function 套用在它們之上？我們可以對此使用 list comprehension。list comprehension 非常類似於 set comprehension。我們現在堅持要取得前十個偶數。我們可以用的 list comprehension 為 `[x*2 | x <- [1..10]]`。`x` 來自於 `[1..10]`，並取得所有在 `[1..10]` 的元素（我們已經將其綁定在 `x`），將它乘以兩倍。來試試這個 comprehension：

<pre name="code" class="haskell: ghci">
ghci> [x*2 | x <- [1..10]]
[2,4,6,8,10,12,14,16,18,20]
</pre>

如你所見，我們得到了想要的結果。現在讓我們在這個 comprehension 加上一個條件（或是述部）。述部在綁定部分之後，並以逗號來分隔。讓我們假定，我們只想要乘以兩倍後大於或等於 12 的所有元素。

<pre name="code" class="haskell: ghci">
ghci> [x*2 | x <- [1..10], x*2 >= 12]
[12,14,16,18,20]
</pre>

酷，它能跑。如果我們想要從 50 到 100 中除以 7 的餘數為 3 的所有數字怎麼辦呢？簡單。

<pre name="code" class="haskell: ghci">
ghci> [ x | x <- [50..100], x `mod` 7 == 3]
[52,59,66,73,80,87,94]
</pre>

成功！注意到利用述部來清除 list 中不合條件的操作也被稱為*過濾（filtering）*。我們取得一個數字的 list 並以述部來過濾它們。現在舉另一個例子：讓我們假定我們想要一個取代每個大於等於 10 的奇數為 `"BANG!"`<span class="note">〔譯註：原文寫作「*大於* 10 的奇數」而非「大於等於」，從程式意義看來應為筆誤〕</span>，並取代每個小於 10 的奇數為 `"BOOM!"` 的 comprehension。假如某個數字不是奇數，我們將它從我們的 list 中捨棄。方便起見，我們將這個 comprehension 放在一個 function 裡面，所以我們可以輕易地重複使用它。

<pre name="code" class="haskell: hs">
boomBangs xs = [ if x < 10 then "BOOM!" else "BANG!" | x <- xs, odd x]
</pre>

comprehension 的最後一部分是述部。function `odd` 對於一個奇數回傳 `True`、對於一個偶數回傳 `False`。只有令所有的述部求值為 `True` 的元素會被包含在 list 中。

<pre name="code" class="haskell: ghci">
ghci> boomBangs [7..13]
["BOOM!","BOOM!","BANG!","BANG!"]
</pre>

我們可以引入多個述部。假使我們想要從 10 到 20 且不為 13、15 或 19 的所有數字，我們會這樣做：

<pre name="code" class="haskell: ghci">
ghci> [ x | x <- [10..20], x /= 13, x /= 15, x /= 19]
[10,11,12,14,16,17,18,20]
</pre>

我們不僅可以在 list comprehension 有多個述部（一個被包含在最終 list 的元素必須滿足所有的述部），我們也可以自多個 list 引入元素。當從多個 list 引入元素時，comprehension 會產生所有給定 list 的組合，並透過我們給定的輸出函數來加入它們。一個從兩個長度為 4 的 list 引入元素的 comprehension，其產生的 list──若是我們不過濾它們──長度將會是 16。
假使我們有兩個 list──`[2,5,10]` 與 `[8,10,11]`，且我們想要取得在這些 list 中所有可能的數字組合的乘積，在這裡我們會這麼做：

<pre name="code" class="haskell: ghci">
ghci> [ x*y | x <- [2,5,10], y <- [8,10,11]]
[16,20,22,40,50,55,80,100,110]
</pre>

正如預期，新的 list 長度為 9。如果我們想要所有大於 50 的可能乘積呢？

<pre name="code" class="haskell: ghci">
ghci> [ x*y | x <- [2,5,10], y <- [8,10,11], x*y > 50]
[55,80,100,110]
</pre>

一個結合形容詞的 list 與名詞的 list 的 list comprehension 如何....

<pre name="code" class="haskell: ghci">
ghci> let nouns = ["hobo","frog","pope"]
ghci> let adjectives = ["lazy","grouchy","scheming"]
ghci> [adjective ++ " " ++ noun | adjective <- adjectives, noun <- nouns]
["lazy hobo","lazy frog","lazy pope","grouchy hobo","grouchy frog",
"grouchy pope","scheming hobo","scheming frog","scheming pope"]
</pre>

我知道了！讓我們來寫我們自己的 `length`！我們把它叫做 `length'`。

<pre name="code" class="haskell: hs">
length' xs = sum [1 | _ <- xs]
</pre>

`_` 代表我們無論如何都不在意我們從 list 引入了什麼，所以我們只寫作 `_`，而不是一個我們永遠不會用到的變數名稱。這個 function 將 list 的所有元素取代為 `1` 並加總起來。這代表著最終的總和將會是我們 list 的長度。

只是個友善的提醒：因為字串為一個 list，我們可以使用 comprehension 來處理並產生字串。這裡是一個接收一個字串，並移除所有非大寫字母的 function：

<pre name="code" class="haskell: hs">
removeNonUppercase st = [ c | c <- st, c `elem` ['A'..'Z']]
</pre>

測試看看：

<pre name="code" class="haskell: ghci">
ghci> removeNonUppercase "Hahaha! Ahahaha!"
"HA"
ghci> removeNonUppercase "IdontLIKEFROGS"
"ILIKEFROGS"
</pre>

在這裡述部作了所有的工作。它假定會被包含在新的 list 之中的字元，只能是 list `['A'..'Z']` 中的其中一個元素。如果你要操作包含 list 的 list，巢狀的（nested）list comprehension 也是可能的。假使有一個包含許多數字 list 的 list。讓我們在不拆開 list 的情況下移除所有奇數。

<pre name="code" class="haskell: ghci">
ghci> let xxs = [[1,3,5,2,3,1,2,4,5],[1,2,3,4,5,6,7,8,9],[1,2,4,2,1,6,3,1,3,2,3,6]]
ghci> [ [ x | x <- xs, even x ] | xs <- xxs]
[[2,2,4],[2,4,6,8],[2,4,2,6,2,6]]
</pre>

你可以橫跨多行撰寫 list comprehension。所以如果你並不在 GHCI 中，把較長的 list comprehension 切成許多行是比較好的，尤其在它們為巢狀的時候。

## <a name="tuples">Tuples</a>

<img src="img/tuple.png" alt="tuples" style="float:right" />
在某些方面，tuple 就像是 list──它們都是將多個值存入一個單一值的方式。然而，它們有一些本質上的不同。一個數字的 list 就是一個數字序列。這就是它的型別，且與 list 之中是只有一個數字或是無限個數字無關。而 tuple 則被使用在你確切知道你想要結合多少值的情況，且它的型別取決於其中有多少個元素與元素的型別。tuple 以括號表示，且其元素以逗號來分隔。

另外一個主要的不同是：tuple 不必是同質的。與 list 不同，tuple 可以包含許多型別的組合。

想想我們要如何 Haskell 裡表示一個二維向量。一種方式是使用一個 list。這也許還可以。所以假設我們想要把一些向量放入一個 list，以表示在二維平面上一個形狀的點呢？我們可能會寫成像 `[[1,2],[8,11],[4,5]]` 這樣。這種方法的問題在於，我們也可以寫成像 `[[1,2],[8,11,5],[4,5]]` 這樣，由於它仍然是一個數字 list 的 list，對於 Haskell 來說這沒有問題，但寫成這樣並沒有意義。不過一個大小為二的 tuple（也被稱為一個 pair）就是它自己的型別，這意味著一個 list 無法有一些 pair 同時又有個 triple（大小為三的 tuple），所以讓我們以它來代替 list。我們並非用方括號來包住一個向量，而是使用括號：`[(1,2),(8,11),(4,5)]`。假使我們嘗試建立一個像是 `[(1,2),(8,11,5),(4,5)]` 的形狀呢？嗯，我們會得到這個錯誤：

<pre name="code" class="haskell: ghci">
Couldn't match expected type `(t, t1)'
against inferred type `(t2, t3, t4)'
In the expression: (8, 11, 5)
In the expression: [(1, 2), (8, 11, 5), (4, 5)]
In the definition of `it': it = [(1, 2), (8, 11, 5), (4, 5)]
</pre>

它告訴我們，我們嘗試在同一個 list 裡使用一個 pair 與一個 triple，這是不應該發生的。你也不能建立一個像是 `[(1,2),("One",2)]` 這樣的 list，因為 list 的第一個元素是數字的 pair，而第二個元素是由一個字串與一個數字組成的 pair。tuple 也可以被用來表示多種資料。舉例來說，假如你想要在 Haskell 裡表示某人的名字與年齡，我們可以使用一個 triple：`("Christopher", "Walken", 55)`。如同這個例子中所看到的，tuple 也可以包含 list。

使用 tuple 應當事先知道一筆資料有多少個元素。tuple 是非常嚴格的，因為每個不同大小的 tuple 都是它自己的型別，所以你無法寫一個通用的 function 來附加一個元素到 tuple 之上──你必須寫一個附加到 pair 的 function、一個附加到 triple 的 function、一個附加到 4-tuple 的 function 等等。

雖然有單一元素的 list，但是沒有像是單一元素的 tuple 這種東西。它在你思考它的時候並不具有多大意義。單一元素的 tuple 僅是它包含的值，而這對我們沒有好處。

如同 list，假如 tuple 的元素可以被比較，它就可以與其它 tuple 比較。只是你雖然可以將兩個不同大小的 list 做比較，而無法將兩個不同大小的 tuple 做比較。這裡有兩個操作在 pair 上的有用 function：

<code class="label function">fst</code> 接收一個 pair 並傳回它的第一個元素。

<pre name="code" class="haskell: ghci">
ghci> fst (8,11)
8
ghci> fst ("Wow", False)
"Wow"
</pre>

<code class="label function">snd</code> 接收一個 pair 並傳回它的第二個元素。真令人驚訝！

<pre name="code" class="haskell: ghci">
ghci> snd (8,11)
11
ghci> snd ("Wow", False)
False
</pre>

<p class="hint">
<em>註記：</em>這些 function 只能用來操作 pair。它不能在 triple、4-tuple、5-tuple 等上運作。我們將在稍後看到從 tuple 中提取資料的不同方式。
</p>

一個很酷的 function：<code class="label function">zip</code>，它會產生一個 pair 的 list。它接收兩個 list，然後藉由將配對的元素加入成一個 pair，把它們合併成一個 list。這是一個非常簡單的 function，但是它很常被使用。這在你想要在某種程度上合併兩個 list 或是同時走訪兩個 list 的時候尤其有用。這裡是個示範：

<pre name="code" class="haskell: ghci">
ghci> zip [1,2,3,4,5] [5,5,5,5,5]
[(1,5),(2,5),(3,5),(4,5),(5,5)]
ghci> zip [1 .. 5] ["one", "two", "three", "four", "five"]
[(1,"one"),(2,"two"),(3,"three"),(4,"four"),(5,"five")]
</pre>

它將元素配對並產生一個新的 list。第一個元素被第一個、第二個元素配第二個，以此類推。注意到因為 pair 裡可以有不同的型別，`zip` 可以接收並結合兩個包含不同型別的 list。如果 list 的長度不同會發生什麼事呢？

<pre name="code" class="haskell: ghci">
ghci> zip [5,3,2,6,2,7,2,5,4,6,6] ["im","a","turtle"]
[(5,"im"),(3,"a"),(2,"turtle")]
</pre>

比較長的 list 被截斷以配合比較短的 list 長度。因為 Haskell 是惰性的，我們可以結合有限的 list 與無限的 list：

<pre name="code" class="haskell: ghci">
ghci> zip [1..] ["apple", "orange", "cherry", "mango"]
[(1,"apple"),(2,"orange"),(3,"cherry"),(4,"mango")]
</pre>

<img src="img/pythag.png" alt="look at meee" style="margin:10px auto 25px;display:block" />

這裡有個結合 tuple 與 list comprehension 的問題：哪些每條邊長度皆為整數，且邊長等於或小於 10 的直角三角形，其周長為 24？首先，讓我們試著產生所有邊長等於或小於 10 的三角形：

<pre name="code" class="haskell: ghci">
ghci> let triangles = [ (a,b,c) | c <- [1..10], b <- [1..10], a <- [1..10] ]
</pre>

我們從三個 list 引入元素，而我們的輸出函數則將它們結合成一個 triple。如果你在 GHCI 中輸入 `triangles` 求值，你將會得到一個所有邊長小於或等於 10 的三角形 list。接下來，我們加上一個它們必須為直角三角形的條件。我們也修改此 function 以考慮到 b 邊長不大於斜邊，且 a 邊長不大於 b 的情況。

<pre name="code" class="haskell: ghci">
ghci> let rightTriangles = [ (a,b,c) | c <- [1..10], b <- [1..c], a <- [1..b], a^2 + b^2 == c^2]
</pre>

我們幾乎完成了。現在，我們修改 function 以表明我們想要周長為 24 的三角形。

<pre name="code" class="haskell: ghci">
ghci> let rightTriangles' = [ (a,b,c) | c <- [1..10], b <- [1..c], a <- [1..b], a^2 + b^2 == c^2, a+b+c == 24]
ghci> rightTriangles'
[(6,8,10)]
</pre>

而這就是我們的答案了！這是函數式程式設計的一個常見模式。你取得一個起始的解答集合，套用轉換到這些解答上，然後過濾它們，直到你得到正確的結果。