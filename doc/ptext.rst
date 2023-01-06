文字格式
---------------

The :ref: `screen`'s 的 ``draw.text()`` 方法可以指定文本的位置和格式。可以指定四個角
或者中心點的坐標以及各個邊中點的坐標，以及單獨指定中心點以及各個角。還可以設置文本的
顏色、字體、字體大小，不過字體檔案要保存在當前 python 文件所在目錄的 fonts 目錄中，
才可以正常的設置字體。範例如下 ::

    screen.draw.text("Text color", (50, 30), color="orange")
    screen.draw.text("Font name and size", (20, 100), fontname="Boogaloo", fontsize=60)
    screen.draw.text("Positioned text", topright=(840, 20))
    screen.draw.text("Allow me to demonstrate wrapped text.", (90, 210), width=180, lineheight=1.5)
    screen.draw.text("Outlined text", (400, 70), owidth=1.5, ocolor=(255,255,0), color=(0,0,0))
    screen.draw.text("Drop shadow", (640, 110), shadow=(2,2), scolor="#202020")
    screen.draw.text("Color gradient", (540, 170), color="red", gcolor="purple")
    screen.draw.text("Transparency", (700, 240), alpha=0.1)
    screen.draw.text("Vertical text", midleft=(40, 440), angle=90)
    screen.draw.text("All together now:\nCombining the above options",
        midbottom=(427,460), width=360, fontname="Boogaloo", fontsize=48,
        color="#AAFF00", gcolor="#66AA00", owidth=1.5, ocolor="black", alpha=0.8)

``screen.draw.text`` 最簡單的用法只需要提供需要顯示的文本以及文本的坐標，不過文本
坐標的方式除了指定左上角的坐標，還有很多其他的方式。除了通過第二個參數指定文本的位置，
還可以通過關鍵字參數設置文本的位置，後面會詳細介紹。::

    screen.draw.text("hello world", (20, 100))

``screen.draw.text`` 有很多可選的位置參數，下面詳細介紹。

設置字體和字號
''''''''''''''''''

pgzero 從你運行的 python 文件所在目錄的 ``fonts`` 目錄載入字體，跟使用圖片和聲音的
方式類似。字體必須是 ``.ttf`` 格式。比如::

    screen.draw.text("hello world", (100, 100), fontname="Viga", fontsize=32)

關鍵字參數:

-  ``fontname``: 顯示文本所用字體的名稱。預設用系統字體。
-  ``fontsize``: 字體大小，預設值是 ``24``.
-  ``antialias``: 是否用反鋸齒模式顯示文本，預設值 ``True``.

顏色和背景色
''''''''''''''''''''''''''

::

    screen.draw.text("hello world", (100, 100), color=(200, 200, 200), background="gray")

關鍵字參數:

-  ``color``: 文本的顏色，預設值是 ``white``.
-  ``background``: 文本的底色，預設是 ``None`` 也就是沒有底色.

``color`` (以及 ``background``, ``ocolor``, ``scolor``, 和
``gcolor``) 可以是類似於 ``(255,127,0)`` 的 (r, g, b) 序列（元組）。也可以
用顏色的名字比如 ``"red"``，亦支持形如 ``"#FF7F00"` 的 HTML 格式的16進位制顏色
字串，或者形如 ``"0xFF7F00"`` 表示十六進位制顏色的字串。

如果 ``background`` 參數值設置為 ``None`` 那麼，文字的底色就是透明的，但是用
 ``screen.draw.text`` 設置文字底色的效率統稱不如用 ``pygame.font.Font.render`` 
 方法。所以如非必要，請勿設置文字的底色。

除了有輪廓和投影的不可見文本外，文字顏色不能指定透明度。 ``alpha`` 參數的介紹
中有更多關於透明色的介紹。

位置
'''''''''''

::

    screen.draw.text("hello world", centery=50, right=300)
    screen.draw.text("hello world", midtop=(400, 0))

關鍵字參數:

::

    top left bottom right
    topleft bottomleft topright bottomright
    midtop midleft midbottom midright
    center centerx centery

位置相關的關鍵字參數的用法與 ``pygame.Rect`` 類似，你可以用兩個參數分別設置
文字（其實是文字的包圍矩形）的位置，也可用用一個元組同時指定兩個位置。
譯者：這裡提供的方法很多，還是需要多練習下，才能夠靈活的組合使用不同的關鍵字
參數指定文字的位置。

如果用了多種方法重複設置了文字的位置，多餘的關鍵字參數一定會被忽律，但是沒有
很明顯的優先度。為了限定文本的位置，``screen.draw.textbox`` 小結中將會詳細
介紹。

單字換行
'''''''''

::

    screen.draw.text("splitting\nlines", (100, 100))
    screen.draw.text("splitting lines", (100, 100), width=60)

關鍵字參數:

-  ``width``: 以像素為單位設置文本繪製的最大寬度，預設是 ``None``，沒限制
-  ``widthem``: 以基於字體的 em 為單位指定文本的最大繪製寬度，預設是 ``None``.
-  ``lineheight``: 行距, 基於字體默認行高.預設值是 ``1.0``，單倍行距。

``screen.draw.text`` 遇到換行符 (``\n``) 會換行，即便是設置了 ``width``
或者 ``widthem`` ，為了確保每行長度小於指定的文本長度也會城市換行。文本長度可能會
超過 ``width``或者 ``widthem`` ，因為換行總是在有空格的地方，如果一個一個
單字很長，不能適應以後的長度，這個長單字不會換行。輪廓和投影文本也是如此。
所以，文本的長度可能會超過給定的寬度。

當然你可以用特殊的 Unicode 字元 (``\u00A0``) 代替空格來組織預設的換行行為。

文本對齊
''''''''''''''

::

    screen.draw.text("hello\nworld", bottomright=(500, 400), align="left")

關鍵字參數:

-  ``align``: 水平對齊，預設為 ``None``.

``align`` 參數的作用，設置繪製多行文本時行與行之間的對齊方式。``align`` 參數
的有效值是字串，``"left"``, ``"center"``, 或者 ``"right"``，0 到 1 之間的
小數或者是 ``None`` 。


如果 ``align`` 參數的值是 ``None`` ，文本對齊方式由其他參數決定，大部分時候可以
滿足你的需求。諸如 (``topleft``, ``centerx``, etc.), ``anchor`` 等參數都可能會
影響對齊方式。建議使用預設對齊方式，如果對齊方式不符合你的預期，就用  ``align`` 
參數明確的指定對齊方式。

文字外輪廓
'''''''

::

    screen.draw.text("hello world", (100, 100), owidth=1, ocolor="blue")

外輪廓參數:

-  ``owidth``: 輪廓寬度，以輪廓寬度為標準，預設是 ``None``.
-  ``ocolor``: 輪廓顏色，預設是 ``"black"``.

如果指定了 ``owidth`` 參數的值，文本將會有外輪廓。如果 ``owidth`` 參數的
值過大，效果看起來可能不會太好。``owidth`` 的單位是經過精心選擇的，``1.0``
是一個典型的比較合適的輪廓寬度只。需要特別注意的是，輪廓寬度的值是字體大小
除以 24 。

如果 ``color`` 設置為透明，比如 (e.g.``(0,0,0,0)``)，同時設定了 ``ocolor``
那麼文字就是只有輪廓的鏤空的文字，這個特性跟 ``gcolor`` 不相容。 ``gcolor``
的有效值跟 ``color`` 的有效值相同。

投影
'''''''''''

::

    screen.draw.text("hello world", (100, 100), shadow=(1.0,1.0), scolor="blue")

關鍵字參數:

-  ``shadow``: (x,y) 投影水平和豎直方向的便宜. 預設是 ``None`` 沒有投影.
-  ``scolor``: 投影的顏色，默認 ``"black"``.

如果指定了 ``shadow`` 參數的值，那麼文本會有一個投影，值必須是只有2個元素的
序列類型（一般是元組），用來指定投影在水平和豎直方向的偏移，值可以是整數，負數。
比如 ``shadow=(1.0,1.0)`` 對應的文本右下角的投影， ``shadow=(0,-1.2)`` 
是文本上方的投影。

投影的單位也是一個經驗值，投影的單位是字體大小除以 18。跟輪廓類似，假設字體
大小是 72，那麼投影的單位就是 72/18=4 。投影和輪廓都是一個相對的值，不是
像素值。

如果 ``color`` 指定的是透明色，指定了 ``shadow``，那麼文本就是中空帶投影的
文本。

投影參數  ``scolor`` 的有效值跟輪廓顏色值和 ``color`` 顏色有效值一樣。

漸變色
''''''''''''''

::

    screen.draw.text("hello world", (100, 100), color="black", gcolor="green")

關鍵字參數:

-  ``gcolor``: 漸變色底端的值，預設是 ``None``.

gpzero 中文本的漸變色是從上到下的線性漸變，頂部是 ``color`` 參數指定的顏色，
底部是 ``gcolor`` 指定的顏色。漸變色的起始和結束點以及漸變方向是寫死的（硬
編碼），無法通過參數設置。

依賴  ``pygame.surfarray`` ，模組，這個模組用到了 numpy 或者 其他數值庫。

顏色透明度
''''''''''''''''''

::

    screen.draw.text("hello world", (100, 100), alpha=0.5)

關鍵字參數:

-  ``alpha``: 0 到 1 之間的透明度值. 預設為 ``1.0``.

In order to maximize reuse of cached transparent surfaces, the value of
``alpha`` is rounded.
為最大限度重用快取的透明度面，``alpha`` 參數進行了四色五入。

依賴  ``pygame.surfarray`` ，模組，這個模組用到了 numpy 或者 其他數值庫。

錨點定位
''''''''''''''''''''

::

    screen.draw.text("hello world", (100, 100), anchor=(0.3,0.7))

關鍵字參數:

-  ``anchor``: 指定了寬度和高度錨點係數的元組，長度為二，默認 ``(0.0, 0.0)``.

``anchor`` 指定了文本相對於指定位置的錨點，如果沒有指定位置相關的關鍵字
參數，``anchor`` 參數的兩個值可以是 ``0.0`` 到 ``1.0`` 之間的任意值。
默認錨點值 ``(0,0)`` ，錨點位於文本的左上角。 ``(1,1)`` 意味著錨點在
文本的右下角。

譯註：如果用 flash 的使用經歷，在 flash 的元件中，需要設置中心點，這個
中心點其實就是錨點。在 flash 中，元件的位置是中心點的位置，旋轉、定位
都是通過這個中心點定位的。

旋轉
''''''''

::

    screen.draw.text("hello world", (100, 100), angle=10)

關鍵字參數:

-  ``angle``: counterclockwise rotation angle in degrees. Defaults to
   ``0``.

旋轉面的定位很棘手。旋轉文本時，錨點的位置固定不變，文本圍繞錨點旋轉。假設一個文本
左上角的坐標是 ``(100, 100)``，旋轉 ``90`` °，那麼 文本所在的 Surface 繪製在左下角
坐標為 ``(100, 100)`` 的位置。

如果覺得不好理解，就把錨點定位在圖形的中心。如果把錨點設置在中心，文本中心保持
不這麼，無論你怎麼旋轉。

In order to maximize reuse of cached rotated surfaces, the value of
``angle`` is rounded to the nearest multiple of 3 degrees.
為了最大程度的利用快取的旋轉的麵，``angle`` 的值會取最近的3的倍數的值。

譯註：旋轉操作很浪費資源，所以會快取旋轉的麵，重複利用，類似於遞迴中用
表查詢法，提高遞迴的效率。假設，你不停的旋轉文字，那麼只需要計算 ``120``
次旋轉的結果，之後每次旋轉過來後，查詢並復用之前的麵 ``surface`` 就行了
無需重新計算，所以效率會很高。這裡屬於最佳化的技巧，pgzero 庫的作者有心。

受限制的文本
''''''''''''''''

::

    screen.draw.textbox("hello world", (100, 100, 200, 50))

``screen.draw.textbox`` requires two arguments: the text to be drawn, and a
``pygame.Rect`` or a ``Rect``-like object to stay within. The font size
will be chosen to be as large as possible while staying within the box.
Other than ``fontsize`` and positional arguments, you can pass all the
same keyword arguments to ``screen.draw.textbox`` as to ``screen.draw.text``.

``screen.draw.textbox`` 需要2個參數：需要繪製的文本以及一個 ``pygame.Rect`` 
或者 ``Rect` 對象。在盒子限定的範圍內，文字會盡可能的大。除了指定位置和字體大小
的參數不能用，``screen.draw.textbox`` 方法的參數跟 ``screen.draw.text`` 參數
的用法相同。

譯註：這裡有點自適應的意思。就是畫個框，往裡面寫文字，類似於 ``ppt`` 中的文本框
文字越多，字體越小。其實這個方法也可以指定一個最小的字體大小，如果計算字號小於
指定的最小字號，那麼文字應該超出這個框。
