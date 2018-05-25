# axe-fastlane

Axe auto tools using fastlane

使用`fastlane` 提供几个便捷的命令以处理组件化接入时的一些问题。在用户开发的模块或者APP中， 通过以下指令进行引入，以实时更新。

	import_from_git(url: 'https://github.com/axe-org/fastlane',  branch: 'v0.2')
	
注意版本号，由于`Axe`系统包括内容较多，所以我们使用版本号来整理依赖情况:

	v0.2的fastlane 使用于 v0.2的后台， 与其他组件。

 创建`minor`版本号标记的分支来做协作开发，而各个部分依然有自己的tag来发布修复`bug`版本。


# 指令列表 

### `axe_pod_install`

安装依赖。 对于`module`和`app`类型，拥有业务组件依赖，所以必须通过该命令安装依赖。 其依赖配置在`Podfile.rb` 文件中，本地不放 `Podfile`文件，`Podfile`文件在`pod install`时自动生成。 所以模块和app的依赖都写在`Podfile.rb` 文件中。

而对于`ground`类型， 其没有业务组件依赖，所以还是使用`Podfile`结构。 两种处理方式是可以接受的，因为开发基础组件的和 开发业务组件的， 是有一定隔离的。

拥有两个参数：

* `production` : 标记使用prd版本还是beta版本的依赖，默认为 false
* `source` : 标记是否使用源码引入，默认为 false。

手动安装时，可以快速执行命令： 

	fastlane axe_pod_install production:true source:true

### `axe_module_build`

构建模块，进行模块构建。 需要先安装`Cocoapods`依赖。

对于模块，其版本发布也由`axe`来处理， 使用 `xxxx.podspec.rb`文件，而不是`xxx.podspec`,。 在发布的时候，根据设定生成相应的`podspec`文件以发布。

该部分两个有意思的地方在于：

* 头文件处理 ： 对于被合并到`ground`组件中的 更底层的基础组件，我们将其他头文件正确处理，以使开发者可以正常引用底层的基础组件。
* 依赖绘制 ： 分析依赖，放在`axe/dependency.dot`中，并使用 [graphviz](http://www.graphviz.org)绘制成图。下图示例 ：

![](http://resource.luoxianming.cn/jOtC94LJ7y2GlzLjZzHnS.svg)

参数：

* `publish` : 默认为false,避免本地自动测试提交。
* `production` : 表示发布的版本，需要设定publish。 默认为false, 表示beta版本，会自动增加版本号，注意生产版本只能发布一次。

手动安装时，可以快速执行命令，以设定参数： 

	fastlane axe_module_build production:true publish:true

### `axe_import_module`

接入`axe`的APP，通过该指令来管理 其他组件的接入。

参数：

* `import` : 引入的模块列表，为json字符串格式，如 `{"login":"0.0.1"}` , `key`为模块名，`value`为模块版本。
* `remove` : 删除的模块列表， 为json字符串格式，如 `["login"]` ,表示删除Login模块。

### `axe_init_module`

脚手架支持， 以创建一个标准的模块仓库。目前只能创建`module`类型，`ground`类型和`app`类型还是要手动创建`axe`文件夹。

# TODO

* 优化APP的依赖分析， 可以考虑绘制成结构图，而不是依赖图
* 优化绘制图形，添加颜色。
*  自动化测试 ：  所有组件都拥有自动化测试的要求，并进行真机测试后，才能进行发布。  基础组件编写单元测试用例， 而业务组件编写的UI测试，还要接入截图功能，以供分析，确保测试效果。 自动化测试是极其重要的部分， 可以借助fastlane的截图工具，做到自动测试业务，截图分析。 自动化测试视为平台化最重要的成果， 第二个版本时，主要开发自动化测试部分。
* js支持， 自动将 ios的头文件转换成 js格式， 也能自动将 js的头文件转换为iOS的形式，同时自动发布到npm或者Cocoapods上， 以提高跨容器的协作开发能力。