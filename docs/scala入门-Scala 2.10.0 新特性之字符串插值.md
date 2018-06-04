### s 字符串插值器
在任何字符串前加上s，就可以直接在串中使用变量了。
``` bash
val name="James"
println(s"Hello,$name")//Hello,James 
```
### f 插值器
在任何字符串字面前加上 f，就可以生成简单的格式化串，功能相似于其他语言中的 printf 函数。当使用 f 插值器的时候，所有的变量引用都应当后跟一个printf-style格式的字符串，如%d。
``` bash
val height=1.9d
val name="James"
println(f"$name%s is $height%2.2f meters tall")//James is 1.90 meters tall f 
```
raw 插值器
除了对字面值中的字符不做编码外，raw 插值器与 s 插值器在功能上是相同的。
``` bash
raw"a\nb"
res1:String=a\nb 当不想输入\n被转换为回车的时候，raw 插值器是非常实用的。
```