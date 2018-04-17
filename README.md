# axe-fastlane

Axe auto tools using fastlane

使用`fastlane` 提供几个便捷的命令以处理组件化接入时的一些问题。在用户开发的模块或者APP中， 通过以下指令进行引入，以实时更新。

	import_from_git(url: 'https://github.com/axe-org/fastlane')

# 指令列表 

### `axe_pod`

安装依赖。 对于`module`和`app`类型，拥有业务组件依赖，所以必须通过该命令安装依赖。 其依赖配置在`Podfile.rb` 文件中，本地不放 `Podfile`文件，`Podfile`文件在`pod install`时自动生成。 所以模块和app的依赖都写在`Podfile.rb` 文件中。

而对于`ground`类型， 其没有业务组件依赖，所以还是使用`Podfile`结构。 两种处理方式是可以接受的，因为开发基础组件的和 开发业务组件的， 是有一定隔离的。

拥有两个参数：

* `production` : 标记使用prd版本还是beta版本的依赖，默认为 false
* `source` : 标记是否使用源码引入，默认为 false。

手动安装时，可以快速执行命令： 

	fastlane axe_pod production:true source:true

### `axe_module_build`

构建模块，进行模块构建。 需要先安装`Cocoapods`依赖。

对于模块，其版本发布也由`axe`来处理， 使用 `xxxx.podspec.rb`文件，而不是`xxx.podspec`,。 在发布的时候，根据设定生成相应的`podspec`文件以发布。

该部分两个有意思的地方在于：

* 依赖绘制 ： 分析依赖，放在`.axe/dependency.dot`中，并使用 [graphviz](http://www.graphviz.org)绘制成图。
* 头文件处理 ： 对于被合并到`ground`组件中的 更底层的基础组件，我们将其他头文件正确处理，以使开发者可以正常引用底层的基础组件。

目前未完成的功能点：

* 自动化测试 ： TODO， 所有组件都拥有自动化测试的要求，并进行真机测试后，才能进行发布。  基础组件编写单元测试用例， 而业务组件编写的UI测试，还要接入截图功能，以供分析，确保测试效果。

参数：

* `publish` : 默认为false,避免本地自动测试提交。
* `production` : 表示发布的版本，需要设定publish。 默认为false, 表示beta版本，会自动增加版本号，注意生产版本只能发布一次。

手动安装时，可以快速执行命令，以设定参数： 

	fastlane axe_pod production:true publish:true

### `axe_import_module`

接入`axe`的APP，通过该指令来管理 其他组件的接入。

> 暂未完成

参数：


