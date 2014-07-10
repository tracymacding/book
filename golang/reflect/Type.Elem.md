# Elem() Type

参数列表

- 无

返回值:

- Type reflect.Type 返回一个直接指向内存地址的 reflect.Type 类型

功能说明:

- reflect.TypeOf(interface{}).Elem() 返回一个类型的元素类型。如果变量类型不是Array,Chan,Map,Ptr或Slice,则panic

代码实例:
    
    package main
    
    import (
        "fmt"
        "reflect"
    )
    
    func main() {

        var a int
        var typeA reflect.Type = reflect.TypeOf(&a)
        fmt.Println(typeA, typeA.Kind()) // 这里是指针，间接指向内存地址
        
        var b int
        var typeB reflect.Type = reflect.TypeOf(&b)
        fmt.Println(typeB.Elem(), typeB.Elem().Kind()) // 加上.Elem() 就可以直接指向内存地址。Elem()很有用，很多时候都用的到它。
        
        var c map[int]string
        var typeC reflect.Type = reflect.TypeOf(c)
        fmt.Println(typeC.Elem(), typeC.Elem().Kind())
        
        var d map[int]string
        var typeD reflect.Type = reflect.TypeOf(&d)
        fmt.Println(typeD.Elem(), typeD.Elem().Elem(), typeD.Elem().Kind())
    }

输出结果

    *int ptr

    int int

    string string

    map[int]string string map

补充

- 看起来Type.Elem()和Value.Elem()功能差不多;
- Type.Elem()和Value.Elem()的区别在于后者只支持ptr类型,如果有个需求:需要获取slice中每个元素的类型,后者就无能为力了
- 关于两者差别可参考下面的例子

Type.Elem()与Value.Elem()差别

代码实例

    package main
    
    import (
        "fmt"
        "reflect"
    )

    func main(){

        var a []*int
        var value reflect.Value = reflect.ValueOf(&a)
        fmt.Println(value.Elem().Kind())
        fmt.Println(value.Elem().Type().Kind())
        fmt.Println(value.Elem().Type().Elem().Kind())
    }

输出结果:

    slice
    slice
    prt

分析

- 示例中Value.Elem().Kind()只能获取类型slice,它没法获取slice中每项的数据类型
- 对Value.Elem.Type.Kind()获取到的类型与直接调用Value.Elem.Kind()一致,证明了Value和Type其实是有相同功能的
- 要获取slice中数据项的数据类型只能通过Value.Elem().Type().Elem().Kind()这种方法,直接通过Value.Elem().Kind()不行
