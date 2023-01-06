在IDLE和其他編輯器運行Pygame Zero
==========================================

我們通常用如下命令運行Pygame Zero::

    pgzrun my_program.py

在特定的程序,比如像IDLE這樣的集成環境,以及Edublokcs中,會運行 ``python`` 而不是 ``pgzrun`` 命令.

但是呢,Pygame Zero已經很貼心的包含了編寫可以通過 ``python`` 命令運行python程序的
方式, 為了能夠用 ``python`` 運行程序,你只需要輸入 ::

    import pgzrun

作為程序的第一行, 然後輸入 ::

    pgzrun.go()

當做程序的最後一行.


例子
-------

下面是用Pygame Zero畫圓的程序. 你可以把代碼複製到IDLE運行.


    import pgzrun


    WIDTH = 800
    HEIGHT = 600

    def draw():
        screen.clear()
        screen.draw.circle((400, 300), 30, 'white')


    pgzrun.go()
