# UGUIEffectDemo
## 基于UGUI的粒子特效解决方案。
### 一、解决UGUI下粒子特效常见的问题：
粒子特效的层级问题。即可以决定特效在UI元素之间的物理层级而不会出现穿插现象。\
粒子特效的裁剪问题。比如常见是一个滚动列表里的粒子特效，在特效被划出滚动列表的裁剪区域后应该是像UI元素一样看不见的。\
粒子特效的适配问题。不能像UI元素一样能够根据设备的分辨率来缩放大小。\
以上的3个问题是使用Unity的粒子系统来制作UI层的特效普遍遇到的问题。网上也有许多解决方案，但都不能完美的解决这3个问题，故而研究了如下方案。

### 二、解决方案
以上3个问题都是UI的问题，如果能够把粒子系统发射出来的粒子当成一个UI元素，那么以上问题就可以迎刃而解。
#### 1、cs代码。
建一个cs脚本【UIParticleSystem.cs】，继承自 MaskableGraphic，这样就可以把这个粒子系统系统发射出来的粒子当成UI元素了。具体代码可以到项目里查看。
#### 2、shader代码。
为了能够让粒子系统发射出来的粒子能够在UI层级上显示（UGUI的渲染队列是 “Transparent” ，RenderQueue = 3000，是透明度混合队列），所以单独提供了一个 shader。

### 三、制作规范
制作这套规范的目的是程序在游戏运行时动态加载特效并添加到对应的UI节点后，无需程序再去调整特效的位置、缩放、旋转等任何参数，减少程序开发人员和特效开发人员的过分耦合。\。
1、所有的粒子系统应该放到一个空的父节点里并且该父节点的 位置 = （0,0,0）、缩放（1,1,1）、旋转（0,0,0），调整特效位置时，请调整父节点下子节点的位置到合适位置。\
2、美术人员在制作特效时应该对着UI预制来做，避免大小，位置等参数不对。

### 四、制作流程（针对特效人员）
1、把UI预制拖到UIRoot下，并找到特效需要挂载的UI节点。\
2、在该UI节点下创建一个空的特效根节点。\
3、在空的根节点下创建一个特效节点（空节点），挂载 UIParticleSystem.cs 脚本，此时 ParticleSystem 会自动被挂载上。\
4、创建材质球，使用 "17zuoye/UI Particle Addtive" shader，并给材质球附上对应贴图后拖到 UIParticleSystem.cs 的material的字段上。\
5、特效人员开始制作特效并保存成预制放到特定文件夹内供程序人员使用
