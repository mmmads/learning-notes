# sublime markdown preview

[参考链接1](https://github.com/mmmads/README) Github Favored Markdwn  
[参考链接2](https://blog.csdn.net/qazxswed807/article/details/51235792) 使用Sublime写markdown  
[参考链接3](https://www.cnblogs.com/linxd/p/4955530.html) Mathjax与LaTex公式简介  

## sublime markdown preview
首先安装Package Control  
设置了mdp快捷键：`alt+m`

## 用MathJax书写公式
设置MathJax支持需要在`Preferences ->Package Settings->Markdown Preview->Setting User`中增加如下代码：
``` 
{
    /*
        Enable or not mathjax support.
    */
    "enable_mathjax": true,

    /*
        Enable or not highlight.js support for syntax highlighting.
    */
    "enable_highlight": true,
}
```
