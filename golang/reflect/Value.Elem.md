# func (v Value) Elem() Value

参数列表

- 无

返回值:

- Value Value.Elem()返回一个指向原始Value存储的内存地址的新reflect.Value

功能说明:

- reflect.ValueOf(interface{}).Elem() 将指针地址指向内存地址，如果原始值类型不是指针,抛出panic

代码实例:
    
    package main
    import (
        "fmt"
        "reflect"
    )
    
    func main(){
        var a int
        var value reflect.Value = reflect.ValueOf(&a)
        fmt.Println(value.Kind(), value.Elem().Kind())
    }

输出结果
        
    ptr int

补充

- 对原始的Value调用Elem()可理解为是对原始指针进行解引用操作
