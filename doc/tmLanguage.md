## tmLanguage

### 基本属性

name[选填]：语法名称。在某些ide里会当做名称放在列表菜单里。

scopeName[必填]：作用域名称。又称语法的顶级scope名称。可以在syntax文件中随处引用。命名方式有source.<lang_name>或者text.<lang_name>。前者一般用于程序语言，后者一般用于标记性的语言。比如javascript的scopeName为source.js，html的scopeName为text.html.basic。

patterns[必填]：语法正文，定义模式。因为模式中的内容通常较长，因此，这部分较为常见的做法是在patterns中include许多分类好的模式，而模式的具体定义则放在repository中完成

repository[选填]：仓库。用来放所有可以被include的东西。直接插入使用，可减少代码重复开发。用于储存子scope，类似于变量的存在，挂在顶级scope名下。

schema：作为json的注释而存在

### 语法正文

syntax使用正则表达式匹配语法，具体内容见学习资料中的Regular Expression。

patterns有两种配置：

一、完整匹配：

Fine Tuning Matches 

```
patterns:
  - name: keyword.other.ssraw
    match: \$([A-Za-z][A-Za-z0-9_]+)
    captures:
      '1': {name: constant.numeric.ssraw}
```

name[可选]：规则名称，是作为语法堆栈解析上的名称

match：匹配规则

captures：正则表达式分组对应的名称，会作为语法堆栈解析上的名称，分组从1开始

二、begin-end匹配：

 Begin-End Rules

```
patterns:
  - name: string.other.ssraw
    begin: (\$)(\{)([0-9]+):
    beginCaptures:
      '1': {name: keyword.other.ssraw}
      '3': {name: constant.numeric.ssraw}
    end: \}
    patterns:
      - include: 'source.js#string'
      - include: '#command-set'
      - name: support.other.ssraw
        match: .
```

begin：开始边界

beginCaptures：对应begin正则表达式分组对应的名称，同captures的规则

end：结束边界

endCaptures：对应end正则表达式分组对应的名称，同captures的规则

patterns：同开头的patterns

include：引入其他scope。当引用repository当中的scope时，需要加上#，顶级scope不需要这个符号。比如source.js#string，意思是引入source.js(javascript)下repository中的string(字符串语法规则)。

### scope仓库

Repository：

repository与patterns类似，区别在于需要以scope名称(也就是key名)开头。

~~~
# Repository Rules
---
repository:
  # set
  command-set:
    name: meta.namespace.set
    begin: (set)(:)
    beginCaptures:
      '1': { name: keyword.commands.set }
      '2': { name: punctuation.separator.key-value.csr }
    end: (?=\s*\?>)
    patterns:
      - include: '#expression'
  # name
  command-name:
    name: meta.namespace.name
    match: (name)(:)([a-zA-Z0-9._]+)
    captures:
      '1': { name: storage.type.name }
      '2': { name: punctuation.separator.key-value.csr }
      '3': { name: variable.other.readwrite }
~~~



### scope命名

vscode当中，不同语法模块显示的颜色不一样，具有不同的样式。这里的这些命名可以理解为某些特定样式的class下的属性。

比如storage.type有一个预设样式为"foreground": "#569cd6"，那么storage.type.function.js就会命中这个样式，显示#569cd6这个颜色。vscode皮肤插件就是针对这些class设计不同的样式，至于怎么使用这些样式，就是我们的问题了。

同时根据这些命名不难看出，针对大部分语法场景已经有一套成熟的使用场景。

在captures当中，比如单引号用string.quoted.single，比较运算符用keyword.operator.relational。

在name当中，一般以meta开头命名代表语法块，比如if语法块叫做meta.tag.if，方法里面的参数叫做meta.function.parameters。

想要参考已有的语法设计结构，可以在vscode编辑器区域当中用组合键:cmd(win)+shift+p，选择"Developer: Inspect TM Scopes"，会弹出一个语法结构的浮层，显示当前光标悬浮位置的语法信息。

命名规则推荐可参考学习资料中的Scope Name



### 参考示例

预期想要达到的结果

- abc三个词高亮显示
- 括号当中的x高亮显示
- 括号当中的abc高亮显示

按照思路，解析应该分为以下步骤：

1. 找到代码中的a、b和c
2. 找到开括号标识，去找x，同时找a、b和c，直到找到了闭括号
3. 继续回到第1步，直到文件末尾

~~~
# [PackageDev] target_format: plist, ext: tmLanguage
---
# 顶级scope名称
scopeName: source.abc
patterns:
  # 引用repository#expression
  - include: '#expression'          
repository:
  # 定义expression规则
  expression:
    patterns:
      # expression使用#letter规则
      - include: '#letter'
      # expression使用#letter规则
      - include: '#paren-expression'
  # 定义letter规则
  letter:
    # 匹配a或者b或者c
    match: a|b|c
    # 匹配规则的名称
    name: keyword.letter
  letterX:
    match: x
    name: keyword.letterX
  paren-expression:
    # 从'('开始
    begin: '\('
    # 遇到')'结束
    end: '\)'
    beginCaptures:
      '0':
        name: punctuation.paren.open
    endCaptures:
      '0':
        name: punctuation.paren.close
    name: expression.group
    patterns:
      # 在'('后使用#letter的规则
      - include: '#letter'
      # 在'('后使用#letterX的规则
      - include: '#letterX'
...

~~~



其实这上面的匹配规则有不少bug，比如没有区分括号对，只是简单粗暴的遇到闭括号就结束。不过作为例子，这个bug反而更能进一步说明内部解析的机制。



### 补充说明

* 如果语法块变量include的不恰当，可能会造成死循环。可以在vscode上通过Help->Toggle Developer Tools查看到具体报错信息。

* 如果是作为前端的一员，可能会对这么多正则堆叠上来的效率有疑问。然而vscode在匹配效率上做的非常好。在不开启语法堆栈解析的情况下，1000行以内基本可以做到秒出。另外就是vscode的内置javascript的syntaxes上的正则数量和长度都是非常多和长的，所以不用过于担心这点。当然如果能够有时间和精力优化正则效率最好了，不过要看优化的边际成本是否大于边际利益了。

* vscode当中一行解析的字符长度是有限的，系统默认是20000，相关设置的key是editor.maxTokenizationLineLength，超过这个长度，将只有白色代码躺在那里。所以直接打开压缩的代码在视觉效果上可能会比较难受。
