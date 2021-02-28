# 5 开始尝试TypeScript

- 5.1 TypeScript演练场
- 5.2 TS Node

这一章给出快速尝试TypeScript的小技巧。

### 5.1 TypeScript演练场
[TypeScript演练场](http://www.typescriptlang.org/play/)是一个在线的TypeScript代码编辑器，包含如下功能：

- 支持完整的IDE风格的编辑：自动补全等
- 显示静态类型错误
- 展示TypeScript代码编译到JavaScript的结果。也可以在浏览器中执行结果。

演练场对于快速实验和写demo非常有帮助。它也可以同时将TypeScript代码片段和编译器配置保存在URL中，这对于分析代码片段给其它人非常有帮助。这是一个这样的URL的示例：

	https://www.typescriptlang.org/play/#code/MYewdgzgLgBFDuBLYBTGBeGAKAHgLhmgCdEwBzASgwD4YcYBqOgbgChXRIQAbFAOm4gyWBMhRYA5AEMARsAkUKzIA

### 5.2 TS Node
[TS Node](https://github.com/TypeStrong/ts-node) 是NodeJs的TypeScript版本，它的使用场景包含：

- 为TypeScript提供了一个REPL命令行：

		$ ts-node
		> const twice = (x: string) => x + x;
		> twice('abc')
		'abcabc'
		> twice(123)
		Error TS2345: Argument of type '123' is not assignable
		to parameter of type 'string'.

- TS Node使得一些JavaScript工具可以直接执行TypeScript代码。它会自动地将TypeScript代码转换成JavaScript代码并且将其传递给这些工具，不需要额外做任何事情。下面的`shell`指令证明如果使用JavaScript单元测试框架Mocha：

		mocha --require ts-node/register --ui qunit testfile.ts



无需安装即可使用`npx ts-node`运行REPL。