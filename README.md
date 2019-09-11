# data_hacking for python 3.6

如题所示，本项目试图为[data_hacking](https://github.com/SuperCowPowers/data_hacking)进行针对python3.6适配。

2019持续更新。

## 关于原项目

请移步至原作者提供的[README](./ORIGIN_README.md)。

## 关于开发环境

- 推荐使用conda进行管理，避免pip使用失误造成的版本冲突和依赖断裂问题。同时，使用conda安装tensorflow更为简单可靠。
- 本仓库提供了package-list，请使用以下命令快速创建虚假环境。

  ```bash
  conda  create -n <env name> -f package-list
  ```

- 经排除，conda缺失`pygraphviz`和`tldextract`包。前者可忽略，因其并未被在项目中实际使用。后者为dga_detection实验的重要依赖项，**暂无较好的解决方案**。
- 由于我们使用conda作为包管理器，所以不要尝试听从原作者的setup建议，否则会污染虚拟环境。我们应当仅仅将内部的`data_hacking`包作为项目内私有包使用。
- 你可以直接使用jupyter notebook或jupyterlab进行本项目的开发，享受原生的ipynb文件使用体验。你也可以使用vscode，其使用带cell注释的py文件实现了与ipython的良好交互体验。vscode提供了ipynb文件至自有cell注释py文件间的相互转换。

## 主要问题与解决方案

- >语法和自有api差异，python2至3的语法发生了较大断裂。典型的例子为`print`方法的变化。
  - 手工修正相关语法差异，移步至[记录表](###语法和自有api差异修正记录)。
- >api断裂，原有代码所依赖的部分第三方库发生了api变动。
  - 替换为相应的新api，具体变动记录移步至[记录表](###第三方api变动)。
- >`modules`导入问题。原作者试图按层次组织自己编写的模块`data_hacking`，并在`__init__.py`写入了一系列`import`语句，给python3下的导入造成了麻烦。
  - 删除`data_hacking`模块所有`__init__.py`中的`import`语句，并相应的修改笔记中的导入语句。
- > 文件编码问题造成读入出错。
  - 指定正确的编码，目前已知可用编码`'ISO-8859-1'`。
- >jupyter在执行代码块时，存在自动更改解释器运行路径至当前文件所在文件夹的行为。该行为无法更改，这给jupyter下的python解释器的模块导入造成了不小的麻烦。
  - 尽管我们无法更改jupyter这一不合理行为，但是可以通过在执行导入前将`data_hacking`模块所在的文件夹路径添加到环境变量`path`来曲线救国。具体修改记录见相关文件同级文件夹下的 changes 文档。
  - 当然，你也可以选择vscode打开本项目（陆续提供同名）。vscode同样支持与ipython的良好交互，只不过其采用了自有注释语法来实现cell的识别。尽管vscode并不支持直接使用ipynb文件，但其提供了ipynb文件至自有cell注释py文件间的相互转换。在vscode中，请确保设置了以下选项以避免执行cell时cwd的改变：

  ```json
  {
      "python.dataScience.changeDirOnImportExport": false
  }
  ```

### 语法和自有api差异修正记录

|  python2.7  | python3.6  |
| :---------: | :--------: |
|    pint     | print(..)  |
|     str     |   bytes    |
|   unicode   |    str     |
| xrange(...) | range(...) |

### 第三方api变动

| package name | python2.7      | python3.6            |
| ------------ | -------------- | -------------------- |
| pandas       | DataFrame.sort | DataFrame.sort_index |
| pandas       | Series.order   | Series.sort_values   |
