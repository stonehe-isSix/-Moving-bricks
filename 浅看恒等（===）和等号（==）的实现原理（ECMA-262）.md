
# 浅看恒等（===）和等号（==）的实现原理（ECMA-262）

#### 恒等（===）Strict Equals Operator实际运算过程。

          假设有表达式 a === b
          ·  计算出表达式 a 的结果，并存入 lref 变量
          ·  将 GetValue(lref) 的结果存入 lval 变量
          ·  计算出表达式 b 的结果，并存入 rref 变量
          ·  将 GetValue(rref) 的结果存入 rval 变量
          ·  执行 Strict Equality Comparison 算法判断 rval === lval 并将结果直接返回
          这里的 Strict Equality Comparison 算法很关键，假设要计算的是 x === y，则过程如下

          1. 如果 Type(x) 和 Type(y) 不同，返回 false
          2. 如果 Type(x) 为 Undefined，返回 true
          3. 如果 Type(x) 为 Null，返回 true
          4. 如果 Type(x) 为 Number，则进入下面的判断逻辑
          4.1. 如果 x 为 NaN，返回 false
          4.2. 如果 y 为 NaN，返回 false
          4.3. 如果 x 的数字值和 y 相等，返回 true
          4.4. 如果 x 是 +0 且 y 是 -0，返回 true
          4.5. 如果 x 是 -0 且 y 是 +0，返回 ture
          5. 如果 Type(x) 为 String，则当且仅当 x 与 y 的字符序列完全相同（长度相等，每个位置上的字符相同）时返回 true，否则返回 false
          6. 如果 Type(x) 为 Boolean，则若 x 与 y 同为 true 或同为 false 时返回 true，否则返回 false
          7. 如果 x 和 y 引用的是同一个对象，返回 true，否则返回 false

#### 相等运算符 == 的实现

          当明白了 === 的实现之后，接下来我们再来看看 == 的实现，比较一下他们有何不同？
          == 被称为 Equals Operator （注意看没有 Strict 了），假设有表达式 a == b，则它的实际运算过程如下
          1.计算出表达式 a 的结果，并存入 lref 变量
          2.将 GetValue(lref) 的结果存入 lval 变量
          3.计算出表达式 b 的结果，并存入 rref 变量
          4.将 GetValue(rref) 的结果存入 rval 变量
          5.执行 Abstract Equality Comparison 算法判断 rval == lval 并将结果直接返回

          注意，其中的前 4 个步骤是和 === 完全相同的。唯独 5 不同。对于 === 来说，调用的是 Strict Equality Comparison 算法，但是 == 则调用的是 Abstract Equality Comparison 算法。虽然仅一词之差，但是却有质的不同，我们下面就来看看到底它是怎么实现的

          假设要计算的是 x == y，Abstract Equality Comparison 计算的过程如下（很冗长，但是每个步骤都很简单）
          1. 如果 Type(x) 和 Type(y) 相同，则
          1.1. 如果 Type(x) 为 Undefined，返回 true
          1.2. 如果 Type(x) 为 Null，返回 true
          1.3. 如果 Type(x) 为 Number，则
          1.3.1. 如果 x 是 NaN，返回 false
          1.3.2. 如果 y 是 NaN，返回 false
          1.3.3. 如果 x 的数值与 y 相同，返回 true
          1.3.4. 如果 x 是 +0 且 y 是 -0，返回 true
          1.3.5. 如果 x 是 -0 且 y 是 +0，返回 true
          1.3.6. 返回 false
          1.4. 如果 Type(x) 为 String，则当且仅当 x 与 y 的字符序列完全相同（长度相等，每个位置上的字符相同）时返回 true，否则返回 false
          1.5. 如果 Type(x) 为 Boolean，则若 x 与 y 同为 true 或同为 false 时返回 true，否则返回 false
          1.6. 如果 x 和 y 引用的是同一个对象，返回 true，否则返回 false
          2. 如果 x 是 null 且 y 是 undefined，返回 true
          3. 如果 x 是 undefined 且 y 是 null，返回 ture
          4. 如果 Type(x) 为 Number 且 Type(y) 为 String，以 x == ToNumber(y) 的比较结果作为返回
          5. 如果 Type(x) 为 String 且 Type(y) 为 Number，以 ToNumber(x) == y 的比较结果作为返回值
          6. 如果 Type(x) 为 Boolean，以 ToNumber(x) == y 的比较结果作为返回值
          7. 如果 Type(y) 为 Boolean，以 x == ToNumber(y) 的比较结果作为返回值
          8. 如果 Type(x) 为 String 或 Number 且 Type(y) 为 Object，以 x == ToPrimitive(y) 的比较结果作为返回值
          9. 如果 Type(x) 为 Object 且 Type(y) 为 String 或 Number，以 ToPrimitive(x) == y 的比较结果作为返回值
          10. 返回 false


