===== 安裝套件 =====

$ sudo apt-get install vnc4server xfce4 xfce4-goodies


===== 準備基本的 Xfce VNC 設定檔 =====

$ mkdir -p ~/.vnc
$ printf '%s\n%s\n%s\n' '#!/bin/sh' 'xrdb $HOME/.Xresources' 'startxfce4 &' > ~/.vnc/xstartup
$ chmod +x ~/.vnc/xstartup


===== 在 display :1 打開 VNC server 並且指定解析度 =====

第一次使用要設定一個登入你的 VNC session 專用的密碼
任何知道這個密碼的人，只要可以連到你的 VNC server (TCP) 就可以打開你的桌面...  所以防火牆很關鍵...

$ vncserver -localhost -nolisten tcp -geometry 1280x720 :1


如果你的本地端螢幕夠大，你可以設定更高的解析度


===== 確認 TCP server 已經打開 (例如 port 5901) =====

$ sudo netstat -nlptu

應該會看到有個 Xvnc4 程序 (running with your user id) 正在聽一個 TCP port


===== SSH proxy =====

$ ssh your-user-name@yourdomain.example.com -NTL127.0.0.1:5901:127.0.0.1:5901

See SSH(1) manual for more information
The -L and -R options of ssh are awesome


===== 現在你可以連到本地端 port 5901 的 VNC server 了 =====

使用任何你喜歡的 vnc client，以 macOS 為例你可以用內建的 Screen Sharing 這個 app：

$ open vnc://127.0.0.1:5901


===== 當你不再需要這個 VNC server session 時，可以把它殺掉 =====

$ vncserver -kill :1


===== XFCE 使用筆記 =====

第一次進入 XFCE 桌面會問要用什麼設定，請選擇「Use default config」

如果發現 TAB 鍵不 work 很可能是 Super 鍵失效所導致的
可以去 Applications -> Settings -> Window Manager 打開 Keyboard 分頁
預設的 Switch window for same application 是 Super+Tab 請把它 Clear 掉！


===== TODO =====

在 Ubuntu 16.04 和 Windows 10 上面推薦用什麼 vnc client?
