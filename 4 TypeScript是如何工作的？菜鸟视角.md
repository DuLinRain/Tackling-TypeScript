# 4 TypeScript是如何工作的？菜鸟视角

- 4.1 Typescript项目结构
  - 4.1.1 tsconfig.json
- 4.2 使用集成开发环境编写TypeScript
- 4.3 TypeScript编译器生成的其它文件
  - 4.3.1 为了在TypeScript中使用npm包，我们需要类型信息
- 4.4 为纯Javascript文件使用TypeScript编译器


本章将给出菜鸟视角眼里TypeScript是如何工作的：典型TypeScript项目的结构是怎样的？哪些东西被编译了以及是如何编译的？如何使用IDE写TypeScript？

### 4.1 Typescript项目结构

下面是一种可能的TypeScript项目结构：

	typescript-project/
	  dist/
	  ts/
	    src/
	      main.ts
	      util.ts
	    test/
	      util_test.ts
	  tsconfig.json

解释：

- 目录`ts/`包含的是TypeScript文件：
  - 子目录`ts/src/`包含的是实际的项目代码
  - 子目录`ts/test/` 包含的是项目的测试代码
- 目录`dist/`是编译器产物的输出目录
- TypeScript编译器将`ts/`目录中的TypeScript文件编译成`dist/`目录中的JavaScript文件. 例如：
  - `ts/src/main.ts`被编译成`dist/src/main.js`（可能还会有其它文件）
- `tsconfig.json` 用来配置TypeScript编译器

#### 4.1.1 tsconfig.json

`tsconfig.json`的内容看起来像下面这样：

	{
	  "compilerOptions": {
	    "rootDir": "ts",
	    "outDir": "dist",
	    "module": "commonjs",
	    ···
	  }
	}

我们做了如下设置：

- TypeScript代码的根目录是`ts/`
- TypeScript编译器产物的存放目录是`dist/`
- 输出文件的模块格式是CommonJS

### 4.2 使用集成开发环境编写TypeScript

有2种比较流行的JavaScript集成开发环境：

- [Visual Studio Code](https://code.visualstudio.com/) (免费)
- [WebStorm](https://www.jetbrains.com/webstorm/) (付费)


本节所做的论述是关于Visual Studio Code的，但是也可能适用于其他IDE。

需要知道的一个很重要的事实是Visual Studio Code以2种独立的方式处理Visual Studio Code源代码：

- 检查打开文件的错误：这是通过所谓的[语言服务](https://langserver.org/)完成的。语言服务独立于特定编辑器存在，给Visual Studio Code提供语言相关的服务：检查错误信息，重整代码，自动完成等等。与服务之前的通信是通过JSON-RPC协议完成的（RPC表示远程过程调用）。这种独立性意味着服务几乎可以用任意语言实现。
  - 需要记住的重要事实：语言服务只会列出当前打开文件的错误，并不会编译TypeScript，他只会对其进行静态分析。
- 构建（编译TypeScript文件成JavaScript文件）：这里有2个选择：
  - 可以使用外部命令行运行构建工具。比如，TypeScript编译器有 `tsc` 有一种--watch 模式可以监听输入的文件，一旦文件有变化就将其编译输出。所以，无论何时当我们在IDE中保存文件的时候，我们可以立即得到对应的编译产物。
  - 我们可以从Visual Studio Code中运行`tsc`。要想这样做，我们需要同时在当前项目目录和全局安装（通过NodeJS包管理工具npm）TypeScript编译器

  
在构建过程中，我们可以得到完整的错误列表。想要了解更多在Visual Studio Code中编译TypeScript的信息可以查看[官方文档](https://code.visualstudio.com/docs/typescript/typescript-compiling)。

### 4.3 TypeScript编译器生成的其它文件
给定一个`main.ts`文件，TypeScript编译器可以生成各种类型的产物。最常见的是：

- JavaScript文件：`main.js`
- 声明文件：`main.d.ts`（包含类型信息；可以认为是`.ts`文件减去对应JavaScript文件剩下的内容）
- Source map文件：`main.js.map`


TypeScript通常不是以`.ts`文件发布的，而是以`.js` 文件和 `.d.ts` 文件发布，原因主要是：

- JavaScript代码包含实际的功能并且可以被纯JavaScript消费。
- 声明文件可以帮助代码编辑器进行自动补全或者完成其它类似的服务。这个声明文件可以使得纯JavaScript可以通过TypeScript消费。当然，即使是我们只使用纯JavaScript它也可以给我们带来收益，比如更好的自动补全等。


Source map文件指定了输出代码`main.js`中的每个部分是由 `main.ts` 中的哪个部分生成的。除此之外，这个信息也使得运行时环境执行JavaScript代码出错时可以显示对应TypeScript代码中的行号。

#### 4.3.1 为了在TypeScript中使用npm包，我们需要类型信息
npm是JavaScript代码的一个巨大仓库。如果我们想在TypeScript中使用一个JavaScript包，我们需要它的类型信息：

- 包本身可能就包含`.d.ts`文件或者甚至是完整的TypeScript代码
- 否则，我们可能仍然可以使用它：[DefinitelyTyped](https://definitelytyped.org/) 是一个声明文件仓库，包含很多人为纯JavaScript包写的类型声明。
`DefinitelyTyped`中的声明文件在`@types`命名空间下。因此，如果我们想使用`lodash`的类型声明文件，我们需要安装`@types/lodash`包。

### 4.4 为纯Javascript文件使用TypeScript编译器
TypeScript 编译器同样可以处理纯JavaScript 文件：

- 使用--allowJs选项，TypeScript编译器会将输入目录中的JavaScript文件拷贝到输出目录。好处：当从JavaScript向TypeScript迁移时，我们可以从JavaScript和TypeScript混用开始，逐渐的将更多的JavaScript替换成TypeScript。
- 使用--checkJs选项，编译器还会检查JavaScript文件（`--allowJs`必须同时使用）。考虑到编译器得到的信息有限，它只会尽可能的进行类型检查。可以通过注释来配置哪些文件被检查：
  - 显式排除：如果一个JavaScript文件包含`// @ts-nocheck`注释，那它不会被类型检查。
  - 显式包含：在没有`--checkJs`选项时，`// @ts-check` 注释可以用来检查单个的JavaScript文件。
- TypeScript编译器也可以使用JSDoc注释（例如下面的示例）中指定的静态类型信息。如果我们这么做，我们可以完全静态地定义纯JavaScript文件的类型并且可以进而得到声明文件。
- 使用`--noEmit`选项，编译器不会有任何输出，它只会对文件进行类型检查。


这是一个使用JSDoc注释为函数`add()`提供静态类型信息的例子：

	/**
	 * @param {number} x - The first operand
	 * @param {number} y - The second operand
	 * @returns {number} The sum of both operands
	 */
	function add(x, y) {
	  return x + y;
	}

更多信息：JavaScript[文件类型检查](https://www.typescriptlang.org/docs/handbook/type-checking-javascript-files.html)。