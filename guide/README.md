# 一.服务端核心介绍
  所谓**服务端核心**(简称核心，有时也被叫做服务端)是指开服时需要使用的服务端运行核心文件或是补丁安装器，他们一般以 .jar 结尾，一般情况下，我们可以使用 CMD (文件以.bat为结尾)或是 Linux Shell (文件以.sh为结尾)运行这样的 jar 以管理服务器。

## 为什么要说「服务端运行核心文件或是补丁安装器」

  一般情况下，我们都应该认为一个服务器的所有运行代码都存在那一个小小的罐子(指jar)里，但是对于某些服务端核心来说，用于运行服务器核心代码完全不在这个 jar 里，如果提及原因的话，时间要拉回好几年前:   
  根据 Mojang 的最终用户许可协议 (EULA)，任何人能二次分发 Minecraft 二进制文件，不能分发 Minecraft 的源代码[^1]，也不能在其项目中包含 Minecraft 的代码部分，因此，一切插件服务端的伊始，CraftBukkit 服务端因为本体包含 Minecraft 代码，违反了  Mojang EULA 和千禧年数字版权法 (DMCA)，面临被起诉的风险，最终被迫停止开发。   
  于是，前来接起 CraftBukkit 服务端核心大旗的，是 SpigotMC 团队和他们开发的 SpigotMC 核心。SpigotMC 团队很聪明，他们直接不提供编译[^2]好的核心，而是向用户分发一个叫做 `BuildTools` 的工具，通过 BuildTools 工具，我们可以在自己的计算机上在线合成 Minecraft 代码和 Spigot 代码，最终合成为 Spigot 核心供我们使用，以此绕开了 Mojang EULA 和 DMCA 的管辖(SpigotMC 团队:我没提供源代码，是用户自己编译的，他们也没分发，就自己用用，没人犯法啊(笑))
    但这不是我们要说的补丁安装器，他是用来编译核心的，最终得到的核心属于`服务端运行核心文件`，是包含 Minecraft 代码的核心文件，那么补丁安装器是什么?   
    大家可能已经发现了 BuildTools 的弊端: 麻烦，为了开个服我既要准备编译环境[^3]，又要花差不多半个小时编译一遍核心，慢死了。因此，后来的 PaperMC 团队使用了一种更灵活的方式糅合服务端核心:`打补丁(patch)`。   
    其实很容易理解所谓的打补丁，PaperMC 团队会把每一次 Paper 的更新制作成一个个补丁文件的形式，然后我们可以在 Paper 官网下载到包含这些补丁的补丁安装器，然后运行补丁安装器，下载原版服务端[^4]文件，安装补丁，生成已打补丁文件并运行（如果你用过，或是即将使用 Paper 核心，那么可以留心 paper 服务端运行后会在根目录[^5]的 cache 文件夹内生成一个 mojang_X.X.X.jar 和 patched.X.X.X.jar，他们便是原版服务端文件和已打补丁文件(已打补丁文件便是包含Minecraft代码的`服务端运行核心文件`))

::: details 注释 1
[^1]: 指通过某些方式，将已经编译成计算机能够识别并运行的 Java 字节码文件(.class)还原回Java源代码(.java)的行为。   
[^2]: 与反编译相反，是指将 Java 源代码处理为字节码文件的行为。   
[^3]: 指编译时需要准备的前置软件，此处指`Git`。   
[^4]: 有关原版服务端的内容请见下文对 `Vanilla` 服务端核心的介绍。   
[^5]: 指服务端核心所在的那一层文件夹，下文可能会以 `.\` 标识，如 `.\cache` 即指服务端根目录下的 cache 文件夹目录。
:::


## 什么是 CMD，什么又是 Linux Shell?为什么我们不能直接双击 jar 运行服务端核心?

  Hey，你知道吗，这个世界上有三种主流的计算机操作系统，他们分别是`Microsoft Windows`, `macOS` 和 `Linux`，在这里我们只谈论 `Windows` 和 `Linux`。   
  大部分人都使用的是 Windows 操作系统，他以易用和图形化著称，其缺点是系统性能消耗相对较高，且极度依赖图形界面;而 Linux，以高性能和开放性著称，他是开源[^6]的，因此高度可定制，同时不依赖与图形界面，很适合服务器操作系统，缺点是对于服务器版Linux发行版[^7]操作系统，一般没有图形化界面，只有黑白的代码界面，因此需要一定 Linux 系统知识才能使用。   
  对于新手来说，除非你有一定运维知识，否则我推荐你使用 Windows 操作系统(比如你正在用的这台)来开服。   
  CMD 是 Windows 操作系统的命令提示符，我们可以使用 CMD 的 .bat 批处理文件在 Windows 上批量完成某些任务; Linux Shell 则用途相同，是 Linux 上用于批量执行任务的东西。   
  那么为什么我们要使用这些东西来开服，直接双击jar运行服务端核心不行吗?   
  答案是因为有时行，有时不行，而且行的那个也不是很彳亍:对于一部分服务端，当你双击运行服务端核心时，会弹出一个 Minecraft 官方服务端提供的原生的控制台窗口，但有些服务端是不提供的，直接双击会导致服务端运行但看不到运行状态，也不能向执行命令(而且那个原生的控制台窗口不支持显示颜色)。   

::: details 注释 2
[^6]: 即开放源代码，指向所有人开放软件的源代码并在一定程度上供人们修改。   
[^7]: 事实上，不存在Linux操作系统，Linux 只是一个内核，因此你用的 Linux 其实是 Linux 的发行版，他们基于 Linux 内核，并实现了不同的功能供你使用(如 Ubuntu,CentOS,Debian 等)。
:::


## 到底有哪些服务端核心

  那么说完上面的，那么现在究竟有哪些核心，他们应该怎么区分，又有什么区别呢?   
注意: 为了方便查阅，我们将会用**粗体**表示推荐使用的服务端核心，使用*斜体*表示另类，冷门，不推荐使用的服务端核心。   
注意:此处我们只会介绍目前还在积极更新的，或是有一定历史意义的服务端核心，对于刚出生就夭寿的，无意义的服务端，此处不多赘述   

1. Vanilla
     对于 Java 版来说，绝大多数服务端的始祖便是 Mojang 提供的官方服务端了，按照习惯，我们把官服叫做 Vanilla(香草，代指纯净)，其实他本身的名字应该是 Minecraft_Server(但是大多数情况下我们不会用这个 Server)

  Vanilla 有以下的属性

  - **不能** 安装基于任何API的模组[^8]
  - **不能** 安装基于任何API的插件[^9]
  - 原生兼容好(如命令方块，MCFunction)
  - 自带原生 GUI 控制台窗口
  - 即开即用
  - 性能较差


  根据以上属性，我们可以发现 Vanilla 基本上啥都干不了，就是原生兼容好，因此比较适合开**原版的服务器**(比如**玩玩命令方块小游戏地图啊，基友联机啊**之类的)


  下载 Vanilla: 

    1. 前往正版启动器手动选择版本下载
    2. 前往第三方下载站下载（如GetBukkit:https://getbukkit.org/download/vanilla）

::: details 注释 3
[^8]: 此处指基于 ForgeAPI，LiteLoaderAPI，RiftAPI，FabricAPI 等 API 开发的模组(Mod)。如果你无法理解，或是从未使用过该服务端所兼容的 ModAPI，那么我建议你不要使用此服务端开服。   
[^9]: 此处指基于 BukkitAPI，SpongeAPI 或是其他API开发的插件(Plugin)。如果你无法理解，或是从未使用过该服务端所兼容的 PluginAPI，那么也没关系，船到桥头自然直，毕竟我们讲的就是这个。
:::

*2.CraftBukkit(有时被称为Bukkit[^10])*
  这可不太好，因为仅用Vanilla我们无法快速，高效的从底层[^11]拓展游戏内容，因此，我们急需一个"窗口"，能够让有开发能力的服主通过这个窗口深入Minecraft，拓展MC内容。于是，CraftBukkit带着BukkitAPI出现在了我们的面前。
  CraftBukkit是一个包含了 BukkitAPI 的服务端，这意味着开发者们可以通过 BukkitAPI 提供的(有限的)内容来拓展服务器逻辑，增强游戏性。不同的开发者们把包含不同额外功能的拓展内容使用 .jar 文件包装起来，以让我们把这些文件放入 CraftBukkit 独有[^12]的插件文件夹(`.\plugins`)加载不同的功能。这些东西就叫做**插件**。

  CraftBukkit 有以下的属性:

  - 基于Vanilla
  - **可以** 安装基于 BukkitAPI 的插件
  - **基本不可以** 安装基于任何 API 的模组[^13]
  - 性能较差

  *为什么不推荐?:CraftBukkit 虽然是跨时代性的，但他和 Vanilla 的性能一样差(甚至更差)，因此在后来的日子里诞生了很多自带优化的服务端，CraftBukkit 对于我们来说只是一个过去时代的丰碑罢了，并不能满足实际使用需求了*
  *讲个题外话，CraftBukkit 曾经的开发者现在要么离开了自己心爱的项目，要么去了 SpigotMC 团队，有一个人挺不寻常，他叫 Searge，他最后收到了 Mojang 的邀请，前去开发 Minecraft 了。对于有的 Mod 开发者来说，你应当知道 Mod Code Pack(MCP)也是由 Searge 等人发起的，而MCP使用的映射名「Srg 名」，也是为了纪念 Searge 这位巨佬*


  下载 CraftBukkit: 

    1. BukkitDev官方:~~http://dl.bukkit.org/~~ (由于Mojang EULA和DMCA的要求，已停止服务)
    2. SpigotMC官方:https://hub.spigotmc.org/jenkins/job/BuildTools/ (CraftBukkit不直接提供，你只能通过BuildTools手动构建)
    3. 使用第三方下载站下载已经构建好的CraftBukkit（如GetBukkit:https://getbukkit.org/download/craftbukkit）

::: details 注释 4
[^10]: 把 CraftBukkit 称作 Bukkit 其实是不负责任的，Bukkit 其实是一个规范，他仅包含接口，不包含实现，我们不应将两者划等号。   
[^11]: 本指原始代码，此处指从代码层面修改游戏内容。   
[^12]: 这里是指在当时，不是指现在。   
[^13]: 为什么说是**基本**呢?因为其实在1.6时代有一个叫做 PlayerAPI 的玩意允许你配合 CraftBukkit 玩一些类似于灵活动作的玩意，不过现在早已销声匿迹了。
:::

3.Spigot
  CraftBukkit 是挺好，但是他性能和 Vanilla 一样捉急，甚至装多了插件还可能会更差，人们急切需要一个能够优化服务端处理逻辑，提升服务器性能的服务端，曾经有过多种这样的服务端，有的可能优化了TNT爆炸逻辑，有的可能优化了耕田逻辑，但是活到最后的，是包含了他们之中绝大部分优化功能的 **Spigot**。
  Spigot 由 SpigotMC 团队开发，可以说是 CraftBukkit 的正统续作，他不仅完全兼容 BukkitAPI 规范，还提供了更多独有的开发API[^14]，最重要的是，这个服务端优化很好，因此十分稳定。

  Spigot 有以下的属性:

  - 基于 CraftBukkit
  - **可以** 安装基于 BukkitAPI,SpigotAPI 的插件
  - **不可以** 安装基于任何 API 的模组
  - 稳定性好
  - 性能较好


  下载 Spigot:

    1. SpigotMC 官方:https://hub.spigotmc.org/jenkins/job/BuildTools/ (Spigot不直接提供，你只能通过BuildTools手动构建)
    2. 使用第三方下载站下载已经构建好的Spigot（如GetBukkit:https://getbukkit.org/download/spigot）

::: details 注释 5
[^14]: Spigot提供的独有API被称作SpigotAPI，其独立于CraftBukkit原生的BukkitAPI(虽然CraftBukkit现在由SpigotMC同时维护，但依然把一些API分开了)，后面要提的Paper服务端也同样提供了PaperAPI，同SpigotAPI和BukkitAPI隔离了起来。这也是服主们开服时某些插件在不同的服务端有不同的运行情况(有的能用有的不能了)的原因。此处独有也是指在当时，现在只要基于Spigot的核心都应支持SpigotAPI。
:::

// TO DO 

**4.Paper(曾用名PaperSpigot，有时被称为PaperClip[^15])**
  一方面是认为Spigot更新太慢了，又一方面是认为Spigot的BuildTools太麻烦了，还一方面是因为Spigot的优化还 不 够 劲，因此，一群人创建了Paper。

  Paper 有以下的属性:

  - 基于Spigot
  - **可以** 安装基于BukkitAPI,SpigotAPI,PaperAPI的插件
  - **不可以** 安装基于任何API的模组
  - *部分*自带原生GUI控制台窗口[^16]
  - 稳定性较好
  - 性能好
  - 更新迅速
  - 提供了更多的优化和服务端个性化选项[^17] [^18]
  - 构建、使用方便
  - 搭载了较为先进的Aikar's Timings®性能分析系统[^19]


*为什么推荐?:Paper是一个兼具稳定，性能，拓展的服务端核心，不仅提供了很多有效的优化，更有很多自定义选项供服主选择，几乎100%兼容BukkitAPI插件也是人们选择Paper的主要原因*


  下载Paper:

    1. PaperMC官方: https://papermc.io/downloads (对于旧版本核心，你可以前往页末的`LEGACY`标签下载不受支持的旧版的Paper核心

::: details 注释 6
[^15]: 此处PaperClip应当指的是Paper的**补丁安装器**，不含Paper核心本体，但因为用补丁安装器安装补丁并启动服务器基本感觉是一气呵成的所以大家总是把PaperClip当做Paper本体。   
[^16]: 之所以说部分支持，是因为大部分版本Spigot是把这个丑到爆炸的控制台删掉了的(即只能使用CMD或Linux Shell开服)，但自某个高于1.15的版本起，Paper又恢复了这个控制台以防你手贱双击打开了JAR但没有办法操控服务器，但这会导致在你不指定`nogui`参数时用命令行开服依然会把那个控制台给召唤出来。   
[^17]: 位于`.\paper.yml`。其实Spigot也是有这样的文件的，位于`.\spigot.yml`，同理，CraftBukkit也有，位于`.\bukkit.yml`，下游服务端是同时拥有上游服务端的这些文件的，因此新的服务端定义的新的文件提供了上游服务端所没有的新特性供服主们设定，而不是相互挤兑冲突。   
[^18]: Hey，也许你是一个生电玩家转生的新手服主，希望开一个生电服，如果如此，请切记**不要**使用Paper，Paper内含对包括0tick等Minecraft原版"特性"的修复，可能会导致你和你的玩家感到疑惑，因此，你应当使用**Spigot**。   
[^19]: Timings是一种自Spigot开始自带的性能分析器，允许你通过一个网页查看一段时间内服务器的总耗能情况，据此推断出哪些插件，或是哪些世界，或是哪些生物卡服。Spigot也有Timings，但是是旧版的，一般称作Spigot Timings，虽也是由Aikar设计但是网页界面观感和功能都相差甚远。Aikar's Timings同时也搭载在Sponge核心中。
:::


5.Tuinity
  众所周知，Paper是开源的，那么这意味着，所有人都可以通过Paper所在的代码托管网站*Github*向PaperMC团队提交各种各样的漏洞修复/性能优化代码，而PaperMC团队也可以选择性的将这些代码合并到自己的项目中，完成一次协作。前往Paper的Github的 [Pull Request](https://github.com/PaperMC/Paper/pulls) 界面，你可以看到这里依然还有超过60个的代码合并请求尚在活跃状态但未被PaperMC团队合并。这些提交中可能包含着诸如视距优化这样的刚需，也包含对开发者有益的API更新。
  但Paper就是不合并，你也没办法。
  因此，一名叫做*Spottedleaf*的大佬站了出来，Fork[^20]了Paper的仓库，然后把那一堆PR[^21]全合并了，又作了一些改动，最后，Tuinity横空出世了
  曾经一段时间内，Tuinity仅支持JRE11[^22]作为其运行环境 但现在Tuinity只需JRE8+即可运行
  启动Tuinity会生成tuinity.yml，在其中可设置单玩家怪物生成，分离视距等高级参数。即使你不会设置这些参数，Tuinity自身自带的一个个优化也足以你的服务器使用。

  Tuinity 有以下的属性:

  - 基于Paper
  - **可以** 安装基于BukkitAPI,SpigotAPI,PaperAPI,TuinityAPI的插件 
  - **不可以** 安装基于任何API的模组
  - 性能极佳
  - 更新较快
  - 较为稳定


  下载Tuinity:

    1. CodeMC自动构建站: https://ci.codemc.io/job/Spottedleaf/job/Tuinity/

::: details 注释 7
[^20]: 指使用Git克隆(拷贝)别人的代码仓库到自己的名下的行为
[^21]: 即Pull Request，拉取请求，就是上面说的那些希望合并的代码
[^22]: 即Java Runtime Environment(Version 11)，Java11的运行环境。同理，后文中JRE8+也指Java8以上的运行环境
:::


6.Akarin/Torch[^23]
  用*Akarin Project*开发者们的原话来说，Akarin是一个 **"来自新纬度的服务端"** *(A server software from the 'new dimension'.)*，其本质原因是Akarin以 **多线程** *(Multi-Threaded)*著称。
  那么在此之前，我们需要了解什么是多线程。简单的来说，人一般情况下只能专心干一件事情，那么我们可以把这种行为叫做单线程;如果你能一下干多个事情，那么这就是所谓多线程——从软件或者硬件上实现多个线程并发执行的技术。
  在Akarin之前，绝大多数的服务端的核心任务都是由主线程这一条线程完成的，如果同时有很多事情要做，那么他得做完了一个再做另外一个，这就会引起卡顿，如果做的这件事情无线重复，或是要花费太长时间以至于连服务器的基本运行事件都给挡住了，那么就会引起**堵塞**，导致服务器瞬卡甚至崩溃。
  通过使用Akarin，我们可以将主线程本应完成的动作转移到其他子线程同时执行，极大的减缓了服务器压力。
  当然，因为这是一个新技术，同时让一个本不兼容多线程的东西兼容多线程是一个很难的工程，因此总会有不稳定因素。

  Akarin 有如下的属性:

  - 基于Paper/Tuinity[^24]
  - **可以** 安装基于BukkitAPI,SpigotAPI,PaperAPI，**可能可以**安装基于TuinityAPI的插件
  - **不可以** 安装基于任何API的模组
  - 性能极佳
  - 支持多线程
  - 更新不快
  - 不太稳定


  下载Akarin:

    1. Github Actions: https://github.com/Akarin-project/Akarin/actions
    2. JosephWorks Jenkins: http://josephworks.ddns.net:8080/job/Akarin-project/

::: details 注释 8
[^23]: Torch，前称TorchSpigot，是一个支持1.8.8的优化核心，是Akarin服务端的前身。由于在部分代码和统计系统上，Akarin仍使用*"Torch"*表示Akarin服务端，因此这里同时将Torch写上
[^24]: 自1.14开始，Akarin开始使用*Tuinity*作为其项目前置，而不是原来的*Paper*，同时因此该服务端对不同API的插件兼容性需注意使用的服务端版本
:::


如果你看到了这里，那么恭喜你，你已经结束了所有**主流**BukkitAPI系服务端的介绍，接下来是一些搭载ForgeAPI或FabricAPI的模组服务端，两个基于SpongeAPI的服务端和两个魔怔猎奇基于其他API的服务端介绍，如果你不需要了解这些，请直接跳到下一节。


7.VanillaForge
  让我们把视线调转回刚开始的Vanilla，如果说Bukkit让修改服务端变成了可能，那么就一定有一个技术能够让修改客户端变为可能，那么这个可能就是Forge。
  VanillaForge则是一个Vanilla+ForgeAPI的服务端，他允许你像服务端安装ForgeMod，处理自定义物品，自定义方块，自定义实体操作。

  VanillaForge 有如下的属性:

  - 基于Vanilla
  - **不可以** 安装基于任何API的插件
  - **可以** 安装基于ForgeAPI的模组
  - 稳定性较好
  - 性能较差
  - 可插拔性强，易于更新[^25]


  下载VanillaForge:

    1. 前往Forge官网下载Forge Installer，并选择install server模式，将安装目录指向运行过一次的Vanilla服务端: http://files.minecraftforge.net/

::: details 注释 9
[^25]: 为什么要可以强调“可插拔性强，易于更新”呢，因为后面你将会看到，所有BukkitAPI+ForgeAPI的服务端（甚至Sponge系服务端)都需要糅合自己的API和ForgeAPI的代码，这导致Forge的部分代码和库是强制写死在服务端上的，你不能手动更新Forge版本。但VanillaForge只支持ForgeAPI，因此没有这个问题
:::


*8.Cauldron/MCPC+*
  “但是老弟你看，你这个逻辑有问题啊，我是可以加Forge模组了，但我还想加Bukkit插件啊，你那个VanillaForge什么的搞不了插件啊”
      —— 选自 刘慈欣《三体》 我也不知道在哪反正我就是想学大史说话 页
    不管大史究竟有没有说过这话，但是这确实是一个问题——那么究竟有没有能同时兼容BukkitAPI和ForgeAPI的服务端呢?
    答案是当然，最初搞出来这个玩意的服务端叫做MCPC+，自1.7.10起改名为Cauldron。
    但是很遗憾，因为糅合代码是个技术活，而且你也看到了，“糅合”，这是不符合Mojang EULA和DMCA规定的，因此Cauldron自1.7.10起停更，不再支持后面的版本。
    同时你也将看到，由于“糅合”的复杂性和难以维护性，因此每一个BukkitAPI+ForgeAPI服务端几乎都只维护一个主流版本，这也是此类服务端遍地开花的一个主要原因。

    Cauldron 有如下的属性:
    - 已停更
    - 基于Spigot
    - **可以** 安装基于BukkitAPI,SpigotAPI的插件
    - **可以** 安装基于ForgeAPI的模组
    - 支持至最高1.7.10


*为什么不推荐?:同CraftBukkit一样，Cauldron也已然成为了一个时代的奠基人和里程碑，其原始的完整代码仓库现在甚至无法被找到，我们也只能在各式各样的第三方构建站看到他的身影。只闻其声，不闻其形。*


  下载Cauldron:

    1. 前往第三方构建站下载


9.KCauldron
  KCauldron是Cauldron的优化版/继承。

  KCauldron 有如下的属性:   

  - 已停更
  - 基于Cauldron
  - **可以** 安装基于BukkitAPI,SpigotAPI的插件
  - **可以** 安装基于ForgeAPI的模组
  - 仅支持1.7.10


  下载KCauldron:
    1.前往第三方构建站下载


10.Thermos
  Thermos是KCauldron的优化版。

  Thermos 有如下的属性:

  - 已停更
  - 基于KCauldron
  - **可以** 安装基于BukkitAPI,SpigotAPI的插件
  - **可以** 安装基于ForgeAPI的模组
  - 仅支持1.7.10


  下载Thermos:

    1. Github Releases: https://github.com/CyberdyneCC/Thermos/releases


11.Contigo
  Contigo是Thermos的优化版/继承。

  Contigo 有如下的属性:

  - 已停更
  - 基于Thermos
  - **可以** 安装基于BukkitAPI,SpigotAPI的插件
  - **可以** 安装基于ForgeAPI的模组
  - 仅支持1.7.10


  下载Contigo:

    1. Github Releases: https://github.com/djoveryde/Contigo/releases


12.Uranium
  Uranium是一款基于KCauldron的BukkitAPI+ForgeAPI服务端，其整合了部分Thermos对服务端的修复，同时进行了一些输入书与笔虚体问题的BUG修复。其最大的特点[^26]是强制使用UTF-8编码作为配置文件编码[^27]和通过UraniumPlus Mod令1.7.10客户端支持Title和Actionbar[^28]。

  Uranium 有如下的属性:

  - 基于KCauldron
  - **可以** 安装基于BukkitAPI,SpigotAPI的插件
  - **可以** 安装基于ForgeAPI的模组
  - 仅支持1.7.10


  下载Uranium:

    1. Jenkins CI: https://ci.uraniummc.cc/job/Uranium-dev/

::: details 注释 10
[^26]: 仅代表个人观点
[^27]: 事实上，我们看到的所有文本，其内容都是经过编码存储在计算机上的，对于Minecraft服务端来说，在1.7.10-版本，Windows使用ANSI编码，而Linux使用UTF-8编码，这引起了诸多不便，因此Uranium强制在所有操作系统上运行该服务端，文件编码均为UTF-8，简化了使用流程
[^28]: Title是自1.8引入的，在客户端上显示大标题和副标题的功能;Actionbar是自1.8引入的，在客户端物品栏上方显示字幕的功能
:::


**13.CatServer**
  此时聪明的网友已经发现了一个问题:怎么上面的BukkitAPI+ForgeAPI服务端都只支持1.7.10啊，有没有支持高版本的?
  答案很显然是肯定的，CatServer就是在那样的大环境下诞生的服务端，他支持1.12.2的BukkitAPI+ForgeAPI，发展至今已十分稳定，同时也拥有独特的优化和BUG修复。

  CatServer 有如下的属性:

  - 基于Spigot
  - **可以** 安装基于BukkitAPI,SpigotAPI的插件
  - **可以** 安装基于ForgeAPI的模组
  - 稳定性好
  - 性能较好
  - 更新较快
  - 仅支持1.12.2

*为什么推荐?:CatServer历经多年的打磨，其已经非常稳定，同时因为1.12.2版本从技术上讲依然是一个稳定的年轻版本，因此使用CatServer开Mod服或许是你的不二之选*
*又一个题外话:如果你刚入服主圈，那么你可能不知道，由于作为第一个支持高版本的BukkitAPI+ForgeAPI服务端，CatServer有过一段艰苦，黑暗的发展历史，从“抄袭风波”到收购风波，从付费风波再到“后门风波”，CatServer曾有过一段饱受诟病的日子，甚至还和下面某些服务端作者产生过争执......笔者作为那段时代的亲历者，只能用一句话来形容那时:
  “黑，真他妈的黑啊”*


  下载CatServer:

    1. Github Releases: https://github.com/Luohuayu/CatServer/releases

  下载CatServer-Async[^29]:

    1. Github Releases: https://github.com/Luohuayu/CatServer/releases/tag/Async-final

::: details 注释 11
[^29]: 即CatServer的多线程版本，用开发者的话来说，“由于多线程版存在过多兼容性问题无法修复, 不再提供更新, 也不推荐使用.”，该版本最后停更于`Mar 19,2020`。本文笔者也不推荐使用此版本
:::


14.Mohist(曾用名PFCraft)
  Mohist和下面的Magma一样，都有一点“另类”，他们本体基于Paper，而不是Spigot，这意味着这两个服务端不仅可以享受Paper带来的漏洞修复和优化，还可以让你轻松使用基于PaperAPI开发的插件。
  但这还没完，Mohist还支持控制台信息国际化[^30]，可选择服务端Mod语言[^31]，内置插件管理器[^32]，NMS向下兼容[^33]等等非常实用的功能！同时，其开发者正在开发的Mohist-BungeeCord可以让你在跨服使用Mohist时减少各种问题[^34]。
  但是很遗憾，由于Mohist本身工程量大难以维护，也由于Mohist开发组重组，近几个月内的Mohist稳定性并不是很好。

  Mohist 有如下的属性:

  - 基于Paper
  - **可以** 安装基于BukkitAPI,SpigotAPI,PaperAPI的插件
  - **可以** 安装基于ForgeAPI的模组
  - 稳定性较差
  - 性能较好
  - 更新较快
  - 控制台/模组本地化支持
  - 内置插件管理器
  - NMS向下兼容
  - 支持1.12.2,1.15.2[^35]


*说个题外话:笔者曾有幸参与了Mohist控制台信息的简体中文、繁体中文本地化工作，并亲眼见证了Mohist从使用高峰到现在的开发过程。Mohist的原开发者Mgazul是个好人，而且能在家庭条件十分有限的情况下，开发出Mohist并开源供大家使用，可以说是我们这个圈子的幸运。*


  下载Mohist-1.12.2:

    1. CodeMC Jenkins CI: https://ci.codemc.io/job/Mohist-Community/job/Mohist-1.12.2/

  下载Mohist-1.15.2:

    1. CodeMC Jenkins CI: https://ci.codemc.io/job/Mohist-Community/job/Mohist-1.15.2/


::: details 注释 12
[^30]: 该功能会自动本地化控制台信息，为你展示你能看得懂文字(Mohist现支持简体中文和繁体中文的控制台本地化)，效果大约如下:

![Mohist-控制台本地化](https://i.loli.net/2019/05/25/5ce8f8ca8abef72303.png)

[^31]: 在一般服务端下，在服务端安装的Mod仅能限制默认的美式英文(en_US)本地化语言文本，这导致客户端无法按照本地语言显示文本，即使有汉化也没法看。但Mohist通过这项功能解决了这个问题
[^32]: 一般来说，服务端插件在服务器启动以后便不能，安装、卸载、更新，要想那么做，得先关闭服务器，这很耗时，插件管理器允许你通过执行指令，在服务器开启的情况下热配置插件。著名的插件管理器PlugMan和Yum两个插件，而Mohist自带了他们的部分功能
[^33]: NMS是指net.minecraft.server包，是Minecraft服务器底层的代码实现，在BukkitAPI没有包装对应方法的情况下，开发者需要调用NMS来实现内容。由于不同版本服务器的NMS包的包名也不同，因此一般开发者开发的这类插件只能兼容一个版本。Mohist可以使部分原仅兼容1.8-1.12间版本的此类插件兼容Mohist
[^34]: 有关BungeeCord等跨服代理的内容超出了本文的范围，故不多赘述
[^35]: 有消息称Mohist开发组正在研发/测试1.16.x版本的Mohist，且Mohist代码仓库中确实存在标签为"1.16.x"的代码分支(空仓库)
:::


15.Magma
  Magma同样是一个基于Paper[^36]的BukkitAPI+ForgeAPI服务端。

  Magma 有如下的属性:

  - 基于Paper
  - **可以** 安装基于BukkitAPI,SpigotAPI,PaperAPI[^36]的插件
  - **可以** 安装基于ForgeAPI的模组
  - 稳定性较好
  - 性能较好
  - 更新较快
  - 支持1.12.2,1.15.2[^37]


  下载Magma-1.12.2:

    1. Github Releases: https://github.com/magmafoundation/Magma/releases (稳定版，请下载-server结尾的版本，-installer结尾的版本暂无法使用)
    2. Jenkins CI: https://ci.hexeption.dev/job/Magma%20Foundation/job/Magma/job/master/ (开发版)

  下载Magma-1.12.2-全Paper特性支持版:

    1. Jenkins CI: https://ci.hexeption.dev/job/Magma%20Foundation/job/Magma/job/feature%252Ffull-paper-support/ (开发版)

  下载Magma-1.15.2:

    1. Jenkins CI: https://ci.hexeption.dev/job/Magma%20Foundation/job/Magma-1.15.x/job/1.15.x/lastSuccessfulBuild/ (开发版)

::: details 注释 13
[^36]: Magma的主要发行版本并未应用所有PaperAPI和Paper的补丁，这可能会带来一些问题
[^37]: 根据Magma项目说明，Magma尚在积极开发对1.16版本的支持，同时，Magma-1.15.2目前仅处于Beta测试版阶段，可能尚不稳定
:::


16.Arclight
  Arclight是一款由 @海螺螺 开发的“在 Forge 上使用 Mixin 实现的 Bukkit 服务端”，提供了1.14.4和1.15.2两个高版本的BukkitAPI+ForgeAPI支持

  Arclight 有如下的属性:

  - 基于Spigot
  - **可以** 安装基于BukkitAPI,SpigotAPI的插件
  - **可以** 安装基于ForgeAPI的模组
  - 稳定性相对较好
  - 性能较好
  - 更新较快
  - 支持1.14.4,1.15.2


  下载Arclight(1.14.4,1.15.2):

    1. Github Releases: https://github.com/IzzelAliz/Arclight/releases
    2. AppVeyor CI: https://ci.appveyor.com/project/IzzelAliz/arclight/build/artifacts


如果你看到了这里，那么恭喜你已经结束了所有**主流**BukkitAPI+ForgeAPI服务端的学习，接下来是一些搭载FabricAPI的模组服务端，两个基于SpongeAPI的服务端和两个魔怔猎奇基于其他API的服务端介绍，如果你不需要了解这些，请直接跳到下一节。


17.SpongeVanilla&SpongeForge
  让我们再将目光转回CraftBukkit时期。一群人做出BukkitAPI以后，发现这个东西实在是太垃圾了:对Mod兼容性差，没有开发文档，代码规范随意，这不是他们想要的那个API。于是，一群人离开了Bukkit开发团队，转而开始制作他们心目中的那个完美的API框架——幸运的是，他们做出来了，这就是SpongeAPI和他的服务端实现:Sponge
  Sponge分为SpongeVanilla和SpongeForge两个版本:前者需要与Vanilla一起使用，他通过注入[^38]的方式，允许你在Vanilla服务端上安装基于SpongeAPI的插件；后者实现在Forge上，允许你在VanillaForge上安装基于SpongeAPI的插件（同时享受安装基于ForgeAPI的模组），需要提到的是，在SpongeForge中，其其实是作为一个**ForgeMod**来使用（即将其放入`.\mods`中并启动服务端），而非作为一个完整的服务端运行核心文件。
  很遗憾的是，由于生不逢时，Sponge并没有得到大多数开发者的支持，因此基于SpongeAPI开发的插件少之甚少，主流BukkitAPI插件迁移至SpongeAPI的更是屈指可数，因此对于普通服主来说，使用Sponge会导致在插件支持上落后于Bukkit使用者。
  同时，由于自1.13起，由于Minecraft源代码的大幅度改动导致ForgeAPI大幅度改动其代码，致使Sponge始终难以兼容1.13+版本，直到最近才发布了对*1.14.4*版本的支持

  SpongeVanilla 有如下属性:

  - **可以** 安装基于Sponge的插件
  - **不可以** 安装基于任何API的模组
  - 性能相对很好
  - 更新较快
  - 稳定性很好
  - 社区支持友好
  - 插件生态较差
  - 支持至最高1.14.4


    SpongeForge 有如下属性:

  - 基于ForgeAPI
  - **可以** 安装基于SpongeAPI的插件
  - **可以** 安装基于ForgeAPI的模组  
  - 性能相对很好
  - 更新较快
  - 稳定性很好
  - 社区支持友好
  - 插件生态较差
  - 对模组兼容性极佳
  - 支持至最高1.14.4


  下载SpongeVanilla:

    1. SpongePowered官方: https://www.spongepowered.org/downloads/spongevanilla/stable/

  下载SpongeForge: 

    1. SpongePowered官方: https://www.spongepowered.org/downloads/spongeforge/stable/

*题外话:曾经有一段时间，Sponge是市面上唯一一个支持1.8+高版本插件+模组的服务端，当Bukkit阵营始终停留在1.7.10时，已经支持之1.12.2的Sponge收到了大部分神奇宝贝服服主的欢迎——直到CatServer的发布*

::: details 注释 14
[^38]: 是一种将自定义代码导入到已有的计算机程序内，从而改变原程序的行为的行为
:::


18.VanillaFabric
  前面我们提到了`由于自1.13起，Minecraft源代码的大幅度改动`，这导致了CraftBukkit/Spigot，Sponge，Forge等项目分别出现了时常不同的窗口期，这段时间内这些项目都没有发布对新版本的支持。Sponge最为严重，直至今日还未发布对1.13版本的支持，其次是Forge，直至Minecraft发布1.14版本Forge都没有发布甚至是一个预览版本的Forge1.13，且当后来Forge1.13(.2)终于发布后，直至今日，Forge1.13(.2)都未发布一个稳定版。
  回归原题，在这段长达半年的窗口期中，涌现了几个新的ModAPI，抛去因为夹带私货和停止支持的RiftAPI，便只剩下了在当时乃至现在最流行的新生代ModAPI——Fabric[^39]
  Fabric和Rift不同，他不是在那段窗口期诞生的替代产品，他早自1.12时代就已出现，只不过和Sponge一样同样生不逢时，虽然设计先进，但大多数开发者当时依然只依赖于Forge开发模组而不是Fabric，知道窗口期的来临，Fabric才得以重获新生，得到了一部分开发者的支持[^40]。
  Fabric是**模块化**[^41]的，这意味着他不想高耦合的Forge，每次Minecraft源代码更新就要折腾一阵子推倒重来，他完全可以拆出不兼容的模块，并更换上兼容新版本的模块以快速发布更新，这也是Fabric甚至有针对每一个Minecraft预览版(Snapshot)的支持的原因。
  VanillaFabric则和VanillaForge类似，是基于Vanilla的实现了FabricAPI支持的服务端，他允许你安装FabricMod。

  VanillaFabric 有以下属性:

  - 基于 Vanilla
  - **不可以** 安装基于任何 API 的插件
  - **可以** 安装基于 FabricAPI 的模组
  - 稳定性较好
  - 性能相对较好
  - 可插拔性强，易于更新


  下载VanillaFabric:

    1. 前往Fabric官网下载Fabric Installer，并选择install server模式，将安装目录指向运行过一次的Vanilla服务端: https://fabricmc.net/use/

::: details 注释 15
[^39]: 此处很显然不严谨，Fabric本体是一个模组加载器（Mod Loader），不是一个ModAPI，Fabric的ModAPI是FabricAPI，但因为Fabric的模块化设计，FabricAPI作为FabricMod与Fabric本体（Fabric Loader）分离，不默认提供，因此FabricAPI又不能代表Fabric，故如此表示
[^40]: 虽然设计确实先进，但随着Forge发布对新版本的支持，Fabric又逐渐趋向没落，只留下来了一些或是小型的，或是客户端向模组的青睐，比如ReplayMod
[^41]: 是指将一整个代码项目设计成由多个互不相关又互相联系的模块，方便维护的代码设计模式
:::


*19.Fukkit*
  Fukkit是一款实现了BukkitAPI+FabricAPI支持的服务端，现已停更归档，因此不多赘述，也不提供下载地址。


*为什么不推荐?:已归档，不稳定*


如果你看到了这里，那么恭喜你已经结束了所有**主流**服务端的介绍，接下来是两个*看看就好*的魔怔服务端的介绍，如果你不需要了解这些，请直接跳到下一节。


*20.Glowstone*
  如果你是个聪明人，你会发现上面的所有服务端都基于Mojang提供的官方服务端Vanilla，那么有没有不依赖于Vanilla的服务端呢，答案是有，这就是Glowstone。
  Glowstone完全不依赖任何Mojang的源码，因此他非常的自由，不会受到Mojang EULA和DMCA的管控。

  Glowstone 有如下属性:

  - 少更新
  - **可以** 安装基于BukkitAPI,SpigotAPI,PaperAPI,GlowstoneAPI的插件
  - **不可以** 安装基于任何API的模组
  - 稳定性不好 
  - 性能较好
  - 缺少很多原版内容
  - 缺少NMS支持
  - 仅支持1.12.2


*为什么不推荐?:由于Glowstone不基于Vanilla，所有Vanilla负责的游戏行为都由其自行处理，因此Vanilla提供的一些功能（比如NMS，一些游戏逻辑）未在Glowstone中提供，同时他也不支持OBC[^42]，这会导致一些基于NMS的插件无法在Glowstone运行，因此对于绝大多数服主都不友好，故不推荐使用*


  下载Glowstone:

    1. Glowstone官方: https://glowstone.net/#downloads

[^42]: （谁起的怪名字根本没听过）即`org.bukkit.craftbukkit`包，某些Bukkit插件需要使用该包内提供的代码具体实现


*21.Cuberite*
  如果你还是个聪明人，你会发现上面所有的服务端都基于Java开发，那么有没有不基于Java的服务端呢，答案是依然有，这就是Cuberite。
  Cuberite同样完全不依赖任何Mojang的源码，因此他非常的自由，不会受到Mojang EULA和DMCA的管控。
  而且，Cuberite支持跨版本运行，1.8-1.12.2的客户端均能加入到你的Cuberite服务器中
  说起来你可能不信，Cuberite甚至还能在Android™️上运行。

  Cuberite 有如下属性:

  - 少更新
  - Android™️跨平台支持
  - **可以** 安装基于CuberiteAPI的插件
  - **不可以** 安装基于任何API的模组
  - 稳定性不好 
  - 性能较好
  - 缺少很多原版内容
  - 缺少NMS支持
  - 同时支持1.8-1.12.2[^43]

*为什么不推荐?:一个由C++制作的服务端虽然可以通过内存管理在理论上无限调优性能，但他不基于Vanilla，甚至不基于Minecraft的开发语言——Java，这导致了一系列兼容问题，因此不推荐使用*

  下载Cuberite:

    1. Cuberite官方: https://cuberite.org/


[^43]: 来自其官网说明，但根据其开源项目提交日志，Cuberite应已支持1.14版本的连接，并正在尝试对1.15的特性进行兼容


最后，以上服务端的迭代关系大致如下:
![anMron.png](https://s1.ax1x.com/2020/07/30/anMron.png)


至此，你已经完成第一节的所有学习，并基本了解了所有主流服务端以及其迭代关系。