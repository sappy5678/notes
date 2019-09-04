---
title: Linux 開機自啟動 Jupyter
categories: [Ubuntu,開機自啟]
tags: [Ubuntu,開機自啟,Jupyter]
date: 2018-03-07 18:43:51
---
# 環境

* Ubuntu 16.04 桌面版
* Anaconda 3.6

# 方法

1. 用管理員權限使用 emacs[^1] 打開 /etc/rc.local 
    
    ```bash
    sudo emacs /etc/rc.local
    ```
2. 加入下面兩行(請隨自己的設定改變)
    說明
    * $user_name 是指自己的使用者名稱
    * su $user_name 是要以該使用者的身分執行
    * /home/$user_name/anaconda3/envs/DL3/bin/jupyter 則是你執行 jupyter 的位置，這裡是以 ubuntu 下的 anaconda 為例
    * lab 跟 notebook 分別是執行 [jupyter lab](https://github.com/jupyterlab/jupyterlab) 跟 [jupyter notebook](http://jupyter.org/)，看個人需要使用
    ```bash
    su $user_name -c "/home/$user_name/anaconda3/envs/DL3/bin/jupyter lab --no-browser;" &
    su $user_name -c "/home/$user_name/anaconda3/envs/DL3/bin/jupyter notebook --port 8889 --no-browser;" &
    ```

    所以我是
    ```bash
    su sappy -c "/home/sappy/anaconda3/envs/DL3/bin/jupyter lab --no-browser;" &
    su sappy -c "/home/sappy/anaconda3/envs/DL3/bin/jupyter notebook --port 8889 --no-browser;" &
    ```
3. 可以使用下面的指令來協助除錯，詳細可以看[這篇文章](http://linux.vbird.org/linux_basic/0510osloader.php#rc.local)
    ```bash
    systemctl status rc-local.service
    ```

# 結論

rc.local 雖然已經過時了，但是......能用就好 😁 :grin:

# 參考

* [鳥哥的 Linux 私房菜](http://linux.vbird.org/linux_basic/0510osloader.php)
* [server - Run Jupyter-notebook on boot on Ubuntu - Stack Overflow](https://stackoverflow.com/questions/44231789/run-jupyter-notebook-on-boot-on-ubuntu)

[^1]: 一種編輯器