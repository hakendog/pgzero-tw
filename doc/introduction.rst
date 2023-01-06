Pygame Zero教學
===========================

.. highlight:: python
    :linenothreshold: 5

創建窗口
-----------------

首先，新建一個python空白文件，並保存為  ``intro.py``。

運行一下命令來驗證文件可以正常運行並創建一個窗口 ::

    pgzrun intro.py


Pygame Zero所有設置都是默認可選，所以一個空的文件也是一個合法可以運行的Pygame Zero腳本！
好神奇有沒有！


單擊窗口的關閉按鈕或者按 ``Ctrl+Q`` ( ``⌘-Q`` on Mac)快捷鍵退出遊戲。如果遊戲卡住了，
你可以在終端窗口按 ``Ctrl+C`` 快捷鍵。

繪製窗口的背景
--------------------


然後，讓我們添加一個 :func:`draw` 函數並且設置窗口的大小。每當需要刷新(重繪)窗口的時候，
Pygame Zero就會調用這個函數。

在 ``intro.py``,文件添加以下代碼::

    WIDTH = 300
    HEIGHT = 300

    def draw():
        screen.fill((128, 0, 0))

重新運行 ``pgzrun intro.py`` 腳本，遊戲窗口變成了紅色的正方形。

這段代碼的作用是什麼？

``WIDTH`` 以及 ``HEIGHT`` 指明了窗口的寬和高. 
這段代碼把窗口設置為300x300大小。

``screen`` 是內建的代表窗口顯示的類. screen類有
:ref:`很多負責繪製精靈和圖形的函數<screen>`. 
調用 ``screen.fill()`` 方法可以用指定一個顏色元組
``(red, green, blue)`` ，然後用純色填充窗口. ``(128, 0, 0)`` 是暗紅色. 試著
改變rgb顏色值，然後查看代碼運行效果。

接下來讓我們設置一個我們生成動畫的精靈：


繪製一個精靈
-------------


在我們繪製任意東東之前，我們需要一個外星人alien精靈圖片。你可以右鍵單擊這張圖，然後選擇
圖片另存為。

.. image:: _static/alien.png


這個精靈圖片是支持透明色的png圖片，非常適合在遊戲裡使用。這張圖片為深色背景設計，所以
只有當你程序運行的時候才能夠看到外星人的頭盔。

.. tip::

    你可以在 `kenney.nl
    <https://kenney.nl/assets?q=2d>`_ 網站找到包括這張圖在內的大量免費精靈圖片. 這張
    圖是 `Platformer Art Deluxe pack
    <https://kenney.nl/assets/platformer-art-deluxe>`_ 的一部分.


只有將圖片保存在正確的路徑Pygame Zero才能夠載入圖片。新建一個 ``images`` 目錄，並且把
圖片另存為 ``alien.png`` 。文件夾和檔案名都是小寫，雖然windows不區分檔案名的大小寫，但是
linux和OSX是區分，不然就會陷入一個跨平台相容性的陷阱。


新建完圖片並且另存圖片之後，你的項目如下：

.. code-block:: none

    .
    ├── images/
    │   └── alien.png
    └── intro.py


``images/`` 目錄是Pygame Zero查找代碼中圖片的標準默認位置。


內建類 :class:`Actor` 用來代表一個你繪製到螢幕的圖形。

讓我們來定義一個在螢幕上顯示的圖形，修改 ``intro.py`` 文件載入圖片::

    alien = Actor('alien')
    alien.pos = 100, 56

    WIDTH = 500
    HEIGHT = alien.height + 20

    def draw():
        screen.clear()
        alien.draw()


哇塞，外星人顯示在螢幕上了。透過把字串 ``'alien'`` 作為參數傳遞給 ``Actor`` 類，Pygame
Zero自動載入了外星人精靈，並且圖片具有位置和大小屬性。這樣我們就可以根據外星人alien的告訴
設置窗口的高度屬性 ``HEIGHT`` 。 ``alien.draw()`` 方法把外星人精靈繪製到螢幕上的當前位置。

移動外星人
----------------

我們先讓外星人在舞台的外面; 修改 ``alien.pos`` 一行程式碼如下::

    alien.topright = 0, 10

Note how you can assign to ``topright`` to move the alien actor by its
top-right corner. 注意修改``topright``屬性來相對於右上角來修改外星人角色位置
的方法。如果外星人角色的右邊橫坐標為``0``, 外星人角色恰好在螢幕的左側. 然後，我們
讓外星人角色動起來。在文件底部添加以下代碼::

    def update():
        alien.left += 2
        if alien.left > WIDTH:
            alien.right = 0

Pygame Zero在每一幀都會調用 :func:`update` 函數。透過在每一幀讓外星人移動很小的像質數，
外星人就會在螢幕上從左向右滑過。一旦外星人左側的坐標大於窗口的寬度，就讓外星人回到左側
重新向右滑動。

處理滑鼠單擊事件
---------------------------
接下來，我們讓遊戲在單擊滑鼠的時候，做點不一樣的東西。為了實現這個目標我們需要定義
一個 :func:`on_mouse_down` 函數。在文件下方添加以下代碼::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            print("Eek!")
        else:
            print("You missed me!")


運行遊戲，並嘗試多次單擊外星人角色。

Pygame Zero可以非常聰明的處理你對於函數的調用。如果你定義的函數沒有 ``pos`` 參數，Pygame
在調用函數的時候就不會傳遞位置參數。``on_mouse_down`` 方法還有一個 ``button`` 按鈕參數，
代表單擊的滑鼠的那個鍵。因此我們也可以這樣定義 ``on_mouse_down`` 函數::

    def on_mouse_down():
        print("You clicked!")

或者::

    def on_mouse_down(pos, button):
        if button == mouse.LEFT and alien.collidepoint(pos):
            print("Eek!")


聲音和圖像
-----------------


接下來我們讓外星人表現受傷的造型，保存一下文件:

* `alien_hurt.png <_static/alien_hurt.png>`_ -保存圖片 ``alien_hurt.png``
  到 ``images`` 目錄.
* `eep.wav <_static/eep.wav>`_ - 新建一個叫做 ``sounds`` 目錄，然後保存 ``eep.wav`` 到聲音目錄。

這時候項目如下圖所示:

.. code-block:: none

    .
    ├── images/
    │   └── alien.png
    │   └── alien_hurt.png
    ├── sounds/
    │   └── eep.wav
    └── intro.py

``sounds/`` 是Pygame Zero查找聲音文件的默認標準目錄。
現在讓我們用新的圖片和聲音資源改寫  ``on_mouse_down`` 函數::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            sounds.eep.play()
            alien.image = 'alien_hurt'

當你單擊外星人的時候，你會聽到一段聲音，精靈也會切換到不開心的外星人。

但是這個遊戲還有一個bug，那就是被單擊後外星人不會回到開心的造型，但是每次單擊的
時候，聲音會播放。接下來讓我們改掉這個bug。


時鐘函數
---------------------------
如果出了遊戲編程之外你對python非常熟悉，你就會知道用  ``time.sleep()`` 來插入延時。
你可以像下面這樣寫程式碼::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            sounds.eep.play()
            alien.image = 'alien_hurt'
            time.sleep(1)
            alien.image = 'alien'

但是不行的是，在遊戲中這樣寫是不合適的。 ``time.sleep()`` 阻塞了所有的活動。我們希望
遊戲能夠繼續運行和播放動畫。實際上我們需要從  ``on_mouse_down`` 返回，然後讓遊戲在切換
外星人的造型之後還能夠繼續運行，讓  ``draw()`` 和  ``update()`` 繼續跑。

這可難不倒Pygame Zero，因為我們有一個內建的 :class:`Clock` ，可以讓函數延時執行。

首先，讓我們重構也就是從新寫程式碼。我們定一個設置外星人手上和返回普通造型的函數::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            set_alien_hurt()


    def set_alien_hurt():
        alien.image = 'alien_hurt'
        sounds.eep.play()


    def set_alien_normal():
        alien.image = 'alien'

運行程式碼跟之前沒什麼區別  ``set_alien_normal()``  並沒有被調用。但是我們可以用時鐘類
修改 ``set_alien_hurt()`` 方法，這樣  ``set_alien_normal()``  就可以延遲一段時間被調用了::

    def set_alien_hurt():
        alien.image = 'alien_hurt'
        sounds.eep.play()
        clock.schedule_unique(set_alien_normal, 1.0)

``clock.schedule_unique()``  可以讓  ``set_alien_normal()`` 方法在
 ``1.0`` 秒後被調用. ``schedule_unique()`` 同時防止同一函數在快速單擊的時候被多次安排調用.

嘗試一下，你會發現外星人alien在1s後恢復正常形態。嘗試快速單擊外星人，驗證外星人只有在最後
單擊的1s之後才會恢復。


總結
-------

我們已經學習如何繪製精靈，播放聲音，處理輸入時間，以及使用內建
的時鐘類。

也許你繼續完善遊戲，可以記錄遊戲的得分，或者讓外星人alien移動的更加詭異。

有許多特性讓Pygame Zero易於使用。訪問  :doc:`內建對象<builtins>`  學習如何使用其他API。
網易少兒編程教研組提供翻譯。歡迎訪問 `網易卡搭 <https://kada.163.com>`_  以及 `網易極客戰記 <https://codecombat.163.com>`_
