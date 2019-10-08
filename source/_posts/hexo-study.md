---
title: hexo_study
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-10 15:29:04
tags:
- hexo
- blog
categories:
- blog
---


sudo apt-get install nodejs
sudo apt-get install node

Node.js: 在服务器端的javascript
npm: 软件管理包  cnpm

# 目录

1. npm install -g hexo-cli 安装客户端
{% asset_img 1.jpg hello world%}
2. 建站
mkdir `<folder>`
cd `<folder>`
hexo init
npm install


![](1.jpg)

3. 目录结构
    - _config.sym
        - 网站
            title, subtitle, description, keywords, author
        - 网址 
            url, root, permalink,  permalink_defaults
            > permalinks: :category/:title
        - 目录
            - source_dir(source): 资源文件夹，存放内容
            - public_dir(public): 公共文件夹，存放生成的站点文件
            - tag_dir: 标签文件夹
            - archive_dir: 归档文件夹
            - category_dir: 分类文件夹
            - code_dir: downloads/code: source_dir下的子目录
            - i18n_dir
            - skip_render: 跳过指定文件的渲染
        - 文件
            - new_post_name
            - post_asset_folder
                > 总共有两种方式 
                > 1. 放在source/images, ![](/images/image.jpg)
                > 2. post_asset_folder: true
            - render_drafts?： 显示草稿
            - demo:
                > 
        - 分类&标签
            - default_category: 默认分类
        - 日期、时间显示
        - 分页
            - per_page: 4好像就合适
            - pagination_dir: page  分页目录???
        - 扩展
            - theme, theme_config
            - deploy: 部署部分的设置
            - meta_generator: meta generator标签。? 好像是html头部的一些东西
        - 包括或不包括目录和文件 glob表达式
            - include: 默认忽略git文件夹，是ok的
            - exclude:
        - hexo server --config custom.yml自定义配置未见的目录
        - 在主题下进行自我配置theme/my-theme/_config.yml
    - package.json: 不用管，应用程序的信息；可以查看总共添加了哪些依赖
    - scaffolds模板文件:   hexo new photo "filename"
        > 注意这里photo.md存储在scaffolds下
        > 默认为post文件
    - source: 存放用户资源的地方，处理_post文件，其他_和.开头的文件会被忽略，markdown和html文件会被解析到public, 而其他文件会被拷贝过去
    - themes: 主题文件夹，根据主题来生成静态页面
        - _config.yml
        - languages
        - layout: 布局文件夹，用来存放主题的模板文件，决定网站内容的呈现方式。hexo采用的Swig模板
            - index: 首页
            - post
            - page
            - archive
            - category
            - tag
        - scripts: 脚本问价加
        - source

4. 命令
    - init:
    - new:  使用default_layout参数
        - -p, --path: 自定义新文章的路径, 决定文章文件的路径 !!! 测试 a
            > new about/me "About me"     me.md  title:"About me"
        - -r, --replace 同名文章替换
        - -s, --slug 新文章的文件名和发布后的url
    - generate g
        - -d, --deploy
        - -w, --watch
        - -f, --force 重新生成文件
    - publish [layout] `<filename>` 发布草稿
    - server
        - -p, --port: 重设端口
        - -s, --static: 只使用静态文件
        - -l, --log: 启动日记记录
    - deploy -g,--generate
    - render 渲染文件 -o 输出路径
    - migrate 迁移
    - clean: 清除缓存文件db.json, 以及已经生成的静态文件public
    - list: 列出网站资料
    - version

> 应用
> 1. 一键所有部署: hexo g -f -d
> 2. 模式：  --safe 安全模式，不会载入插件和脚本?  --debug 到debug.log  --slient 隐藏终端信息 --draft 显示文件夹中的草稿文章 --cwd 自定义当前工作目录的路径


5. 文章
    - 布局 layout
        - false: 不要处理我的文章
        - page: source ? 好像只是路径不同，所以是路径问题?
        - post source/_posts
        - draft source/_drafts  --draft 预览?
    - title, date, updated, comments, tags, categories, keywords
        > 分类具有顺序性和层次性，标签没有顺序性和层次
        > -[Diary, Linux] -[PS3, ]

6. 标签插件
    - 引用块 quote: 
        > {% blockquote %} {% endblockquote %}
    - 代码块 ocde
        > {% codeblock %} {% endcodeblock %}
    - 反引号代码块
        > ```[langage][title][utl][link text] code snippet``
    - 其他
        - youtube视频  {% youtube video_id %} ? 如何用
        - 插入Vimeo视频 {% vimeo video_id %}
        - 引用文章 {}
        - 引用资源
            > 引用文件，图片，链接等， 注意这里是特殊的格式
            ```
            {% asset_path slug %}
            {% asset_img slug [title] %}
            {% asset_link slug [title] %}
            ```
        - 插入swig标签  {% raw %} {% endraw %}
        - 摘要 `<!-- more -->`

7. 数据文件夹 hexo3.0新增功能  source/_data: 可以在网站中复用这些东西， 比如在menu.yml中配置的信息可以通过代码解析

8. 服务器
    - npm install hexo-server --save
    - hexo server -p 5000 -i ip

9. 生成器
    - hexo generate --watch查看变动

10. 部署
    - 可以部署多个deploy:
        ```
        deploy:
        - type:
          repo:
        - type:
          repo
        ```
    - git
        - npm install hexo-deployer-git --save
        - 参数
            - type: git
            - repo: 地址
            - branch: 
            - meassage: 自动提交消息
        - 原理
            > 当hexo deploy时，hexo会将public目录中的文件和目录推送到_config.yml对应的远端仓库，并且完全覆盖giants分支下的已有内容

11. 变量
    - 全局变量
        - site 网站变量
            - posts
            - pages ??
            - categories
            - tags
        - page: 针对该页面的内容
            - title
            - date
            - 文章 posts
                - published
                - categories
                - tags
            - 首页 index
                - per_page
                - total
                - current
            - 归档 archive
                - archive
                - year
                - month
            - category分类，与index布局相同
                - category
            - 标签 tag:
                - tag
        - config: 网站配置
        - theme: 主题配置
        - _ : loadsh函数库
        - path: 当前页面的路径（不含根路径）
        - url: 当前页面的完整网址
        - env: 环境变量

12. 辅助函数
    - <%- image_tag(path, [options]) %> 插入图片
        - alt,  class, id, width, height
        > CDN图片引用访问 Cloudinary
    - gravatar 全球唯一关联邮箱图片
    - toc
    

13. 插件
    - 脚本  scripts
    - 插件  node_modules  hexo-XX
        - 官方


mathjax: true
reward: false 
password:

Front-matter


 npm install hexo-generator-searchdb --save


问题：
1. About页面没有
2. 对应的编辑器没有
3. 侧边栏不好看


https://blog.winsky.wang/Hexo%E5%8D%9A%E5%AE%A2/Hexo%E5%8D%9A%E5%AE%A2Next%E4%B8%BB%E9%A2%98%E9%85%8D%E7%BD%AE/

todo

https://theme-next.org/docs/getting-started/


todo
SEO工具
获取token

https://www.baidufree.com/1873.html

https://hui-wang.info/2016/10/23/Hexo%E6%8F%92%E4%BB%B6%E4%B9%8B%E7%99%BE%E5%BA%A6%E4%B8%BB%E5%8A%A8%E6%8F%90%E4%BA%A4%E9%93%BE%E6%8E%A5/

https://zhuanlan.zhihu.com/p/38143946
优化和维护暂时还不可，2-5天
https://www.jianshu.com/p/f8ec422ebd52

谷歌站点地图
https://search.google.com/search-console?resource_id=https%3A%2F%2Faugf.github.io%2F


