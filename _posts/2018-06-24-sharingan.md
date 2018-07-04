--- 
layout: post
title: 写轮眼（sharingan)奇淫技巧
tags: 
- sharingan
- slides
status: publish
type: post
published: true
---

> 本片文章直接拷贝于部门wiki，作者还有renwanfeng、duyalei

写轮眼是谢益辉开发的，利用markdown语法完成的slides的[工具](https://github.com/yihui/xaringan)， 排版非常考究，书写速度极快。非常适合工程师序列的工作人员。

不过因为GFW的存在，sharingan 调用的js和font不能加载，会导致slides不能被顺利渲染。 这里提供一些常用参数设置，通过本地编译，sharingan能够提供 self-contained 文件，不依赖于网络环境。

# 全局配置

## slides的title 部分

```markdown
title: "少儿英语启蒙路线产品"
subtitle: ""
author: "刘思喆"
date: '`r Sys.Date()`'
output:
  xaringan::moon_reader:
    css: [default, zh-CN.css]
    lib_dir: libs
    chakra: remark.min.js
    self_contained: true    
    nature:
      highlightStyle: github
      highlightLines: true
      countIncrementalSlides: false
```

- css 中增加一个自定义 zh-CN.css，这里同时声明了字体路径
- lib_dir: 本地libs的目录
- chakra 使用本地 js
- self_contained 变更为 true

所有本地依赖文件的[打包地址](/upload/sharingan.zip)

## 字体设置

css 部分指向了本地的woff字体地址：

```css
@font-face {
  font-family: 'Yanone Kaffeesatz';
  font-style: normal;
  font-weight: 400;
  src: local('Yanone Kaffeesatz Regular'), local('YanoneKaffeesatz-Regular'),
       url('woff/yanone-kaffeesatz.woff') format('woff');
}
@font-face {
  font-family: 'Droid Serif';
  font-style: normal;
  font-weight: 400;
  src: local('Droid Serif'), local('DroidSerif'),
       url('woff/droid-serif.woff') format('woff');
}
@font-face {
  font-family: 'Source Code Pro';
  font-style: normal;
  font-weight: 400;
  src: local('Source Code Pro'), local('SourceCodePro-Regular'),
       url('woff/source-code-pro.woff') format('woff');
}
```

# 其他实用技巧

## 演示相关

### 实时预览

- 利用xaringan的infinite moon reader插件实现实时预览（在Viewer中所见即所得），在R console中输入即可

  ```
  xaringan:::inf_mr("your_file.Rmd")
  ```

### 演讲者模式

演讲时候经常需要使用演讲者模式，这样可以自己看到上下页面演讲内容，同时也可以看到备注和相应的时间提示：

- 首先在编译出来的html文件中按C键复制到新窗口，新窗口可以给观众看
- 其次在本地的这个html演示文稿中点击p进入到演示模式即可

这样就实现了本地滑动演示的时候，新窗口同步滚动

### 自动播放幻灯片

有时候你可能对时间把控不是很准确的时候，可以锻炼自己每页ppt 1分钟演讲时间，这时候，可以设置一个幻灯片倒计时60s：

```
  output:
        xaringan::moon_reader:
          nature:
            countdown: 60000
```

或者如果你的演讲精彩到忘记用鼠标，那么可以用自动播放的功能，比如30s换一页？

```
  output:
    xaringan::moon_reader:
      nature:
        autoplay: 30000
```

## slides内容编辑

### 页面风格控制

写轮眼利用remark.js采取class的方式来定义整页幻灯片的风格，需要注意的是这个是父级的，直接控制整个页面，而如果页面需要单独对每个版块进行单独控制的，最好不要直接在用全页面的class属性，比如我们经常在演讲的时候用到的衔接页面，跟其他的页面背景色可以采取不一样的，这样让观众知道我们要讲下一个part了，这时可以采取如下的页面设置：

```
  class:inverse,middle,center

  # 这是一个过渡页面
```

以上展现效果是黑色背景页，白字居页面正中。

### 设计节选的主标题

演讲的时候一个slides可能分为几部分，而其中的一部分有一个主标题，比如谈"追求理解的教学设计"，那么针对这个大标题有三个小标题，这种情况在开始这一part的时候可以设定一个`layout:true`，只要没有遇到`layout:false`之前的所有页面节标标题都是跟这个设置页面一样，比如以下的某一页面设置了节标题：

```
  ---
  layout:true

  # 这是一个节标题
```

### 图片编辑

图片不建议使用 base64 内嵌，会导致加载速度很慢。参见[这里](https://yihui.name/en/2017/08/why-xaringan-remark-js/)，但如果量小也是可以的，使用方法如下

```
background-image: url(`r knitr::image_uri('incomeHeatmap.png')`)
```


图片分为背景图、左右悬浮图或者页面标记（比如51talk公司logo）,不同的场景可以用不同实践方法

- 对于背景图全覆盖可以采用`background-size: contain`，不设定位置；对于logo标志可以采用如下code；这种设定适合长宽一样的情况下设定

  ```
    background-image: url('figure/51talklogo.png')
    background-size: 100px
    background-position: 90% 5%
  ```

- 对于左右悬浮图可以采用自定义的CSS, 定义图片宽和高占页面大小的百分比以及悬浮方式在左边还是右边

  ```
    # 在zh-CN.css中添加如下自定义css
    .img-right{
      width: 50%;
      height: 50%;
      float: right;
    }
    #在Rmd中使用
    .img-right[![](figure/example.jpg)]
  ```

- 对于一些特殊的比例控制也可以采用自定义的css。比如宽度90%，高度50%的图片

  ```
  # css start
  .image-9050 img {
  height: 50%;
  width: 90%;
  }
  ## css end
  .image-9050[![](figure-html/jiandan.gif)]   # 自定义大小,在css中定义`.image-9050`
  ```

### 公式编辑

- 公式

  ```
  $$x=1$$
  ```

## 页面辅助功能

- 添加脚注：比如有的参考文献或者相关补充提示

  ```
  # Rmd中添加上在相应的位置加上<sup>*</sup>

  .footnote[[*] 这是一个脚注]
  ```

- 添加备注：每一页像ppt一样增加演讲的备注，??? 以下的代表备注，图片也可以加

  ```
  ???
  this is a test note
  ```

- 自增式slides演示：通过鼠标点击出现下一个内容,类似ppt的滑动效果，采用`--`来控制效果，注意标识符后空一行否则滑动未生效；区分三横杠`---`、二横杠`--`和`-`，分别代表幻灯片分割，自增和列表的语法

  ```
  --

  上面是一个空行
  ```
