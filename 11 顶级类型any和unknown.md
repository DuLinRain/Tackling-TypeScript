# 11 顶级类型any和unknown
- 11.1 TypeScript中的2个顶级类型
- 11.2 顶级类型`any`
  - 11.2.1 示例：`JSON.parse()`
  - 11.2.2 示例：`String()`
- 11.3 顶级类型`unknow`

在Typecript中，`any`和`unknown`是可以包含所有值的类型。本章我们将探讨它们是什么以及可以用来干啥。

### 11.1 Typecript中的2个顶级类型
`any`和`unknown`是TypeScript中的2个所谓的顶级类型。引用自[维基百科](https://en.wikipedia.org/wiki/Top_type)：

> 顶级类型是通用类型，有时候也被称为通用超类型因为所有其它的类型都是它子类型。大多数情况下，它可以包含任何类型的值。

也就是说，当你把类型视为值的集合的时候，`any`和`unknow`是包含所有值的集合。另外，TypeScript也有最低级的类型`never`，它是空集。

### 11.2 顶级类型any
如果类型是`any`的话，我们可以对它做任何事情：

	function func(value: any) {   
	    // Only allowed for numbers, but they are a subtype of `any`   
	    5 * value;    
	    // Normally the type signature of `value` must contain .propName   
	    value.propName;    
	    // Normally only allowed for Arrays and types with index signatures   
	    value[123]; 
	}

任何类型都可以赋值给`any`：

	let storageLocation: any;  
	storageLocation = null; 
	storageLocation = true; 
	storageLocation = {};


类型为`any`的值可以赋值给任意类型的变量：

	function func(value: any) {   
	    const a: null = value;   
	    const b: boolean = value;   
	    const c: object = value; 
	}

如果设置成`any`的话，我们失去了TypeScript的静态类型检查给我们带来的保护。因此应当只有在迫不得已的时候（比如指定类型或者`unknown`都不可行）才去使用它。

#### 11.2.1 示例：JSON.parse()
`JSON.parse()`的结果取决于它的输入，这也就是为什么它的返回类型是`any`:

	JSON.parse(text: string): any;

`JSON.parse()`是在`unknown`类型存在之前就加入到TypeScript的，否则它的返回类型可能会设为`unknown`。

#### 11.2.2 示例：String()
`String()`可以将任何值转成字符串，所以它的类型定义如下：

	interface StringConstructor {   
	    (value?: any): string; // call signature   
	    // ··· 
	}


### 11.3 顶级类型unknown
类型`unknown`是类型`any`的类型安全版本，当你在任何打算使用`any`的地方，可以先考虑使用`unknown`。

`any`允许我们做任何事情，而`unknown`则会有许多限制。

在我们基于`unknown`类型的值做任何操作之前，必须通过一些手段先缩小它的类型：

- 类型断言

		function func(value: unknown) {   
		    // @ts-expect-error: Object is of type 'unknown'.   
		    value.toFixed(2);    
		    
		    // Type assertion:   
		    (value as number).toFixed(2); // OK 
		}

- 相等性

		function func(value: unknown) {   
		    // @ts-expect-error: Object is of type 'unknown'.   
		    value * 5;    
		    
		    if (value === 123) { // equality     
		        // %inferred-type: 123     
		        value;      
		        value * 5; // OK   
		    } 
		}

- 类型守护

		function func(value: unknown) {   
		    // @ts-expect-error: Object is of type 'unknown'.   
		    value.length;    
		    if (typeof value === 'string') { // type guard     
		        // %inferred-type: string     
		        value;      
		        value.length; // OK   
		    } 
		}


- 断言函数

		function func(value: unknown) {   
		    // @ts-expect-error: Object is of type 'unknown'.   
		    value.test('abc');    
		    
		    assertIsRegExp(value);    // %inferred-type: RegExp 
		      
		    value;   
		     
		    value.test('abc'); // OK 
		}  
		
		
		/** An assertion function */ 
		function assertIsRegExp(arg: unknown): asserts arg is RegExp {   
		    if (! (arg instanceof RegExp)) {     
		        throw new TypeError('Not a RegExp: ' + arg);   
		    } 
		}

