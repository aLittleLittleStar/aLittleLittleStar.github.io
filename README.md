<p style="text-align: center;font-size: 60px;font-weight: bold;">Hexo</p> 
## README
博客所用框架: Hexo <br>
主题： Tranquilpeak <br>
__将gitbash部署hexo到github__<br>
```bash
# 清除记录
hexo clean
# 编译
hexo generate
# 运行
hexo server
# 提交github
hexo deploy

```

### 博客基本构架搭建

标签： CSS3、缓存、ES6、JS、
> 
> 设置文章索引,文章头部添加 <!-- toc -->
> 
> 设置首页文章为收起状态，在想要显示的段落后面添加
> 
> <!-- more --> 定义帖子摘录并将帖子摘录保留在帖子内容中
> <!-- excerpt --> 定义帖子摘录并删除帖子内容的帖子摘录
> 
> npm install --save hexo-tag-aplayer

```bash
NexT 主题默认已经集成了文章【字数统计】、【阅读时长】统计功能，
如果我们需要使用，只需要在主题配置文件 _config.yml 中打开 wordcount 统计功能即可。如下所示：
# Post wordcount display settings
# Dependencies: https://github.com/willin/hexo-wordcount
post_wordcount:
  item_text: true
  wordcount: true         # 单篇 字数统计
  min2read: true          # 单篇 阅读时长
  totalcount: false       # 网站 字数统计
  separated_meta: true

如果没有安装 hexo-wordcount 插件，先安装该插件：


npm i --save hexo-wordcount
```