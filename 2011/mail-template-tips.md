# 邮件模板注意事项
- date: 2011-01-01
- category: work
- tags: html, css

---------------

公司年底的时候要给用户们发送祝福邮件，邮件要好看当然少不了模板，于是安排我来写这个模板。

邮件模板为了兼容性好得用table布局来写，写完这个模板才深刻体会到div的便利。列举一下过程中遇到的一些问题，算是分享与备忘。

1. tr th td img 的margin/padding/line-height/font-size都可能会引起边距，为了覆盖掉不同邮箱的样式，最好都加上 !important 。
2. 样式最好写成内联的，有的邮箱会过滤掉 style 标签。
3. font-family 要声明，否则在mac下显示的中文字体为宋体。
4. 有的邮件客户端不支持背景图，尽可能使用img来显示图片。
5. 背景图片引用线上的绝对地址，背景图片代码写法为<table background=”background.gif”></table>。
6. 若想邮件居中给table加上align=”center” 但是有的邮箱不支持居中。