===== 伺服器上，安裝套件 =====

$ sudo apt-get install vnc4server xfce4 xfce4-goodies firefox


===== 伺服器上，準備基本的 Xfce VNC 設定檔 =====

$ mkdir -p ~/.vnc
$ tee ~/.vnc/xstartup <<'END'
#!/bin/sh
xrdb $HOME/.Xresources
startxfce4 &
END
$ chmod +x ~/.vnc/xstartup


===== 伺服器上，打開 VNC server 並且指定解析度 =====

第一次使用，要設定一個登入你的 VNC session 專用的密碼。
任何知道這個密碼且可以連到你的 VNC server (TCP) 的人，都可以存取你的桌面。

$ vnc4server -localhost -nolisten tcp -geometry 1280x720

如果你的本地端螢幕夠大，你可以設定更高的解析度

這個指令成功執行以後，會顯示 display number 像是 :1 或 :2 或 :3 之類的

預設情況下

    display :1 corresponds to TCP port 5901
    display :2 corresponds to TCP port 5902
    display :3 corresponds to TCP port 5903

也可以用以下指令去做確認，看看是不是有程序已經打開 TCP server 了

    $ ls ~/.vnc/*.pid
    $ sudo netstat -nlptu

以下假設我們的 VNC display server 回報的是 :1 (即 TCP port 5901)


===== 在本地端使用 SSH 建立單條 TCP 通道的 proxy =====

$ ssh yourname@yourserver -N -L 1234:127.0.0.1:5901

See SSH(1) manual for more information
The -L and -R options of ssh are awesome


===== 現在你可以連到本地端 port 1234 的 VNC server 了 =====

使用任何你喜歡的 vnc client

在 Ubuntu 上，你可以用 xvnc4viewer 這個 app：

    $ sudo apt-get install xvnc4viewer
    $ xvnc4viewer 127.0.0.1:1234

在 macOS 上，你可以用內建的 Screen Sharing 這個 app：

    $ open vnc://127.0.0.1:1234

在 Windows 上，你可以用 TigerVNC (以及 PuTTY 的 port forwarding 功能)

    https://github.com/TigerVNC/tigervnc
    https://bintray.com/tigervnc/stable/tigervnc/1.8.0
    vncviewer-1.8.0.exe
        127.0.0.1:1234


===== 伺服器上，當你不再需要這個 VNC server session 時，可以把它殺掉 =====

$ vnc4server -kill :1


===== XFCE 重點 =====

第一次進入 XFCE 桌面會問要用什麼設定，請選擇「Use default config」

如果發現 TAB 鍵不 work 應該是 Super 鍵失效所導致的
可以去 Applications -> Settings -> Window Manager 打開 Keyboard 分頁
預設的 Switch window for same application 是 Super+Tab 請把它 Clear 掉！


===== XFCE 其他使用筆記 =====

經過實測，在 xfce4-terminal 下可以正常輸入 128 個 7-bit ASCII 字元
雖然沒有 Super 鍵，已經很堪用了

用快捷鍵 Alt+F2 或 Alt+F3 可以搜尋、開啟應用程式

可以去 Applications -> Settings -> Keyboard
在 Application Shortcuts 設定用 Ctrl+Alt+T 開啟 xfce4-terminal

如果希望 Terminal 不要有 Alt+F 等快捷鍵
去 Edit -> Preferences -> Advanced ->
把 Disable all menu access keys (such as Alt+f) 勾起來

如果希望修改 Terminal 背景色、前景色、十六色
去 Edit -> Preferences -> Colors 做適當的設定
或者可以選擇預先定義好的 Presets
例如 "Dark Pastels" 或 "Solarized (light)" 或 "Black on White"
