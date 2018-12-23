## java中foreach循环

```
public static void main(String[] args) {
    int[] arr = new int[]{8,2,1,0,3};
    int[] index = new int[]{2,0,3,2,4,0,1,3,2,3,3};
    String tel = "";

    for(int i : index) {
        tel += arr[i];
    }
    System.out.println("联系方式：" + tel);
}
```

上边代码相当于下边的写法：

```
for(int i=0; i<index.length; i++){
    i = index[i];
    tel += arr[i];
}
```
