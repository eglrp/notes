---
title: Working Notes
...

-   **You should blog even if you have no readers.**

-   360 的自我保护模式，关闭，然后才能退出。

    真想不通图书馆那个重启后自动回复到原始状态的电脑上干嘛要装 360，你还得手工
    关闭着破玩意。

    还有那蛋疼的有道词典以及它蛋疼的弹窗。以及搜狗输入法。即使搜狗输入再好，我
    也不愿意用，因为它打扰我，妈的智障软件。

    还有蛋疼的 Adobe 阅读器，安装后没有打开过一次，害得所有人打开后看它长长的
    license，都要点“接受”。

    还有图书馆的【有道笔记本】，txt 居然默认用这货打开！这货修改后没保存居然没
    有提示。当然我说的不是退出的时候直接就退出了，我说的是他居然没有显示一个
    `*` 表示内容已经修改，这不是行业规范吗？！

-   `CMAKE_BUILD_TYPE=Release/Debug`{.cmake}

    :   A Release build is equivalent to use the
        `--enable-optimized` flag in the configure script, while a Debug build is
        equivalent to the `--disable-optimized` flag.

-   For Visual Studio 2013, use the generator for Visual Studio 12. The
    name of the generator uses the Visual Studio version instead of its
    commercial name.

-   The most noticeable design decision of LLVM is its segregation between backend
    and frontend as two separate projects, LLVM core and Clang. LLVM started as a set
    of tools orbiting around the LLVM intermediate representation (IR) and depended
    on a hacked GCC to translate high-level language programs to its particular IR,
    stored in bitcode files. Bitcode is a term coined as a parody of Java bytecode files. An
    important milestone of the LLVM project happened when Clang appeared as the first
    frontend specifically designed by the LLVM team, bringing with it the same level of
    quality, clear documentation, and library organization as the core LLVM. It is not
    only able to convert C and C++ programs into LLVM IR but also to supervise the
    entire compilation process as a flexible compiler driver that strives to stay compatible
    with GCC.

    +   clang check
    +   clang format
    +   C/C++ program ---clang--> LLVM IR

    clang:

    1.  c/c++ -> LLVM IR
    2.  dump the internal Clang Abstract Syntax Tree (AST) representation of any
        program

    Clang Modernizer
      ~ It is a code refactoring tool that scans C++ code and
        changes old-style constructs to conform with more modern styles proposed
        by newer standards, such as the C++-11 standard

    Clang Tidy
      ~ It is a linter tool that checks for common programming mistakes
        that violate either LLVM or Google coding standards

    Modularize
      ~ It helps you in identifying C++ header files that are suitable to
        compose a module, a new concept that is being currently discussed by C++
        standardization committees (for more information, please refer to Chapter 10,
        Clang Tools with LibTooling)

    PPTrace
      ~ It is a simple tool that tracks the activity of the Clang
        C++ preprocessor

    ```bash
    $ clang-modernize –version
    clang-modernizer version 3.4
    ```

    Understanding Compiler-RT

    :   The Compiler-RT (runtime) project provides target-specific support for low-level
        functionality that is not supported by the hardware. For example, 32-bit targets
        usually lack instructions to support 64-bit division. Compiler-RT solves this problem
        by providing a target-specific and optimized function that implements 64-bit division
        while using 32-bit instructions. It provides the same functionalities and thus is the
        LLVM equivalent of libgcc. Moreover, it has runtime support for the address and
        memory sanitizer tools. You can download Compiler-RT Version 3.4 at http://llvm.
        org/releases/3.4/compiler-rt-3.4.src.tar.gz or look for more versions at
        http://llvm.org/releases/download.html

    ```cpp
    #include <stdio.h>
    #include <stdint.h>
    #include <stdlib.h>
    int main() {
        uint64_t a = 0ULL, b = 0ULL;
        scanf ("%lld %lld", &a, &b);
        printf ("64-bit division is %lld\n", a / b);
        return EXIT_SUCCESS;
    }
    ```

    ```bash
    clang -S -m32 test.c -o test-32bit.S    # -m32  --> 32 bit x86 program
    clang -S test.c -o test-64bit.S         # .S    --> assembly lang
    ```

    Even with the rise of Clang, DragonEgg persists today because Clang only handles
    the C and C++ languages, while GCC is able to parse a wider variety of languages.

    refs and see also

    -   <http://blog.csdn.net/yeliping2011/article/details/7210700>

-   tasklist, taskkill

-   一个中科大 LUG 的 Qt 教程：<https://lug.ustc.edu.cn/sites/qtguide/>

-   ShareX, <-- .net 4.0

-   CSAPP: encoding

    ```cpp
    /* WARNING: This is buggy code */
    float sum_elements(float a[], unsigned length)
    {
        int i;
        float result = 0;

        for (i = 0; i <= length-1; i++)
            result += a[i];
        return result;
    }
    ```

    In the single-precision floating-point format (a float in C), fields s,
    exp, and frac are 1, k = 8, and n = 23 bits each, yielding a 32-bit
    representation. In the double-precision floating-point format (a double in C),
    fields s, exp, and frac are 1, k = 11, and n = 52 bits each, yielding a
    64-bit representation.

    ```
    exp  = e_{k-1}...e_0 -> E
    frac = f_{n-1}...f_0 -> M

    s           k (exp)                         n (frac~[0, 1))
    1    +       8 (0-- 255,  -128-- 127)   +   23=32
    1    +      11 (0--2047, -1024--1023)   +   52=64
    ```

    exp = 0 -> denormalized form.

    page 89

-   i3 快捷键

    +   Mod1 + 数字 切换到不同虚拟桌面 (container)
    +   Mod1 + enter 启动 terminal 终端
    +   Mod1 + v 启动 dmenu, 可以方便启动各个程序
    +   Mod1 + f 全屏当前程序
    +   Mod1 + 方向键 在一个虚拟桌面 container 里切换不同程序界面
    +   Mod1 + Shift + 数字键 将当前程序移动到指定虚拟桌面
    +   Mod1 + Shift + ; 改变窗口排列为纵向
    +   Mod1 + Shift + q 退出当前程序窗口
    +   Mod1 + Shift + r 重启 i3, 不会关闭当前程序窗口, 但是会失去窗口的布局
    +   Mod1 + Shift + e 注销退出 i3, 会关闭所有程序
    +   Alt-Shift-<方向键>

[这部好片我一直舍不得安利 - 简书](http://www.jianshu.com/p/d8790315b483#)

[还以为她只有胸和屁股，那她怎么靠声音拿影后 - 简书](http://www.jianshu.com/p/66fd396bc62b)

[奶爸的英语教室-微广场-关注你最关心的内容](http://www.iwgc.cn/list/2128)

不断地练习，直到你的每一根手指都能清楚记得周围的按键在什么地方。

[电影下载帮助 - BT电影天堂](http://btbt.tv/bangzhu.html)

:   清晰度注解？

    1、CAM（枪版） CAM通常是用数码摄像机从电影院盗录。有时会使用小三角架，但大
    多数时候不可能使用，所以摄像机会抖动。因此我们看到画面通常偏暗人物常常会失
    真，下方的 字幕时常会出现倾斜。 由于声音是从摄像机自带的话筒录制，所以经常
    会录到观众的笑声等声音。因为这些因素，图象和声音质量通常都很差。

    2、TS(准枪版) TS是TELESYNC的缩写。TS与CAM版的标准是相同的。但它使用的是外置
    音源（一般是影院座椅上为听力不好的人设的耳机孔）这个音源不能保证是好的音源，
    因为受到很多背景噪音的干扰。TS是在空的影院或是用专业摄像机在投影室录制，所
    以图象质量可能比CAM好。但画面的起伏很大。论坛上常出现的有一般TS版和经过修复
    清晰TS版

    3、TC(胶片版) TC是TELECINE的缩写。TC使用电视电影机从胶片直接数字拷贝。画面
    质量还不错,但亮度不足，有些昏暗。很多时候制作TC使用的音源来自TS，因此音质很
    差，但画面质量远好过TS。如果不是太讲究的话TC版还是不错的选择。

    4、DVDSCR(预售版) SCR是SCREENER的缩写。DVDSCR预览版的或者是测试版的DVD，非
    正式出版的版本。从预览版 DVD 中获取，通过mpeg-4技术进行高质量压缩的视频格式。
    能比DVDRip早发布，但画质稍差。（经常有一些不在黑边里在屏幕下方滚动的消息，
    包含版权和反盗版电话号码 ，会影响观看。）如果没有严格的划分它的画质应与TC版
    差不多。

    5、R5（俄罗斯5区版） 俄罗斯5区版的DVD，因为配音为俄语，所以需要去寻找英语音
    轨，R5版本就是一种合成版本（俄5区DVD视频＋通过其它渠道获得的英语音轨），R5
    版本的画质一般都不错，音频部分由于音轨的来源不同，效果有好有差。

    6、HD RIP（高清版） HDRip 是HDTVRip（高清电视资源压缩）的缩写，是用
    DivX/XviD/x264等MPEG4压缩技术对HDTV的视频图像进行高质量压缩，然后将视频、音
    频部分封装成一个.avi或.mkv文件，最后再加上外挂的字幕文件而形成的视频格式。
    画面清晰度更高。

    7、BD（蓝光版） BD是Blue Disk的简称，翻译成中文是“蓝光影碟”的意思。就是从蓝
    光影碟转录的视频和音频，画面清晰度很高。

    8、DVD,HDVD,DVD5,DVD9 DVD的英文全名是Digital Video Disk,即数字视频光盘或数
    字影盘,它利用MPEG2的压缩技术来储存影像。

    9、HDVD（压缩碟或者经济版DVD） HDVD俗称压缩碟或者经济版DVD，介质通常为DVD-5
    （容量4.7G）也有DVD-9的（容量8.5G），采用MPEG-1或MPEG-2编码，由于码流较低，
    所以每张盘可容纳长达7个小时的视频节目，画质水平略高于或等同于VCD。用于看连
    续剧最省钱。

    10、VHSRip VHSRip是从零售版VHS录象带转制，主要是滑冰/体育内容的发布。

    11、TVRip 从电视（最好是从数码有线电视/卫星电视捕捉）转制的电视剧，或接收由
    卫星提前几天向电视网传送的预播节目（不包含加密但有时有雪花）。有些节目，比
    如WWF RAW IS WAR包含多余的部分；"DARK MATCHES"和CAMERA/COMMENTARY测试被包含
    在TVRip里。PDTV是从PCI数码电视卡捕捉，通常效果最好；破解组织倾向于使用SVCD
    来发布。VCD/SVCD/DivX/XviD rips也都被用于发布TVRip。

    12、WORKPRINT (WP) WORKPRITN (WP)是从未完成的电影拷贝转制而成，可能会缺失镜
    头和音乐。质量可能从最好到很差。有些WP可能和最终版本相差很远。(MEN IN BLACK
    的WP丢失了所有的外星人，代之以演员)；另一些则包括多余的镜头(Jay and Silent
    Bob). WPs可以作为有了好质量的最终版本后的附加收藏。

    13、DivX Re-Enc DivXRe-Enc是从原始VCD发布用DivX编码成的小一些的文件。通常可
    在文件共享网络找到。它们通常以 Film.Name.Group(1of2)等形式命名。常见的发布
    组织有SMR和TND。这些版本通常不值得下载，除非你不清楚某部电影，只想要 200MB
    的版本。一般应避免。

    14、Watermarks 很多从Asian Silvers/PDVD (参看下面)来的电影带有制作人的标记。
    通常是一个字母，名字缩写或图标，位于屏幕一角。最有名的是"Z","A"和"Globe"。

    15、Asian Silvers / PDVD Asian Silvers / PDVD是亚洲盗版商发行影片的，通常被
    一些发布组织购买来当做他们自己的发布。Silvers很便宜，在很多国家都很容易找到。
    发布Silvers很容易，所以现在有很多发布，主要是由一些小的组织发布；这些组织通
    常发布几个RELEASE后就不见了。PDVD和Silver一样，不过是压在DVD上。 PDVD通常有
    外挂字幕，质量也比Silver好。PDVD象普通的DVD一样转制，但通常用VCD的格式发布
    Scene Tags发布文件的标志。

    16、PROPER 根据发布规则，最先发布Telesync (TS)的组织赢得(TS发布的)比赛。但
    是，如果这个发布版本质量很差，同时另一组织有另一TS版本(或质量更好的同一片
    源)，那么标记PROPER被加到目录上以避免重复。PROPER是一个最主观的标记，很多人
    会争论是否PROPER比原始发布版本好。很多发布组织只不过因为输掉了发布比赛而发
    布 PROPER。发布PROPER的原因因该总是包含在NFO文件里。

    17、SUBBED 对于VCD发布而言，SUBBED通常表示字幕被压进了电影。它们通常是马来
    语/中文/泰文等，有时有两种语言。它们可能占据了很大一部分屏幕。SVCD支持外挂
    字幕，所以DVDRip用外挂字幕发布。这些信息可以在NFO文件中找到。

    18、UNSUBBED 当一部电影曾经发布过有字幕的SUBBED版本，没字幕的UNSUBBED版本也
    可能发布。

    19、LIMITED LIMITED电影指该电影只在有限的电影院放映，通常少于250家。通常较
    小的电影（比如艺术电影）的发行是LIMETED。

    20、INTERNAL INTERNAL发布有几个原因。经典的DVD组织有很多.INTERNAL.发布版本，
    这样不会引起混淆。同时，低质量的发布会加以 INTERNAL标记，这样不会降低发布组
    织的声誉，或由于已经发布的数量。INTERNAL发布可以正常的在组织的会员网站上获
    取，但没有其他网站管理员的要求它们不可以被交换到其他网站。一些TERNAL发布仍
    然流到IRC/NEWSGROUP，这通常取决于电影及其流行度。今年早些时候，人们把
    CENTROPY做为INTERNAL。这表示发布组织只向其会员和网站管理员发布。这和其通常
    意思不同。

    21、STV STV表示电影从未在电影院放映过就被发布，因此很多望网站不允许STV。

    22\ASPECT RATIO TAGS *ws*表示宽银幕，*FS*表示全屏幕。

    23、RECODE RECODE是以前已经发布过的版本，通常用TMPGenc编码过滤以去除字幕，
    纠正颜色等。虽然它们看起来好一些，但通常不认为这是好的行为因为发布组织应该
    去找他们自己的片源。

    24、REPACK 如果发布组织发布了一个坏的版本，他们会发布REPACK来解决这些问题。

    25、NUKED 一个发布可能因为多种原因被NUKE掉。有些网站会因为违犯他们的规则而
    NUKE发布(比如不允许发布TS版本)。但如果发布的版本有很大的问题 (如20分钟没有
    声音，CD2是错误的电影或游戏)，那么所有的网站都会NUKE这个发布。在这些网站上
    交换NUKED版本的人会失掉他们的信誉。但 NUKED发布仍然可以通过P2P/USENET传播，
    所以应该总是首先找到其被NUKE的原因以防万一。如果发布组织发觉他们的发布有问
    题，他们可以要求NUKE。

[Terr Tai's Blog](http://terrytai.me/profile/)

[如何评价电影《夏洛特烦恼》？ - 温建波的回答 - 知乎](https://www.zhihu.com/question/35514477/answer/66122643)

:   大春最后的回答让人明白了：即便真有这么一个人，穿越回到97年，极力告诉你想尽
    办法买房，你也不会因房致富。

    因为就算你买了你也会过两年就迅速卖掉，你不会撑到15年20年后再卖掉。当时的人
    是脱离不了当时人的思维的。在当时人看来，5000涨到了7000这已经非常恐怖了，迅
    速卖掉是最好的选择。你会沾沾自喜，等着后面房价降下来接手你房子的人吃苦果。

    你根本不会去相信 7 千会涨到 1 万，1 万会涨到 2 万，2 万涨到 3 万，现在二环
    边上都没有 5 万一平的均价了。。这种轮番上涨是当时代的人没法想象的。

    所以啊，思维是脱离不了时代的窠臼的。

    ---

    二十年之后的人过来告诉你房价那时候是一百二十万一平，你现在也不会去买的

    （话说，看了这个电影，真心不喜欢。）

任何智商正常人士都不会在这个问题上持太过标新立异的看法。

[上海的中学教育有何厉害之处？ - 知乎](https://www.zhihu.com/question/22195196#answer-3433815)

:   哈哈哈。英语确实。

[图灵社区 : 图书 : Chrome扩展及应用开发（首发版）](http://www.ituring.com.cn/book/1421)

[《用 CvANN_MLP 进行路牌判别》by 唐志雄 来自爱可可-爱生活 - 微博](http://weibo.com/1402400261/E1xsWvZ47?type=comment#_rnd1470039381546)

[如何比较 UPA 和萨格勒布学派的动画作品？ - 知乎](https://www.zhihu.com/question/22230758)

[Image Transforms - Fourier Transform](http://homepages.inf.ed.ac.uk/rbf/HIPR2/fourier.htm)

[2016校招小结 - 作业部落 Cmd Markdown 编辑阅读器](https://zybuluo.com/chaoren-fly/note/191366)

[WebGL Indoor Real-Scene Application System Based on Three.js](http://www.swoda.cn/Panorama/)

[ffmpeg Documentation](http://ffmpeg.org/ffmpeg.html)

:   <http://whudoc.qiniudn.com/2016/ffmpeg.exe>（36.9 MB）

    refs and see also

    -   [ffmpeg基本用法(转) - wainiwann - 博客园](http://www.cnblogs.com/wainiwann/p/4031129.html)

<http://gnat.qiniudn.com/misc/pano/Panorama.7z>

[广埠屯杀人案真相调查：一个电脑城的孤独守望者为什么会变成绝望者？](http://mp.weixin.qq.com/s?__biz=MzAwNDA5NDIwNA==&mid=2650149457&idx=1&sn=59d3c72c19749567153e289421a3fe24&scene=23&srcid=0804I12ZsEVleKiErUxQAzuk#rd)