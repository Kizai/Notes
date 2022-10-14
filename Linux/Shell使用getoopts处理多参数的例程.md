```bash
#!/bin/bash
# 自定义options参数shell脚本代码例程
# funtion函数
demo1() {
    echo "demo1"
}
demo2() {
    echo "demo2"
}
demo3() {
    echo "demo3"
}
demo4() {
    echo "demo4"
}
demo5() {
    echo "demo5"
}
demo6() {
    echo "demo6"
}
demo7() {
    echo "demo7"
}
demo8() {
    echo "demo8"
}
demo9() {
    echo "demo9"
}

demo10() {
    echo "demo10"
}

# usage function 参数对应功能解析
usage() {
    echo "Usage:$0 [ -h | -? ] [ -a num ] [ -b letter ] [ -c string ]" 1>&2
    echo "NUM:1~3"
    echo "letter:A~D"
    echo "string:haha oop sos"
    exit 0
}

while getopts ':ha:b:c:' options; do
    case "$options" in
    h)
        usage
        ;;
    a)
        val=${OPTARG}
        case "$val" in
        1)
            demo1
            ;;
        2)
            demo2
            ;;
        3)
            demo3
            ;;
        *)
            echo "Unknown this command $val!!";
            usage
            ;;
        esac
        ;;
    b)
        val=${OPTARG}
        case "$val" in
        A)
            demo4
            ;;
        B)
            demo5
            ;;
        C)
            demo6
            ;;
        D)
            demo7
            ;;
        *)
            echo "Unknown this command $val!!";
            usage
            ;;
        esac
        ;;
    c)
        val=${OPTARG}
        case "$val" in
        haha)
            demo8
            ;;
        oop)
            demo9
            ;;
        sos)
            demo10
            ;;
        *)
            echo "Unknown this command $val!!";
            usage
            ;;
        esac
        ;;
    ?)  
        echo "Error option!!! -${OPTARG} requires an argument."
        usage
        ;;
    esac
done
shift $(($OPTIND - 1));

```
