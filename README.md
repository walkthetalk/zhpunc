
ConTeXt中文标点支持插件，支持常用的标点压缩/标点挤压方案，行端标点对齐，以及其他一些功能补强；兼容竖排[vtypeset](https://github.com/Fusyong/vertical-typesetting)插件。

zhpunc项目是对[zhfonts](https://github.com/Fusyong/zhfonts)项目的重构，而二者都基于[liyanrui/zhfonts项目](https://github.com/liyanrui/zhfonts)，谨致谢忱。

## 安装和使用方法

* 两种安装方法：
    1. 按[ConTeXt官方指南](https://wiki.contextgarden.net/Modules)安装模块文件：`t-zhpunc.mkiv`（入口）和`t-zhpunc.lua`，然后使用`context --generate`命令更新文件索引
    1. 将上述文件直接放在编译时的当前路径（通常即排版脚本所在的目录，在vscode环境中即项目根目录）；直接使用lua模块时不限定存放位置，但需要自行确保导入位置正确
* 两种使用方法，在排版脚本前言中分别设置如下：

```latex
%%%%%%%%%%%%% 通过入口文件使用 %%%%%%%%%%%%%
% pattern: quanjiao(全角，default), kaiming(开明), banjiao(半角),
%          yuanyang(原样), hangjian(行间，暂不可用)
% spacequad: 0.5(default)

\usemodule[zhpunc][pattern=kaiming, spacequad=0.5]
```

```lua
%%%%%%%%%%%%% 直接使用lua模块 %%%%%%%%%%%%%
\startluacode
---[[
    local zhpunc = require("./t-zhpunc.lua")
    zhpunc.set("kaiming", "0.5") --设置
    zhpunc.append() --挂载
--]]
\stopluacode
```

可参考test文件夹下样例脚本中的设置（可能使用了夹注[jiazhu](https://github.com/Fusyong/jiazhu)、竖排[vtypeset](https://github.com/Fusyong/vertical-typesetting)、标点挤压[zhpunc](https://github.com/Fusyong/zhpunc)三个模块）。

### 编译脚本

1. 仅在[ConTeXt LMTX](https://wiki.contextgarden.net/Installation)环境测试，其他版本的ConTeXt当不支持。ConTeXt LMTX是与LuaMetaTeX(LuaTeX的后继者)配合使用的、最新的ConTeXt版本。调整后当可用于LuaTeX。可以使用`context --version && luametatex --version`命令查看你的环境版本。
1. 如下编译排版脚本：
    >```shell
    >> context 大学章句.lmtx
    >```
1. 如果控制台显示中文时有乱码，可用命令临时改变代码页：
    >```shell
    >> chcp 65001
    >```

## 效果

![plot](https://blog.xiiigame.com/img/2022-02-15-ConTeXt-LMTX中文竖排插件/vtypesetting_callback_1.jpg)

![plot](https://blog.xiiigame.com/img/2022-02-15-ConTeXt-LMTX中文竖排插件/vtypesetting_callback_2.jpg)

## TODO & bugs

1. [ ] 全部缓存、使用实测的标点字模数据，而不是使用预设的比例，以适应不同字体的实际；
    1. [x] 缓存标点数据
    1. [x] 用于标点压缩
    1. [ ] 用于行首尾对齐版心
    1. [ ] 用于盒子/夹注首尾对齐版心（**待定**，对窄盒子不利）
1. [ ] 变“全角标点压缩/挤压”为“半角标点加空”，预制常见的标点加空规则：
    1. [x] 全角（默认）
    1. [x] 半角
    1. [x] 开明
    1. [x] 原样
    1. [ ] 行间
1. [x] 用户可自定加空宽度（默认0.5角）
1. [x] 支持[直排模块](https://github.com/Fusyong/vertical-typesetting)
1. [x] 模块化
1. [ ] 拓展、校正或覆盖系统的`hanzi`脚本的行为（**待定**）
    1. [x] 修正两侧收缩胶收缩幅度为实有空白，以防重叠
    1. [ ] 据条件修正两侧拉伸胶的幅度
1. [ ] 允许用户扩展符号类别（**待定**），如：
    * ☆★○●◎◇◆□
    * ㈠㈡㈢⑴⑵⑶⒈⒉⒊ⅠⅡⅢ……

## 符号分类与定义

| 《标点符号用法》     |              |                | 重定义   |                  |          |                                                                 | 配置加空     |              |      |      |      |
| -------------------- | ------------ | -------------- | -------- | ---------------- | -------- | --------------------------------------------------------------- | ------------ | ------------ | ---- | ---- | ---- |
| 分类                 | 独用占位(角) | 形式           | 补充符号 | 语义单元中的位置 | 禁排     | 备注                                                            | 开明（默认） | 全角（国标） | 半角 | 原样^**^ | 行间 |
| 句末点号             | 1            | 。？！         | ．       | 后               | 行首禁排 | 忽略`？！` `双用占一，三用占二`的规定，重制统一的标点组占位规则 | 后0.5        | 后0.5        |      |      |      |
| 句内点号             | 1            | ，、；：       |          | 后               | 行首禁排 |                                                                 |              | 后0.5        |      |      |      |
| 后半标号             | 1            | ”’）］〕】》〉 | 」』〗｝ | 后               | 行首禁排 |                                                                 |              | 后0.5        |      |      |      |
| 前半标号             | 1            | “‘（［〔【《〈 | 「『〖｛ | 前               | 行尾禁排 |                                                                 |              | 前0.5        |      |      |      |
| 省略号               | 2            | `……`           |          | 中^*^            |          | 作为整体                                                        |              |              |      |      |      |
| 破折号（中后标号）   | 2            | `——`           |          | 中^*^            |          | 作为整体                                                        |              |              |      |      |      |
| 全角连接号（中标号） | 1            | —～            |          | 中               | 行首禁排 |                                                                 |              |              |      |      |      |
| 半角连间分（中标号） | 0.5          | -·/            | ／       | 中               | 行首禁排 | `/`由`首尾禁排`改为`行首禁排`                                   |              |              |      |      |      |

^*^ 实际上，省略号、破折号可出现在语义单元的前、中、后各处，但字模占位已经足够宽，两侧不再加空，与“连间分”地位相当。  
^**^ 原样：仅处理不合规的标点（不处理字体本身的问题），如破折号不连续。

## 标点占位/加空（挤压、压缩）规则

* **标点字模的占位**
    * **所有字模均预先调小占位，且居中放置。** 除横势标点（`……` `——` `—` `～` `-`）按《标点符号用法》规定占位，**其余标点一律占0.5角（quad，下同）。** 原因是：
        * 字模占位窄对断行算法、行尾标点对齐有利；
        * 居中可简化连用时的定位；
* **标点组与加空**
    * 各类标点加空配置见上表中的“配置加空”列；
    * 语义位置相同的相邻几个标点为一个标点组。如`”〉》`为一个后标点组；
    * 前标点组，根据最前一个标点，在组前加空。如：
        * 全角配置：`囗囗␣《〈“囗囗`
        * 开明配置：`囗囗《〈“囗囗`
    * 后标点组，根据最后一个标点，在组后加空。如
        * 全角配置：`囗囗”〉》␣囗囗` `囗囗！〉》␣囗囗` `囗囗〉》！␣囗囗` `囗囗？！␣囗` `囗囗？！！␣囗`
        * 开明配置：`囗囗”〉》囗囗` `囗囗！〉》囗囗` `囗囗〉》！␣囗囗` `囗囗？！␣囗` `囗囗？！！␣囗`
    * 中标点组（通常是单个），根据最前一个标点，在组前加空；根据最后一个标点，在组后加空。如：
        * 全角配置：`囗囗·囗囗` `囗囗/囗囗` `囗囗～囗囗` `囗囗——囗囗`
        * 开明配置：`囗囗·囗囗` `囗囗/囗囗` `囗囗～囗囗` `囗囗——囗囗`
    * 同一个位置（如前标点组与后标点组之间）只加空一次（取最大值）。如：
        * 全角配置：`囗囗”〉》␣《〈“囗囗` `囗囗”〉》！␣《〈“囗囗`  `囗囗”〉》，␣《〈“囗囗`
        * 开明设置：`囗囗”〉》《〈“囗囗` `囗囗”〉》！␣《〈“囗囗`  `囗囗”〉》，《〈“囗囗`
    * 中标点组，前后有标点组时，直接附着与前后的标点组，其间不加空。如：
        * 全角配置：`囗囗”·“囗囗` `囗囗”·囗囗` `囗囗·“囗囗`
        * 开明设置：`囗囗”·“囗囗` `囗囗”·囗囗` `囗囗·“囗囗`
* 默认标准空（固定宽度的胶）占位0.5，**可通过设置比例统一调整实际宽度。**
* 标点前后加扩展胶，与系统给汉字后加扩展胶的规则一致。

## 行首行尾禁排规则

只有行首禁排（禁止符号前断行）、行尾禁排（禁止符号后断行）、不禁排三种（前后可断行）。见上表“禁排”列。
尽量通过官方的scrp-cjk.lua实现。

## 行首行尾对齐/凸排/悬挂规则

一律对齐字母实际边界与预设文字边界（盒子边界再刨除缩进）。
尽量通过官方模块实现。

## 调整标点前后伸缩胶

尽量通过官方的scrp-cjk.lua实现。

