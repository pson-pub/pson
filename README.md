#pson

pson是一种简化版的json，并结合了prototxt的特点，但与json不兼容。

相比json主要有如下特色：
- 支持单行注释    
    以#开头的一行为注释
- 键值(key/value)对中，键(key)无须双引号包裹   
    键(key)不再是字符串，不需要双引号包裹，由此键(key)的命名要求不能包含#'",<>{}[]-\/| 等符号；推荐使用c/java 变量命名规则（一般情况以字母或下划线开头，包含数字，字母，下划线）。   
    以$开头的key是一种特殊情况，用在在模板定义中。
- 成员间可用逗号或空白符分割    
    pson的对象或数组的成员间使用逗号或空白符分割，可以只使用逗号，或只是空白符分割。空白符包括空格\t,\r,\n。
    对象数组中，逗号或空白符也可以都省略掉: [{}{}]。
- 字符串类型，可以使用成对的单引号或双引号包裹      
    如 "value", 'value' 均为字符串值。 
- 键值对中，如果值为对象或数组，那么中间的冒号可省略    
    如 obj{} ,等价于obj:{}
- 常量值    
    常量值为不需要引号包括的字符串，可作为值使用
    + 布尔值常量：除了true和false表示真假外，增加了on和off。on等价于true，off等价于false   
    + 空值常量：null   
    + 类型常量：表示值的数据类型，有：Object, String, Array, Number, Bool, Raw。  类型常量用于模板定义中。   
- 对象模板   
    可以使用模板来定义一个对象结构。对象模板是一种特殊的键值对。键(key)以$开头，值为对象结构；对象值中的成员成员是有序的，成员值可以使用类型常量来表示，也可以使用相应类型的一个值来表示（通过值来推断类型）。
    在值为对象或数组的键值对中使用对象模板，此时键(key)名字后面跟随<对象模板名称>来指定使用的模板，对象的成员的key可以省略掉。
    在包含大量重复类型的数据中，使用模板，可以有效的节省数据的大小。
    如下定义了名为Person的对象模板，后续的person对象，和persons数组使用该模板，它们的对象值中成员均省略了键(key).
```
{
    someone{
        name:'some one'
        id:100
        student:on # or use true
        languages:['Chinese' 'English']
    }

   # 模板
    $Person:{
        name:String
        id:Number
        languages:[String]
    }
    person<Person>{"Jesse" 38 ['english']}
    persons<Person>[
        {'Ada' 1 ['english']}
        {'Basic' 2 ['english','chinese']}
        {'Code' 3 ['english','chinese']}
        {'Dart' 4 ['english','chinese']}
        {'Eve' 5 ['english','chinese']}
        ]
}        
```

    ```pson
# pson 是吸收了 prototxt 和json有点的一种数据结构描述语言
{
# pson支持注释，注释是以#开头的一行文本
# pson 中的键名(key)不需要双引号包裹
  name:'pson'
}
    ```
    