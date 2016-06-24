# SCSS结构说明

![](http://i.imgur.com/MwjV7jc.png)


### scss 说明
- mixin & include (一些常用结构可以套用，使用频率一个页面正常在5个以内的结构和样式)
- %placeholder & extend (避免mixin方式生成过多冗余样式代码，正常特指一个页面或者一个功能使用频率相当高的)
- class & extend (不推荐)

### 样式规范
- 不使用驼峰式命名方式，全部用单独加 "-" 方式命名；
- 常用功能样式命名；
````css
// [mini, tiny, small, medium, large, big, huge, massive]
````
- 常用排序命名；
````css
// i: 1, ii: 2, iii: 3, iv: 4, v: 5, vi: 6, vii: 7, viii: 8, ix: 9, x: 10
````
- 常用状态命名；
````css
// disabled selected hover
````



### Fonts

- 字体文件统一放置在common根目录下的fonts文件夹中；
	- 字体实现全部使用content方式，IE67浏览器使用背景图方式实现;
	- 禁止在html中直接使用字体图标实体编码;
````css
.add {
    &:after, &::after {
        content: '';
    }
}

````
````css
// IE6-7.hack.css
.add {
	background-image: url();
	background-position: 0, 0;
}
````
- 字体文件分类分文件夹管理；
- 每个文件夹中有各自独立的_font.scss用于管理自身字体相关版本引用及默认字体样式；
````css
@font-face {
    font-family: 'Roboto-Regular';
    src: url('roboto_regular_macroman/Roboto-Regular-webfont.eot');
    src: url('roboto_regular_macroman/Roboto-Regular-webfont.eot?#iefix') format('embedded-opentype'),
    url('roboto_regular_macroman/Roboto-Regular-webfont.woff') format('woff'),
    url('roboto_regular_macroman/Roboto-Regular-webfont.ttf') format('truetype'),
    url('roboto_regular_macroman/Roboto-Regular-webfont.svg#RobotoRegular') format('svg');
    font-weight: normal;
    font-style: normal;
}

@font-face {
    font-family: 'Roboto-Bold';
    src: url('roboto_bold_macroman/Roboto-Bold-webfont.eot');
    src: url('roboto_bold_macroman/Roboto-Bold-webfont.eot?#iefix') format('embedded-opentype'),
    url('roboto_bold_macroman/Roboto-Bold-webfont.woff') format('woff'),
    url('roboto_bold_macroman/Roboto-Bold-webfont.ttf') format('truetype'),
    url('roboto_bold_macroman/Roboto-Bold-webfont.svg#RobotoBold') format('svg');
    font-weight: bold;
    font-style: normal;
}
````


### common-scss

    全局共用scss代码，主要放置代码类型如下：

- 全局共用结构 assets (如表单Dom嵌套)；

````css
$form-margin-bottom: 12px; // application 可进行覆盖

@mixin test-form {
    margin:0;
    padding: 0;

    .form-item {
        margin-bottom: $form-margin-bottom;
    }
}
````

- 全局的基础样式 base;
	1. _font.scss 基础字体文件，提供字体初始化参数，及相关字体图标的默认参数；
	2. _normalize.scss, _reset.scss
	3. _var 定义全局scss共用变量 多而全，便于后期扩展和覆盖；
- 常用布局结构 mixin, 下面分类型写，便于维护，举例:

````css
/**
 * 图片垂直居中
 */
@mixin image_center {
    display: table-cell;
    vertical-align: middle;
    text-align: center;
}
````

- 公用软连接 symbolic, 基于基础的mixin并结合大家平时使用习惯，增加此文件，便于过度，举例：

````css
@import "../mixin/center";
@import "../mixin/layout";

.split {
    @include layout_line('li', 14px);
}

.image-center {
    @include image_center;
}
````

### application

各引入common中的共用结构，并自身存在全局文件，可对common进行覆盖重写；

- assets公用结构, 举例对common中的进行覆盖;
````css
@import "../../scss/assets/form";

//override

$form-margin-bottom: 2rem;
````

- base;
	- _var.scss 重新定义全局变量的值，修改为当前应用需要的；
	- _font.scss 修改全局字体图标大小；


- page 业务代码相关，只要处理一些页面上的各自具体布局；
````css
@import "../base/base";
@import "../assets/form";


body {
    .ob-form {
        @include form;
        width:200px;
        font-family: 'Roboto-Bold';
    }

    .at-form {
        @include form;
        width:300px;
        font-family: 'Roboto-Regular';
    }
}
````