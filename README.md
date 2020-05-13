# scrapy_video
爬取b站番剧的评分等信息，学习爬虫的案例，仅供学习参考，不可商用。<br>
可以直接运行，mian里面flag来改变需要爬取和保存的内容，可以自定义一次存入量，path等根据自己需要进行修改。
### 明确问题：

    明确要解决的问题：番剧推荐，番剧的产量 
    =>明确需要的数据：剧番id、标题、标签、长评数、短评数、年份、视频链接；用户id，剧番id，评分，评分时间

### 确定验证方案
  
     1）数据获取：爬虫，b站番剧
     2）数据处理
     3）数据展示和分析
     4）推荐算法+预测算法

### 设计预测模型
        
     1）新用户：基于内容的推荐
     2）老用户：基于协同过滤的推荐
     3）根据类型和时间进行新番的评分预测

### 实施流程：

    1、获取数据：爬虫；
    2、了解数据：数据展示和处理(统计：一共有多少番剧，一共有多少用户进行评分；
    
    在爬虫过程中，已经进行的数据处理有：
        1）对重复爬取的link进行去重；
        2）对评论时间的处理，把只有月日、“昨天”，“x小时前”，“x分钟前”这样的非标准日期进行转化，变成年月日的日期形式；
        3）对于不存在评论的番剧已经进行跳过处理，不会爬取；

    获得爬虫数据之后需要的数据处理：
        1）对于还有可能的存在是数据重复进行处理

    爬虫存在的问题：    
        1）网络问题，会导致一些内容没有爬取到，而没存入csv中；    
        2）可能有些特殊情况，比如评论时间的问题等，格式不一致或者其他情况，导致未爬取成功。    
    这些情况导致的数据缺失问题，由一开始的基数进行弥补，即为了获取较多的数据，一开始选取的番剧数量也足够多。

爬虫遇到的问题&解决方法：

 出现的问题  | 猜测原因  | 解决方法
 ---- | ----- | ------  
  用request获取页面code无法获取真正的内容  | 页面是js动态加载的 | 借助selenium库，利用模拟浏览器模式打开网页获取内容<br/>```chrome_options = webdriver.ChromeOptions()<br/>#使用headless无界面浏览器模式```<br/>```chrome_options.add_argument('--headless') # 增加无界面选项```<br/>```chrome_options.add_argument('--disable-gpu') #如果不加这个选项，有时定位会出现问题```<br/>```#启动浏览器```<br/>```driver =webdriver.Chrome(options=chrome_options)``` <br/>```driver.get(url)```<br/>```content = driver.page_source```<br/>```soup = BeautifulSoup(content, 'lxml')```<br/>```driver.close()```<br/> 
 突然访问失败，网页浏览也是  | ip被限制 | 设置随机等待时间，<br/>```rand_seconds = random.choice([1, 3]) + random.random()```<br/>  ```sleep(rand_seconds) ```|
 重复爬取第一页内容 | 反爬机制 | 在更改url获取页面code的时候，	<br/>使用 driver.refresh()进行页面刷新 

剩余内容查看‘B站番剧评分数据分析.ipynb’

