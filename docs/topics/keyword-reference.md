[//]: # (title: Keywords and operators)

## Hard keywords
硬关键字

The following tokens are always interpreted as keywords and cannot be used as identifiers:
<br />以下标记始终被解释为关键字，不能用作标识符：

 * `as`
     - is used for [type casts](typecasts.md#unsafe-cast-operator).
       <br />用于[类型转换](typecasts.md#unsafe-cast-operator)。
     - specifies an [alias for an import](packages.md#imports)
       <br />指定[导入的别名](packages.md#imports)
 * `as?` is used for [safe type casts](typecasts.md#unsafe-cast-operator).
   <br />用于[安全类型转换](typecasts.md#unsafe-cast-operator)。
 * `break` [terminates the execution of a loop](returns.md).
   <br />[终止循环的执行](returns.md)。
 * `class` declares a [class](classes.md).
   <br />声明一个[类](classes.md)。
 * `continue` [proceeds to the next step of the nearest enclosing loop](returns.md).
   <br />[继续执行最近的封闭循环的下一步](returns.md)。
 * `do` begins a [do/while loop](control-flow.md#while-loops) (a loop with a postcondition).
   <br />开始一个[do/while 循环](control-flow.md#while-loops)（一个带有后置条件的循环）。
 * `else` defines the branch of an [if expression](control-flow.md#if-expression) that is executed when the condition is false.
   <br />定义当条件为假时执行的 [if 表达式](control-flow.md#if-expression) 分支。
 * `false` specifies the 'false' value of the [Boolean type](booleans.md).
   <br />指定 [布尔类型](booleans.md)的“false”值。
 * `for` begins a [for loop](control-flow.md#for-loops).
   <br />开始一个[for循环](control-flow.md#for-loops)。
 * `fun` declares a [function](functions.md).
   <br />声明一个[函数](functions.md)。
 * `if` begins an [if expression](control-flow.md#if-expression).
   <br />开始一个 [if 表达式](control-flow.md#if-expression)。
 * `in`
     - specifies the object being iterated in a [for loop](control-flow.md#for-loops).
       <br />指定在 [for 循环](control-flow.md#for-loops) 中迭代的对象。
     - is used as an infix operator to check that a value belongs to [a range](ranges.md),
       a collection, or another entity that [defines a 'contains' method](operator-overloading.md#in-operator).
       <br />用作中缀运算符，以检查值是否属于 [范围](ranges.md)、 集合或其他 [定义了 'contains' 方法](operator-overloading.md#in-operator) 的实体。
     - is used in [when expressions](control-flow.md#when-expressions-and-statements) for the same purpose.
       <br />在 [when 表达式](control-flow.md#when-expressions-and-statements) 中也用于相同的目的。
     - marks a type parameter as [contravariant](generics.md#declaration-site-variance).
       <br />将类型参数标记为[逆变体](generics.md#declaration-site-variance)。
 * `!in`
     - is used as an operator to check that a value does NOT belong to [a range](ranges.md),
       a collection, or another entity that [defines a 'contains' method](operator-overloading.md#in-operator).
       <br />用作运算符，以检查值是否不属于 [范围](ranges.md)、集合或其他 [定义了 'contains' 方法](operator-overloading.md#in-operator) 的实体。
     - is used in [when expressions](control-flow.md#when-expressions-and-statements) for the same purpose.
       <br />在 [when 表达式](control-flow.md#when-expressions-and-statements) 中也用于相同的目的。
 * `interface` declares an [interface](interfaces.md).
   <br />声明一个[接口](interfaces.md)。
 * `is`
     - checks that [a value has a certain type](typecasts.md#is-and-is-operators).
       <br />检查[值是否具有某种类型](typecasts.md#is-and-is-operators)。
     - is used in [when expressions](control-flow.md#when-expressions-and-statements) for the same purpose.
       <br />在 [when 表达式](control-flow.md#when-expressions-and-statements) 中也用于相同的目的。
 * `!is`
     - checks that [a value does NOT have a certain type](typecasts.md#is-and-is-operators).
       <br />检查[值不具有特定类型](typecasts.md#is-and-is-operators)。
     - is used in [when expressions](control-flow.md#when-expressions-and-statements) for the same purpose.
       <br />在 [when 表达式](control-flow.md#when-expressions-and-statements) 中也用于相同的目的。
 * `null` is a constant representing an object reference that doesn't point to any object.
   <br />是一个常量，表示一个不指向任何对象的对象引用。
 * `object` declares [a class and its instance at the same time](object-declarations.md).
   <br />声明[一个类及其实例](object-declarations.md)。
 * `package` specifies the [package for the current file](packages.md).
   <br />指定当前文件的包（packages.md）。
 * `return` [returns from the nearest enclosing function or anonymous function](returns.md).
   <br />[从最近的封闭函数或匿名函数返回](returns.md)。
 * `super`
     - [refers to the superclass implementation of a method or property](inheritance.md#calling-the-superclass-implementation).
       <br />[指方法或属性的超类实现](inheritance.md#calling-the-superclass-implementation)
     - [calls the superclass constructor from a secondary constructor](classes.md#inheritance).
       <br />[从辅助构造函数调用超类构造函数](classes.md#inheritance)
 * `this`
     - refers to [the current receiver](this-expressions.md).
       <br />指的是[当前接收者](this-expressions.md)。
     - [calls another constructor of the same class from a secondary constructor](classes.md#constructors-and-initializer-blocks).
       <br />[从辅助构造函数调用同一类的另一个构造函数](classes.md#constructors-and-initializer-blocks)。
 * `throw` [throws an exception](exceptions.md).
   <br />[抛出异常](exceptions.md)
 * `true` specifies the 'true' value of the [Boolean type](booleans.md).
   <br />指定 [布尔类型](booleans.md)的 'true' 值。
 * `try` [begins an exception-handling block](exceptions.md).
   <br />[开始异常处理块](exceptions.md)。
 * `typealias` declares a [type alias](type-aliases.md).
   <br />声明一个[类型别名](type-aliases.md)。
 * `typeof` is reserved for future use.
   <br />保留供将来使用。
 * `val` declares a read-only [property](properties.md) or [local variable](basic-syntax.md#variables).
   <br />声明一个只读的[属性](properties.md)或[局部变量](basic-syntax.md#variables)。
 * `var` declares a mutable [property](properties.md) or [local variable](basic-syntax.md#variables).
   <br />声明一个可变的[属性](properties.md)或[局部变量](basic-syntax.md#variables)。
 * `when` begins a [when expression](control-flow.md#when-expressions-and-statements) (executes one of the given branches).
   <br />开始一个 [when 表达式](control-flow.md#when-expressions-and-statements)（执行给定分支之一）。
 * `while` begins a [while loop](control-flow.md#while-loops) (a loop with a precondition).
   <br />开始一个[while循环](control-flow.md#while-loops)（一个带有前提条件的循环）。

## Soft keywords
软关键字

The following tokens act as keywords in the context in which they are applicable, and they can be used
as identifiers in other contexts:
<br />以下标记在其适用的上下文中用作关键词，并且在其他上下文中可用作标识符：

 * `by`
     - [delegates the implementation of an interface to another object](delegation.md).
       <br />[将接口的实现委托给另一个对象](delegation.md)。
     - [delegates the implementation of the accessors for a property to another object](delegated-properties.md).
       <br />[将属性访问器的实现委托给另一个对象](delegated-properties.md)
 * `catch` begins a block that [handles a specific exception type](exceptions.md).
   <br />开始一个[处理特定异常类型的](exceptions.md)代码块。
 * `constructor` declares a [primary or secondary constructor](classes.md#constructors-and-initializer-blocks).
   <br />声明一个[主构造函数或辅助构造函数](classes.md#constructors-and-initializer-blocks)
 * `delegate` is used as an [annotation use-site target](annotations.md#annotation-use-site-targets).
   <br />用作 [注释使用站点目标](annotations.md#annotation-use-site-targets)。
 * `dynamic` references a [dynamic type](dynamic-type.md) in Kotlin/JS code.
   <br />引用 Kotlin/JS 代码中的[动态类型](dynamic-type.md)。
 * `field`
     - declares an [explicit backing field](properties.md#explicit-backing-fields).
       <br />声明一个[显式支持字段](properties.md#explicit-backing-fields)。
     - is used as an [annotation use-site target](annotations.md#annotation-use-site-targets).
       <br />用作 [注释使用站点目标](annotations.md#annotation-use-site-targets)。
 * `file` is used as an [annotation use-site target](annotations.md#annotation-use-site-targets).
   <br />被用作 [注释使用站点目标](annotations.md#annotation-use-site-targets)。
 * `finally` begins a block that [is always executed when a try block exits](exceptions.md).
   <br />开始一个代码块，该代码块[在 try 代码块退出时总是执行](exceptions.md)。
 * `get`
     - declares the [getter of a property](properties.md).
       <br />声明属性的[getter](properties.md)。
     - is used as an [annotation use-site target](annotations.md#annotation-use-site-targets).
       <br />用作 [注释使用站点目标](annotations.md#annotation-use-site-targets)。
 * `import` [imports a declaration from another package into the current file](packages.md).
   <br />[将另一个包中的声明导入到当前文件中](packages.md)。
 * `init` begins an [initializer block](classes.md#constructors-and-initializer-blocks).
   <br />开始一个[初始化块](classes.md#constructors-and-initializer-blocks)。
 * `param` is used as an [annotation use-site target](annotations.md#annotation-use-site-targets).
   <br />用作 [注释使用站点目标](annotations.md#annotation-use-site-targets)。
 * `property` is used as an [annotation use-site target](annotations.md#annotation-use-site-targets).
   <br />用作 [注释使用站点目标](annotations.md#annotation-use-site-targets)。
 * `receiver` is used as an [annotation use-site target](annotations.md#annotation-use-site-targets).
   <br />用作 [注释使用站点目标](annotations.md#annotation-use-site-targets)。
 * `set`
     - declares the [setter of a property](properties.md).
       <br />声明[属性的设置器](properties.md)。
     - is used as an [annotation use-site target](annotations.md#annotation-use-site-targets).
       <br />用作[注释使用站点目标](annotations.md#annotation-use-site-targets)
* `setparam` is used as an [annotation use-site target](annotations.md#annotation-use-site-targets).
  <br />用作 [注释使用站点目标](annotations.md#annotation-use-site-targets)。
* `value` with the `class` keyword declares an [inline class](inline-classes.md).
  <br />`value` 与 `class` 关键字声明一个[内联类](inline-classes.md)。
* `where` specifies the [constraints for a generic type parameter](generics.md#upper-bounds).
  <br />指定[泛型类型参数的约束](generics.md#upper-bounds)。

## Modifier keywords
修饰关键字

The following tokens act as keywords in modifier lists of declarations, and they can be used as identifiers
in other contexts:
<br />以下标记在声明的修饰符列表中充当关键字，并且可以在其他上下文中用作标识符：

 * `abstract` marks a class or member as [abstract](classes.md#abstract-classes).
   <br />`abstract` 将类或成员标记为 [抽象的](classes.md#abstract-classes)。
 * `actual` denotes a platform-specific implementation in [multiplatform projects](https://kotlinlang.org/docs/multiplatform/multiplatform-expect-actual.html).
   <br />表示 [多平台项目](https://kotlinlang.org/docs/multiplatform/multiplatform-expect-actual.html) 中的平台特定实现。
 * `annotation` declares an [annotation class](annotations.md).
   <br />声明一个 [annotation 类](annotations.md)。
 * `companion` declares a [companion object](object-declarations.md#companion-objects).
   <br />声明一个 [伴生对象](object-declarations.md#companion-objects)。
 * `const` marks a property as a [compile-time constant](properties.md#compile-time-constants).
   <br />将属性标记为[编译时常量](properties.md#compile-time-constants)。
 * `crossinline` forbids [non-local returns in a lambda passed to an inline function](inline-functions.md#returns).
   <br />禁止 [在传递给内联函数的 lambda 表达式中使用非局部返回值](inline-functions.md#returns)。
 * `data` instructs the compiler to [generate canonical members for a class](data-classes.md).
   <br />指示编译器[为类生成规范成员](data-classes.md)。
 * `enum` declares an [enumeration](enum-classes.md).
   <br />声明一个[枚举](enum-classes.md)。
 * `expect` marks a declaration as [platform-specific](https://kotlinlang.org/docs/multiplatform/multiplatform-expect-actual.html), expecting an implementation in platform modules.
   <br />将声明标记为 [平台特定的](https://kotlinlang.org/docs/multiplatform/multiplatform-expect-actual.html)，期望在平台模块中实现。
 * `external` marks a declaration as implemented outside of Kotlin (accessible through [JNI](java-interop.md#using-jni-with-kotlin) or in [JavaScript](js-interop.md#external-modifier)).
   <br />表示声明是在 Kotlin 之外实现的（可通过 [JNI](java-interop.md#using-jni-with-kotlin) 或在 [JavaScript](js-interop.md#external-modifier) 中访问）。
 * `final` forbids [overriding a member](inheritance.md#overriding-methods).
   <br />禁止[重写成员](inheritance.md#overriding-methods)。
 * `infix` allows calling a function using [infix notation](functions.md#infix-notation).
   <br />允许使用[中缀表示法](functions.md#infix-notation)调用函数。
 * `inline` tells the compiler to [inline a function and the lambdas passed to it at the call site](inline-functions.md).
   <br />告诉编译器[在调用点内联函数及其传递的 lambda 表达式](inline-functions.md)。
 * `inner` allows referring to an outer class instance from a [nested class](nested-classes.md).
   <br />允许从[嵌套类](nested-classes.md)中引用外部类实例。
 * `internal` marks a declaration as [visible in the current module](visibility-modifiers.md).
   <br />将声明标记为[在当前模块中可见](visibility-modifiers.md)。
 * `lateinit` allows initializing a [non-nullable property outside of a constructor](properties.md#late-initialized-properties-and-variables).
   <br />允许在构造函数之外初始化一个[不可为空的属性](properties.md#late-initialized-properties-and-variables)。
 * `noinline` turns off [inlining of a lambda passed to an inline function](inline-functions.md#noinline).
   <br />关闭[传递给内联函数的 lambda 的内联](inline-functions.md#noinline)。
 * `open` allows [subclassing a class or overriding a member](classes.md#inheritance).
   <br />允许[继承类或重写成员](classes.md#inheritance)。
 * `operator` marks a function as [overloading an operator or implementing a convention](operator-overloading.md).
   <br />将函数标记为 [重载运算符或实现约定](operator-overloading.md)。
 * `out` marks a type parameter as [covariant](generics.md#declaration-site-variance).
   <br />将类型参数标记为 [协变量](generics.md#declaration-site-variance)。
 * `override` marks a member as an [override of a superclass member](inheritance.md#overriding-methods).
   <br />将成员标记为 [超类成员的重写](inheritance.md#overriding-methods)。
 * `private` marks a declaration as [visible in the current class or file](visibility-modifiers.md).
   <br />将声明标记为[在当前类或文件中可见](visibility-modifiers.md)。
 * `protected` marks a declaration as [visible in the current class and its subclasses](visibility-modifiers.md).
   <br />将声明标记为[在当前类及其子类中可见](visibility-modifiers.md)。
 * `public` marks a declaration as [visible anywhere](visibility-modifiers.md).
   <br />将声明标记为[在任何地方可见](visibility-modifiers.md)。
 * `reified` marks a type parameter of an inline function as [accessible at runtime](inline-functions.md#reified-type-parameters).
   <br />将内联函数的类型参数标记为 [可在运行时访问](inline-functions.md#reified-type-parameters)。
 * `sealed` declares a [sealed class](sealed-classes.md) (a class with restricted subclassing).
   <br />声明一个[密封类](sealed-classes.md)（一个子类化受限的类）。
 * `suspend` marks a function or lambda as suspending (usable as a [coroutine](coroutines-overview.md)).
   <br />将函数或 lambda 标记为挂起（可用作[协程](coroutines-overview.md)）。
 * `tailrec` marks a function as [tail-recursive](functions.md#tail-recursive-functions) (allowing the compiler to replace recursion with iteration).
   <br />将函数标记为[尾递归](functions.md#tail-recursive-functions)（允许编译器用迭代替换递归）。
 * `vararg` allows [passing a variable number of arguments for a parameter](functions.md#variable-number-of-arguments-varargs).
   <br />允许[为参数传递可变数量的参数](functions.md#variable-number-of-arguments-varargs)。

## Special identifiers
特殊标识符

The following identifiers are defined by the compiler in specific contexts, and they can be used as regular
identifiers in other contexts:
<br />以下标识符由编译器在特定上下文中定义，在其他上下文中可用作常规标识符：

 * `field` is used inside a property accessor to refer to the [backing field of the property](properties.md#backing-fields).
   <br />`field` 在属性访问器内部使用来引用 [属性的支持字段](properties.md#backing-fields)。
 * `it` is used inside a lambda to [refer to its parameter implicitly](lambdas.md#it-implicit-name-of-a-single-parameter).
   <br />`it` 在 lambda 表达式内部用于[隐式引用其参数](lambdas.md#it-implicit-name-of-a-single-parameter)。

## Operators and special symbols
运算符和特殊符号

Kotlin supports the following operators and special symbols:
<br />Kotlin 支持以下运算符和特殊符号：

 * `+`, `-`, `*`, `/`, `%` - mathematical operators
       <br />数学运算符
     - `*` is also used to [pass an array to a vararg parameter](functions.md#variable-number-of-arguments-varargs).
       <br />`*` 也用于[将数组传递给可变参数](functions.md#variable-number-of-arguments-varargs)。
 * `=`
     - assignment operator.
       <br />赋值运算符。
     - is used to specify [default values for parameters](functions.md#parameters-with-default-values).
       <br />用于指定[参数的默认值](functions.md#parameters-with-default-values)。
 * `+=`, `-=`, `*=`, `/=`, `%=` - [augmented assignment operators](operator-overloading.md#augmented-assignments).
   <br />[增强赋值运算符](operator-overloading.md#augmented-assignments)。
 * `++`, `--` - [increment and decrement operators](operator-overloading.md#increments-and-decrements).
   <br />[递增和递减运算符](operator-overloading.md#increments-and-decrements)。
 * `&&`, `||`, `!` - logical 'and', 'or', 'not' operators (for bitwise operations, use the corresponding [infix functions](numbers.md#bitwise-operations) instead).
   <br />逻辑'and', 'or', 'not' 运算符（对于位运算，请改用相应的[中缀函数](numbers.md#bitwise-operations)）。
 * `==`, `!=` - [equality operators](operator-overloading.md#equality-and-inequality-operators) (translated to calls of `equals()` for non-primitive types).
   <br />[相等运算符](operator-overloading.md#equality-and-inequality-operators)（对于非原始类型，转换为调用 `equals()`）。
 * `===`, `!==` - [referential equality operators](equality.md#referential-equality).
   <br />[引用相等运算符](equality.md#referential-equality)。
 * `<`, `>`, `<=`, `>=` - [comparison operators](operator-overloading.md#comparison-operators) (translated to calls of `compareTo()` for non-primitive types).
   <br />[比较运算符](operator-overloading.md#comparison-operators)（对于非原始类型，转换为对 `compareTo()` 的调用）。
 * `[`, `]` - [indexed access operator](operator-overloading.md#indexed-access-operator) (translated to calls of `get` and `set`).
   <br />[索引访问运算符](operator-overloading.md#indexed-access-operator)（转换为对 `get` 和 `set` 的调用）。
 * `!!` [asserts that an expression is non-nullable](null-safety.md#not-null-assertion-operator).
   <br />[断言表达式不可为空](null-safety.md#not-null-assertion-operator)。
 * `?.` performs a [safe call](null-safety.md#safe-call-operator) (calls a method or accesses a property if the receiver is non-nullable).
   <br />执行[安全调用](null-safety.md#safe-call-operator)（如果接收器不可为空，则调用方法或访问属性）。
 * `?:` takes the right-hand value if the left-hand value is null (the [elvis operator](null-safety.md#elvis-operator)).
   <br />如果左侧值为空，则取右侧值（[elvis运算符](null-safety.md#elvis-operator)）。
 * `::` creates a [member reference](reflection.md#function-references) or a [class reference](reflection.md#class-references).
   <br />创建[成员引用](reflection.md#function-references)或[类引用](reflection.md#class-references)。
 * `.`
     - accesses [members](classes.md), including [nested classes](nested-classes.md) and [enum entries](enum-classes.md#working-with-enum-constants).
       <br />访问[成员](classes.md)，包括[嵌套类](nested-classes.md)和[枚举条目](enum-classes.md#working-with-enum-constants)。
     - defines and calls [extensions](extensions.md).
       <br />定义并调用[扩展](extensions.md)。
     - qualifies names and [packages](packages.md).
       <br />限定名称和[包](packages.md)。
     - separates the integer and fractional parts of a [floating-point literal](numbers.md#floating-point-types).
       <br />分隔[浮点字面量](numbers.md#floating-point-types)的整数部分和小数部分。
 * `..`, `..<` create [ranges](ranges.md).
   <br />创建 [范围](ranges.md)
 * `:` separates a name from a type in a declaration.
   <br />在声明中，用于分隔名称和类型。
 * `?` marks a type as [nullable](null-safety.md#nullable-types-and-non-nullable-types).
   <br />将类型标记为[可为空](null-safety.md#nullable-types-and-non-nullable-types)。
 * `->`
     - separates the parameters and body of a [lambda expression](lambdas.md#lambda-expression-syntax).
       <br />分隔 [lambda 表达式](lambdas.md#lambda-expression-syntax) 的参数和主体。
     - separates the parameters and return type declaration in a [function type](lambdas.md#function-types).
       <br />在[函数类型](lambdas.md#function-types)中分隔参数和返回类型声明。
     - separates the condition and body of a [when expression](control-flow.md#when-expressions-and-statements) branch.
       分<br />隔 [when 表达式](control-flow.md#when-expressions-and-statements) 分支的条件和主体。
 * `@`
     - introduces an [annotation](annotations.md#usage).
       <br />引入[注解](annotations.md#usage)。
     - introduces or references a [loop label](returns.md#break-and-continue-labels).
       <br />引入或引用[循环标签](returns.md#break-and-continue-labels)。
     - introduces or references a [lambda label](returns.md#return-to-labels).
       <br />引入或引用 [lambda 标签](returns.md#return-to-labels)。
     - references a ['this' expression from an outer scope](this-expressions.md#qualified-this).
       <br />引用[外部作用域中的 'this' 表达式](this-expressions.md#qualified-this)。
     - references an [outer superclass](inheritance.md#calling-the-superclass-implementation).
       <br />引用一个[外部超类](inheritance.md#calling-the-superclass-implementation)。
 * `;` separates multiple statements on the same line.
   <br />分隔同一行中的多个语句。
 * `$` references a variable or expression in a [string template](strings.md#string-templates).
   <br />引用[字符串模板](strings.md#string-templates)中的变量或表达式。
 * `_`
     - substitutes an unused parameter in a [lambda expression](lambdas.md#underscore-for-unused-variables).
       <br />在 [lambda 表达式](lambdas.md#underscore-for-unused-variables) 中替换未使用的参数。
     - substitutes an unused parameter in a [destructuring declaration](destructuring-declarations.md#underscore-for-unused-variables).
       <br />在[解构声明](destructuring-declarations.md#underscore-for-unused-variables)中替换未使用的参数。

For operator precedence, see [this reference](https://kotlinlang.org/grammar/#expressions) in Kotlin grammar.
<br />有关运算符优先级，请参阅 Kotlin 语法中的[此参考](https://kotlinlang.org/grammar/#expressions)。
