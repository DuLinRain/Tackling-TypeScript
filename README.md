# Tackling TypeScript中文
Tackling TypeScript中文翻译

### 章节

- 4 TypeScript是如何工作的？菜鸟视角
  - 4.1 Typescript项目结构
    - 4.1.1 tsconfig.json
  - 4.2 使用集成开发环境编写TypeScript
  - 4.3 TypeScript编译器生成的其它文件
    - 4.3.1 为了在TypeScript中使用npm包，我们需要类型信息
  - 4.4 为纯Javascript文件使用TypeScript编译器
- 5 开始尝试TypeScript
  - 5.1 TypeScript演练场
  - 5.2 TS Node
- 6 本书约定
  - 6.1 测试断言（动态）
  - 6.2 类型断言（静态）
- 19 数组类型
  - 19.1 数组的角色
  - 19.2 定义数组类型的方式
    - 19.2.1 列表角色：数组类型字面量 VS 接口类型Array
    - 19.2.2 元组角色：元组类型字面量
    - 19.2.3 形如数组的对象：带索引语法的接口
  - 19.3 陷阱：类型接口并不总是能够得到正确的数组类型
    - 19.3.1 推导数组的类型是很困难的
    - 19.3.2 对非空数组字面量的类型推导
    - 19.3.3 对空数组字面量的推导
    - 19.3.4 const 断言数组和类型接口
      - 19.3.4.1 const断言的潜在陷阱
  - 19.4 陷阱：TypeScript会假设索引永不越界
