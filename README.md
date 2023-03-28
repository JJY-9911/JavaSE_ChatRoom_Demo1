# JavaSE_ChatRoom_Demo1
多线程，Socket通信，IO流，网络编程，数据库，可注册登陆的多人聊天群。

服务器：
RoomerServer：获取服务器IP，创建ServerSocket，实例化ServerFrame。
SeverFrame：打开服务器管理界面，开启死循环监听Socket请求。每监听到一个请求就加入到Socket集合，针对每个Socket开启一个消息处理线程和一个下线监听线程。消息处理线程用死循环监听该用户的消息，用迭代器遍历在线用户，将消息分发给每个人。下线监听线程用死循环监听用户下线动作，若下线则移出集合。

客户端：
Client：包含内部类LoginFrame和Register。先打开LoginFrame，获取输入的用户名密码，调用数据库验证。若点击注册，则打开注册界面，获取注册用户名密码，在数据库中验证存在性。

ChatFrame：若登陆成功，则打开聊天界面。添加按钮监听和键盘监听，可以使用“发送”键和回车来发送消息。获取用户IO流，read来自服务器分发的消息，append到聊天框；write输入框的text给服务器进行分发。

Dao：
jdbc:mysql://192.168.0.52/chat?&useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true
用户IP和服务器IP是不同的。服务器IP是192.168.0.52，数据库在服务器所在机器，所以要在末尾写allowPublicKeyRetrieval=true，并将数据库的mysql库中user表里user为root的host改成%，来允许客户端连接数据库，才能在用户机器上调用服务器数据库验证登陆。读者需将Dao中此处修改成自己服务器的IP。


Util介绍：
ConnTest：添加数据库驱动jar包后，可运行ConnTest测试数据库是否能驱动。
ServerPort：存放的是服务器IP和程序端口。
KeyCode：在ChatFrame中，有回车键发消息的功能，可以运行此方法获取回车键键值。


PS:
1.读者请创建数据库，库名chat，字段id,username,password，id主键，自增。
2.注意检查两处数据库驱动语句中的密码和IP，密码是作者数据库的。作者服务器IP写在Util.ServerPort中，Client建立Socket时使用了此IP，注意修改成自己的IP来指向自己的数据库。


问题记录：
1.无论是用户还是服务器，IO流都是从Socket获取的，ServerSocket不存在IO流。
2.try-catch内声明的变量都是局部变量，所以要在块的外部声明。
3.线程常常是内部类，要考虑每个线程的功能，调用什么变量，针对性的在外部类中声明该变量。
4.服务器的线程常常使用死循环来保持监听。



改进：
用线程同步来避免多地登陆同一账号
Mybatis
规范使用MVC框架

