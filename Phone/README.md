# 手机端页面制作
## 使用meta标签
    <meta name="viewport" content="width=device-width,initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no" />
>  对于此标签还有以下需要分享：
>  1. user-scalable=no就一定可以保证页面不可以缩放吗？NO，有些浏览器不吃这一套，还有一招就是minimum-scale=1.0, maximum-scale=1.0 最大与最小缩放比例都设为1.0就可以了。
>  2. initial-scale=1.0   初始缩放比例受user-scalable控制吗？不一定，有些浏览器会将user-scalable理解为用户手动缩放，如果user-scalable=no，initial-scale将无法生效。
>  3. 手机页面可以触摸移动，但是如果有需要禁止此操作，就是页面宽度等于屏幕宽度是页面正好适应屏幕才可以保证页面不能移动。
>  4. 如果页面是经过缩小适应屏幕宽度的，会出现一个问题，当文本框被激活（获取焦点）时，页面会放大至原来尺寸。

## reset.css样式表是初始化页面
>  这个样式表对应的设计稿尺寸是宽750px，高1334px。