# 美团点评打包技术使用详细教程
## 介绍
由于公司之前每次打包使用的是Android Studio打包方式，每出一个渠道包需要大概2分钟左右，公司目前得渠道暂时还很少只有25个，但是这么少得渠道包都只要需要花费将近一个小时的时间。加上之前对打包技术的了解基本都是Android Studio上点点点然后生成。作为一个有追求想提高技术（其实想有更多时间喝咖啡）的程序员，这种情况怎么能忍。之前就对急速的**美团多渠道打包**的技术有所耳闻，最近刚好有一点时间，所以花了几天的时间来研究一下。  
以下是我这几天对美团点评多渠道打包技术的一些总结和简单的上手教程，由于目前网上有以下三个版本，我会结合他们GitHub上的教程再进行详细的描述。我的开发环境是win10+Android Studio(4.0.1)+Java_1.8。
- [**walle**](https://github.com/Meituan-Dianping/walle)

- [**packer-ng-plugin**](https://github.com/mcxiaoke/packer-ng-plugin)

- [**gradle-packer-plugin**](https://github.com/mcxiaoke/gradle-packer-plugin)

由于**gradle-packer-plugin**年代久远，而且更加的推荐**packer-ng-plugin**所以我就没有对它进心实践了，这里主要讲的是**packer-ng-plugin**和**walle**。好的废话到这里就结束了，下面都是干货。  


-----
## packer-ng-plugin

**packer-ng-plugin** 分为v2和v1由于v2之后是只支持v2签名的，他们的生成方式有些许的不同，所以我们这里分为两部分来进行讲解。

### v1
这里使用的是v1的最近的版本v1.0.9。
