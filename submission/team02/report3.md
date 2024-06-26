# 第三阶段报告

## 代码框架设计
1- 我们先复制了一份surakarta棋的qt项目，并在此基础上分开构建客户端和服务端。在MainWindow里面我们利用connect函数实现了客户端与服务端的收发信息，并在槽函数里面实现了对应surakarta棋的通信协议。
2-  在网络日志和行棋记录方面，我们用QFile类构造了文件并在客户端和服务端收发消息时同时往文件里记录信息内容和行棋过程。

## 联网逻辑
这一阶段我们成功搭建了服务端和客户端的连接，实现了server和client之间信号的发送以及棋盘的同步更新。
其中，我们完善了receive_from_client, send_to_another_client, receive_from_server等函数以及在收到信息后更新棋盘的操作。

## AI接入
我们修改了第一阶段的AI，使用了贪心算法并将AI打包到函数之中，在客户端接收到来自服务端的move_op后通过函数调用AI并进行下一步行棋动作。

## 日志系统
分别在server和client以指定格式创建txt文件，并记录每一次消息传送。每次产生send或者receive动作时，data都会被记录。
行棋记录在每一次收到或是发送move_op的时候添加，并在judge_end的时候记录游戏结束原因，同样传入另外一个txt文件中。
两个文件都与project在平行的文件夹存放。

## 游戏逻辑完善
1- 我们在服务端增加了not_player_turn的判定，实现了当玩家不在自己的轮数移动棋子的时候视为不合法动作并结束游戏。
2- 我们修改了Timer逻辑，在server端进行计时并在超时的时候给两个client发送超时信息。
3- 我们修改了认输按钮按下后的判定，保证在己方轮数时认输会导致server发送结束信息，而在对方轮数时会被忽视。

## 遇到的困难
1-校园网的IP地址似乎会随着时间和空间的变化而改变，并且可能存在网络连接不顺畅的情况。
2- 一开始我们没有使用wait_for_connection，因此一直没有连上网络，但后续发现了是没有调用该函数导致服务端还没有接收到信息就自动判定客户端没有发送。
3- 我们发现最终debug后形成的文件只在其中一台电脑上能够成功运行，但目前还没有发现原因。
4- 由于我们第二阶段重新构建了一条行棋逻辑，导致第三阶段无法再使用第一阶段原有的函数，所以在第三阶段又重新完成了一次end，illegal_move等判定内容。

## 实验分工：
张宇衡：分析联网逻辑，游戏AI，debug，命令行输入
郑子姗：完成了客户端收到信息之后相应的处理函数，服务端收发信息的函数和行棋日志
蔡乐宜：完成了在联网逻辑基础上发送MOVE_OP和END_OP以及判断游戏结束，共同完成行棋日志
