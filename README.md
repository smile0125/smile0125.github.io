# 简写

- hexo n "我的博客" == hexo new "我的博客" #新建文章
- hexo p == hexo publish
- hexo c == 清除缓存
- hexo s == hexo server #启动服务预览
- hexo g == hexo generate#生成
- hexo d == hexo deploy#部署

# 服务器

- hexo server #Hexo 会监视文件变动并自动更新，您无须重启服务器。
- hexo server -s #静态模式
- hexo server -p 5000 #更改端口
- hexo server -i 192.168.1.1 #自定义 IP
- hexo clean #清除缓存 网页正常情况下可以忽略此条命令
- hexo g #生成静态网页
- hexo d #开始部署

# 监视文件变动

- hexo generate #使用 Hexo 生成静态文件快速而且简单
- hexo generate --watch #监视文件变动

# 个人常用

- hexo c && hexo s
- hexo c && hexo g && hexo d
