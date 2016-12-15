---
  title: 解决linux：vim的退格删除键不能用
  tags:
    - Linux
    - Linux问题集
  categories: Linux问题宝典
  permalink: linux/vim-problem-one
---

## 问题：linux下vim的退格删除键不能使用

## 解决方案:
~/.vimrc中加入
set backspace=indent,eol,start
即可，测试时需要注销重新登录

或者进入vim后执行:set backspace=indent,eol,start
可直接生效

>摘自:http://blog.zhukunqian.com/?p=314	对作者表示感谢，如有侵权，请联系删除。