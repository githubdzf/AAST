# AAST

核心是paran/黑幕大佬的py脚本。
原型是wangziyingwen/酷安id-卷腿毛菌的项目


### 特别说明/Thanks ###
* refresh_token：下载rclone，文件夹内CMD
* 示例：rclone authorize "onedrive" "f82c748c-a719-4a85-a84a-7bd23a6b5711" "CxA~M-kL9_05lbT~5xWRDO-Y4Oc.y8b.MS"（不建议使用）：https://github.com/wangziyingwen/GetAutoApiToken
* 视频教程：（我操作很慢，自行倍速/快进,Secret版的教程，将就看吧）
   * 在线/下载地址：https://kino-onemanager.herokuapp.com/Video/AutoApi%E6%95%99%E7%A8%8B.mp4?preview
   * B站：https://www.bilibili.com/video/av95688306/
           


### 步骤 ###
* 第一步，获取应用id、机密、refresh_token 

* 第二步，建项目、设置action
  

  
    **赋予api权限的时候，选择以下几个**
  
                Calendars.ReadWrite、Contacts.ReadWrite、Directory.ReadWrite.All、
                
                Files.ReadWrite.All、MailboxSettings.Read、Mail.ReadWrite、
                
                Notes.ReadWrite.All、People.Read.All、Sites.ReadWrite.All、
                
                Tasks.ReadWrite、User.ReadWrite.All、User.Read

  
  在你电脑上新建多个txt文本，例如你有两个账号，则账号 0 对应为 0.txt , 账号 1 对应为 1.txt , 以此类推。(只有一个账号，则只需一个0.txt，一定要从0开始数)
  
  再把各个账号对应的refresh_token粘贴进对应的txt文件。
  
   > refresh_token位置如图下。复制refresh_token紧接着的双引号里的内容（红竖线框起来的），不要把双引号复制进去。复制进txt后，留意结尾不要留空格或者空行
     
  
   
  
  再然后把你项目token文件夹里的文件全删掉（记得点commint确认删除），再把你的0.txt...n.txt上传到token文件夹下。

* 第三步，依次点击上栏 Setting > Secrets > Add a new secret，新建两个secret如图：ID_LIST、KEY_LIST 。

  内容分别如下: ( 把你的应用id改成你的应用id , 你的应用机密改成你的机密，单引号不要动 )
  
  (同样以此类推，直接复制增加；如果只需一个账号，则 id_list = [r'账号0应用id'] )
  
  **注意所有符号均是英文条件下的符合**
  
  ID_LIST
  ```shell
  id_list = [r'账号0应用id',r'账号n应用id']
  ```
  KEY_LIST
  ```shell
  secret_list = [r'账号0应用机密',r'账号n应用机密']
  ```

  
  
  最终格式应该是类似这样的：
  
 
  
  
* 第四步，修改参数配置
  
  你项目的testapi.py文件第15行有个config_list
  
       各参数说明：
         * 每次轮数：每启动一次运行多少轮api调用，一轮调用10个api
         
         * 是否启动随机时间：每一轮结束隔“多久”才开始下一轮调用，这个“多久”会根据后面的参数随机生成
         
         * 延时范围起始，结束：例如设置600跟1200，则“多久”会在600到1200秒这个范围随机生成一个数，到时间开启下一轮调用
         
         * 是否开启随机api顺序:根据一定规则从28个api抽13个随机排序，我设置的是30天换一次顺序。不开启则默认原教程10个api。
         
         * 是否开启各api延时：就是每个api调用要不要停一下才开始下一个api调用。（个人建议不开）
           
           同样有范围，例如：分延时范围开始跟分结束分别设置为10，20.则会在10到20秒这个范围随机生成一个数，然后调用下一个api
           
         * 是否开启备用应用：更换应用id调用api。同样每30天更换一次应用id。（目前只至支持1个副应用）
         
           开启后，需在设置的secret再增加两条：
           ID_LIST2
           内容为： id_list2=[r'帐号1副应用id',r'帐号n副应用id']

           KEY_LIST2
           内容为： secret_list2=[r'帐号1副应用机密',r'帐号n副应用机密']
           
           然后类似的在backuptoken文件夹里放入对应的副应用的0.txt....n.txt。
           
           （这里看不懂的话，直接选N吧）
           
           （由于延时需要长时间运行，测试的时候建议把随机、延时都关了，迅速运行完看看看情况，再更改喜欢的配置）
  
* 第五步，进入你的个人设置页面(右上角头像 Settings，不是仓库里的 Settings)，选择 Developer settings > Personal access tokens > Generate new token,


  

  设置名字为GITHUB_TOKEN , 然后勾选 repo , admin:repo_hook , workflow 等选项，最后点击Generate token即可。
  

  
  
* 第五步，点击右上角星星/star立马调用一次，再点击上面的Action就能看到每次的运行日志，看看运行状况

（必需点进去Test Api看下，api有没有调用到位，有没有出错。外面的Auto Api打勾只能说明运行是正常的，我们还需要确认10个api调用成功了，就像图里的一样。如果少了几个api，要么是注册应用的时候赋予api权限没弄好；要么是没登录激活onedrive，登录激活一下）


  

* 第六步，没出错的话，就搞定啦！！再看看下面的定时次数要不要修改，不打算改就不用管了。

  我设定的每天9、13、16点自动运行一次（点击右上角星星/star也可以立马调用一次），你们自行斟酌修改（我也不知道保持活跃要调用多少次、多久）：

  * 定时自动启动修改地方：（在.github/workflow/autoapi.yml文件里，自行百度cron定时任务格式，最短每5分钟一次）
   

  
  
------------------------------------------------------------
### 题外话 ###
> Api调用
  你们可以自己去graph浏览器看一下，学着自己修改要调用什么api(最重要的是调用outlook、onedrive)
  https://developer.microsoft.com/zh-CN/graph/graph-explorer/preview

### GithubAction介绍 ###
提供的虚拟环境：

· 2-core CPU
· 7 GB RAM 内存
· 14 GB SSD 硬盘空间

使用限制：
* 每个仓库只能同时支持20个 workflow 并行。
* 每小时可以调用1000次 GitHub API 。
* 每个 job 最多可以执行6个小时。
* 免费版的用户最大支持20个 job 并发执行，macOS 最大只支持5个。
* 私有仓库每月累计使用时间为2000分钟，超过后$ 0.008/分钟，公共仓库则无限制。

（我们这里用的公共仓库，按理，你们可以设定无限循环调用，然后6小时启动一次，保证24小时全天候调用）

参照感谢黑幕/paran大佬&wangziyingwen/酷安id-卷腿毛菌
