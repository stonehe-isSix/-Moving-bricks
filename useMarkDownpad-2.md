# 测试 
## 欢迎使用 MarkdownPad 2 ##
##目录
* [目录1](#_2)
	 + [目录1_1](#_3)
* [目录2](#_4)
	 + [目录2_2](#_5)

**MarkdownPad** 是 Windows 平台上一个功能完善的 Markdown 编辑器。
<span id="_2"></span>
### 专为 Markdown 打造 ###

提供了语法高亮和方便的快捷键功能，给您最好的 Markdown 编写体验。

来试一下：

- **粗体** (`Ctrl+B`) and *斜体* (`Ctrl+I`)
- 引用 (`Ctrl+Q`)
- 代码块 (`Ctrl+K`)
- 标题 1, 2, 3 (`Ctrl+1`, `Ctrl+2`, `Ctrl+3`)
- 列表 (`Ctrl+U` and `Ctrl+Shift+O`)

1. 有序标题
2. 无序标题 

### 实时预览，所见即所得 ###
<span id="_3"></span>
无需猜测您的 [语法](http://markdownpad.com) 是否正确；每当您敲击键盘，实时预览功能都会立刻准确呈现出文档的显示效果。

### 自由定制 ###
<span id="_4"></span>

100% 可自定义的字体、配色、布局和样式，让您可以将 MarkdownPad 配置的得心应手。

### 为高级用户而设计的稳定的 Markdown 编辑器 ###
 
 MarkdownPad 支持多种 Markdown 解析引擎，包括 标准 Markdown 、 Markdown 扩展 (包括表格支持) 以及 GitHub 风格 Markdown 。
 
 有了标签式多文档界面、PDF 导出、内置的图片上传工具、会话管理、拼写检查、自动保存、语法高亮以及内置的 CSS 管理器，您可以随心所欲地使用 MarkdownPad。

[baidu](http://baidu.com)
<span id="_5"></span>
![text](https://ooo.0o0.ooo/2017/03/17/58cbb44bd3f63.jpg)

    
    <?php defined('SYSPATH') or die('No direct script access.');
        
         * 让welcome继承Init
         */
        class Controller_Welcome extends Controller_Init{
        /**
         * 定义action,名为index
         * URL；
         * www.XXXX.com/welcome/index.html
         * @return HTML
         *
         */
        public function action_index(){
            //加载视图模板
            //模板存放在views/welcome/index.php
            $this->template = View::factory('welcome/index');
        }

> 这里是引用 

注意这个是引用的使用规则

| 表格-标题1 | 表格-标题2 |
|------------|------------|
|one         |tow         |
|1           |2           |
