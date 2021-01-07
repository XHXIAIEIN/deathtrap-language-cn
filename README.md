授人以鱼不如授人以渔。  
汉化的方法写在这里了，有空的同学自己去试试~

--

# 语言文件
Deathtrap 的语言文件在这个位置
```
...\steamapps\common\Deathtrap\Strings
```

打开文件夹之后你可以看到各个语言为命名的文件夹。  
最外面的那个 Files.N2PK 是默认英文。

但是你也看到了，他们都是 Files.N2PK， 普通的方式根本无法打开。  
所以，需要进行解包：

# N2PK 解包工具

很幸运，  
@mome-borogove 大佬分享了一个解包程序 [N2PK Unpacker](https://github.com/mome-borogove/40K-n2pk-converter)  
它使用 python 语言编写，你要需要 python 官网安装一个最新版，就可以使用了  

# 如何使用
将  unpack-n2pk.py 保存到本地，比如 D 盘。  
然后把你准备解压的 Files.N2PK 也放到这里。  
    
然后在文件夹窗口里 按住 Shift + 鼠标右键， 在此处打开 Powershell 窗口 (S)    
  
会弹出一个蓝色窗口的 Windows PowerShell 控制台，在里面输入这句：  
```
python unpack-n2pk.py Files.N2PK  output
```
  
后面那个 output 是指定解压到哪个文件夹的参数  

或者可以用一个批处理程序来自动输出：  
在记事本粘贴，然后另存为 unpack.bat 放在同一个文件夹下。

```
@echo off
cmd /C "python unpack-n2pk.py Files.N2PK new"
move Files.N2PK output
exit
```


# 如何翻译
解压出来的文件就是这几个xml文件：   
```
  lang.xml
  lang_artifacts.xml
  lang_editor.xml
  lang_news.xml
  lang_quest.xml
  lang_skills.xml
  lang_soldiers.xml
  lang_traps.xml
```
  
然后直接用文本编辑器就可以编辑了。  
  
文件结构：  
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
<Root>
    <LanguageName>
            <eng>English</eng>
    </LanguageName>
    <Languages>eng</Languages>
    <Campaign> 
            ... 
    </Campaign>
</Root>
```
  
由于我不想直接替换英文的文件，想着，能不能直接在语言设置里添加一个中文选项？  
  
于是，我打开 Strings 目录，新建了一个 Chinese 文件夹，接着把解压出来的几个xml文件放进去。  
   
如果要添加语言，注意文件里是有个<Languages>标签区分的。  
比如 <eng>English</eng>,   <DE>German</DE>,  ...  
  
所以，这就明白了：  
把标签换成 CN 试试  <Languages>CN</Languages>  
然后后面的都是用CN  

```
<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
<Root>
	<LanguageName>
		<CN>简体中文</CN>
	</LanguageName>

	<Languages>CN</Languages>

	<Campaign>
		<GUI>
			<main>

				<exit>
					<CN>返回标题</CN>
				</exit>

				<single>
					<CN>单人模式</CN>
				</single>

			...
```

再次重启游戏，打开设置。  
嗷！！终于出现了！
  
[previewimg=21709827;sizeFull,floatLeft;language.png][/previewimg]

既然修改有效，后面的都是体力活了~  

# 自动换行
毕竟是个老游戏，开发团队那时候估计也没想过要做本地化。所有UI都是根据英文来设计的。  
  
包括对话框的自动换行，游戏引擎判断换行的方式有2种：  
分别是：单词， 字母。  
单词：hello world 这是2个单词。自动换行就变成 hello \n world  
字母：比如一行只能显示 4 个字母，强制换行就会变成 hell- \n o worl- \n d  
  
注意到了吗，强制换行的话，前面一行的末尾会自动加一个 - 符号。  
  
所以很不幸，如果你直接把文本翻译成中文，一行超过 15 个中文字符，就会自动在后面添加 - 的符号。然后强制换行。  
  
如果你能忍受后面的 - 符号，倒还好。反正我看着有点别扭。  
  
于是想自动换行的话，只能手动插入空格。   
如果空格的下一行超出长度，就会自动到下一行，如果一行放不下，就加- 强制换行。  
   
还有一种不加空格的办法。  
就是用 \n 来换行。这个就需要翻译的人计算好一行显示多少个字了~  

总结：  
1. 一行最多 15 个中文字符，如果里面包含了数字、英文，符号，实际长度会更短。  
超过会强制换行。（有个-）  

2. 约定：  
如果你要翻译，系统内置的数字，变量，符号之类的， 最好加使用空格隔开。  
中文标点符号，。 也可以在后面加一个空格，这样会自然换行。

...

# 字体配置
其实我觉得原版英文的字体很好看，但是直接修改文本的话，原有的字体肯定是没有中文字符的。所以，还得修改一下字体文件。  

```
\steamapps\common\Deathtrap\Cfg\Fonts\FontMap.cfg
```

这个修改起来比较简单，换字体名字就可以了。  
当然，这个文件也可以指定语言的。  
比如 FontMap_CN.cfg  
这样就只有语言标签为 CN 才会应用这字体  

```
FONTNAME_GENERAL=Philosopher
FONTPATH_GENERAL=UI/Fonts/Philosopher-Regular.ttf

FONTNAME_TITLES=Philosopher
FONTPATH_TITLES=UI/Fonts/Philosopher-Regular.ttf

FONTNAME_POPUPS=Philosopher
FONTPATH_POPUPS=UI/Fonts/Philosopher-Regular.ttf

FONTNAME_TEXT=Philosopher
FONTPATH_TEXT=UI/Fonts/Philosopher-Regular.ttf

FONTNAME_DEBUG=Arial
FONTPATH_DEBUG=UI/Fonts/ARIAL.TTF

FONTNAME_SUBTITLE=Covington Bold
FONTPATH_SUBTITLE=UI/Fonts/Coving03.TTF 
FONTARGB_SUBTITLE=255;255;255;255 
FONTSIZE_SUBTITLE=18


FONTFILENAME0=./UI/Fonts/Philosopher-Regular.ttf
FONTFILENAME1=./UI/Fonts/TGL_0-1451Eng.ttf
```


然后把想替换的字体放到这个目录，就可以了~
```
\steamapps\common\Deathtrap\UI\Fonts
```


例如。我使用了魔兽世界玩家@暗影无花 制作的一款字体——   
啊...这款字体没有名字。  
就是把两款字体(包豪斯中文+方正粗活意英文数字)合成在一起变成——合成字体！  
虽然我也不知道包豪斯中文到底是哪款中文字体，反正看的还顺眼就是了。  
  
简单替换一下，变成这样  

```
CAN_USE_UPPERCASE=0

FONTNAME_GENERAL=Font Creator Program
FONTPATH_GENERAL=UI/Fonts/wow.ttf

FONTNAME_TITLES=Font Creator Program
FONTPATH_TITLES=UI/Fonts/wow.ttf

FONTNAME_POPUPS=Font Creator Program
FONTPATH_POPUPS=UI/Fonts/wow.ttf

FONTNAME_TEXT=Font Creator Program
FONTPATH_TEXT=UI/Fonts/wow.ttf

FONTNAME_DEBUG=Font Creator Program
FONTPATH_DEBUG=UI/Fonts/wow.ttf

FONTNAME_SUBTITLE=Font Creator Program
FONTPATH_SUBTITLE=UI/Fonts/wow.ttf
FONTARGB_SUBTITLE=255;255;255;255 
FONTSIZE_SUBTITLE=18


FONTFILENAME0=./UI/Fonts/wow.ttf
FONTFILENAME1=./UI/Fonts/wow.ttf
```


--

# 会遇到的问题：
 
1. 翻译之后，部分变量会因为文本长度问题而无法读取。  
  
最明显的例子就是。道具表 lang_artifacts.xml  里面，  
对异常状态的敌人造成额外伤害，这个标签ID太长了：<AllSkillDamagePercentOnFrozenEnemies>   
导致在游戏中无法读取，但是只有翻译成中文会出现这情况，其他语言显示是正常的。  
  
也许也是解包的问题导致？总之，原因不明。
就是因为遇到了这个问题，我才不想继续再重新汉化这游戏了。。太蠢了。  
  
