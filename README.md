# Automated Academic Paper Info Scraper

## Introduction
这个项目的目的是完成一定程度的自动化的论文一些关键信息的爬取，包括：“作者”，“文章名称”，“期刊/会议名称”，“类别”，“是否SCI/EI”，“年份” 这几个论文相关信息，下面是一个例子：
| 作者                                                            | 文章名称                                                                | 期刊/会议名称                 | 类别      | 是否SCI/EI | 年份 |
| --------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------- | --------- | ---------- | ---- |
| Yuan Gao, Xiang Wang, Xiangnan He, Huamin Feng, Yong-Dong Zhang | Rumor detection with self-supervised learning on texts and social graph | Frontiers of Computer Science | 中科院3区 | SCI,EI     | 2023 |

- “作者”， “文章名称”， “年份”在dblp的页面上都有直接可以爬取的信息，处理起来也比较简单，这里不详细介绍。
- “期刊/会议名称”：另外在dblp的搜索结果中包含了每一篇论文的发表会议/期刊的缩写，比如上面例子的期刊在dblp中的缩写就是fcsc，以后简称缩写，“期刊/会议名称”则是可以通过用缩写构建dblp对应会议/期刊的页面的url，之后爬取dblp里面的全名，这个全名不一定是中科院或者CCF的名字，可以适当的做异常处理，你可以修改manually_add_confjour_full_name_and_level.ipynb里面的代码来实现你想要的结果。
- “类别”： 类别就是这个期刊/会议属于什么评级标准，期刊按照中科院的标准来，会议则是CCF的标准，本次任务涉及的会议和期刊并不多，所以我是手动查询CCF给的pdf和中科院的网站之后放到conferences_level.txt和journals_level.txt里面，你也可以自己手动改一下，来适配你的应用场景，当然如果你写了一个全自动爬虫版也可以分享到你的github中（期待）
- “是否SCI/EI” 是在 [Web of Science](https://webofscience.clarivate.cn/wos/alldb/basic-search) 和  [Engineering Village - Quick Search](https://www.engineeringvillage.com/search/quick.url)中查询对应的论文名字根据前k个数据的返回结果匹配论文名称，如果匹配则说明被SCI或者EI收录。我个人认为这个是最费时间的一步，因此本次github的代码最主要的也是实现了用selenium进行模拟使用这两个网站查找是否被收录的过程。为了简便起见，我运行代码是在校园网的环境下的，所以不需要处理登录的问题，如果你的工作场景需要登录，那么请参考一下网络上的博客解决一下吧（），或者模仿一下我代码里面的点击搜索的过程，把登录页面的信息输入进去（哈哈）。

## Files Description
- main.ipynb
主程序，完成了dblp的爬取和SCI和EI是否收录的判定
你可以修改第二和第三个代码块的代码来构建自己的paper_list，本项目只需要url_dblp页面下2023年的论文所以写出的代码是第二和第三块代码的风格
- manually_add_confjour_full_name_and_level.ipynb
根据dblp爬取的会议/期刊的缩写abbr属性去自动获取dblp中对应的全称和读取手动定义的CCF和中科院的会议/期刊评级
- journals_level.txt
参考 [范围认证 - 中国科学院文献情报中心期刊分区表升级版](http://advanced.fenqubiao.com/Macro/Journal?name=%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6&year=2022)，手动定义的期刊等级
- conferences_level.txt
参考CCF2022给出的会议登记pdf来手动定义的会议等级

## Concluding Remarks
愿天堂没有杂活:pray: