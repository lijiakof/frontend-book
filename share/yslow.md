# 23条 Web 性能最佳实践和规则
以下规则是来自 YSlow 团队对 Web 性能总结出来的规则。随着 Web 前端技术的发展以及新技术的不断涌现，我们可以想想是不是还有其它的优化点或者这23条又一些以及不满足现状。
1 . 尽可能减少HTTP请求次数
2 . 使用CDN
3. 避免空src和href标签
4. 加入Expires或Cache-Control Header
5. 使用Gzip压缩
6. 在html文件顶部放置样式表
7. 在html文件底部放置JavaScript脚本
8. 避免使用CSS表达式
9. 使用外部JavaScript和CSS外部文件
10. 减少使用DNS查找次数
11. 精简JavaScript和CSS
12. 避免重定向
13. 移除重复的脚本
14. 配置ETag
15. 缓存AJAX
16. 使用GET完成AJAX请求
17. 减少DOM元素数量
18. 避免404
19. 减少Cookie大小
20. 使用无Cookie的域
21. 避免使用滤镜
22. 不要在HTML中缩放图片
23. 使用小favicon.ico文件，并让其可缓存