## 一、Dos命令
- Dos： Disk Operating System 磁盘操作系统
- 常用的 dos 命令    
 
    | 命令        | 作用            |
    |:---------:|:-------------:|
    | dir       | 查看目录内容        |
    | cd        | 切换到其他盘下       |
    | tree      | 查看指定目录下所有的子目录 |
    | cls/clear | 清屏            |
    | exit      | 退出            |
    | md        | 创建目录          |
    | rd        | 删除目录          |
    | copy      | 拷贝文件          |
    | del       | 删除文件          |
    | echo      | 输入内容到文件     |
## 二、变量
![Java数据类型](media/16629433472925/16629467222452.png)
### 基本数据类型
- 基本数据类型转换
    - 自动类型转换
        - `精度小的类型` 自动转换 `精度大的数据类型` => 自动类型转换
    - 数据类型按精度(容器)大小排序
        - `char`、 `int`、 `long`、 `float`、`double`
        - `byte`、 `short`、 `int`、 `long`、 `float`、 `double`
- 基本数据类型转换注意和细节
    1. 有多种类型的数据混合时,系统首先自动将所有数据转换成容器大的那种数据类型,再进行计算
    2. 精度大的赋值给小的,会报错
    3. (byte,short) 和 char之间不会相互转换,但这三者可以计算,计算时首先转换为int类型
    4. 自动提升原则: 表达式结果的类型自动提升为操作数中最大的类型
- 强制类型转换
    - 数据大 => 小 ,就要用强转 
#### 1. 整数类型
- 整数常用 `int` ,除非要表示大数,才用 `long`
- `bit`: 计算机最小存储单位. `byte`: 计算机基本存储单位, `1byte = 8bit`
    ```java
    public class IntDetail {
       public static void main(String[] args) {
         // Java的整数常量(具体值)默认为int,声明long型常量需后加l或L
         int n1 = 1; // int[4], 1byte = 8bit => int = 32 bit
         // int n2 = 1L; x
         long n3 = 1L; // √
       }
    }
    ```
#### 2. 浮点类型
- Java 的浮点类型可以表示一个小数
    - 浮点数在机器中存放形式的简单说明,浮点数=符号位+指数位+尾数位
    - 尾数部分可能丢失，造成精度损失(小数都是近似值)。
- 浮点型常量有`两种表示形式`
    - 十进制数形式: `如: 5.12`
    - 科学计数法形式: `如：5.12e2 [5.12 * 10 的 2 次方 ],  5.12E-2 [5.12/10的2次方]`
    ```Java
    public class FloatDetail {
      public static void main(String[] args) {
        // Java的浮点数默认为double,声明float须后加f或F
        float num1 = 1.1F; // √
        // float num2 = 1.1; x
        double num3 = 1.1; // √
        double num4 = 1.1f; // √
        
        // 科学计数法形式:如：5.12e2 [5.12 * 10 的 2 次方 ]  5.12E-2 [5.12/10的2次方]
        System.out.println(5.12e2);//512.0
        System.out.println(5.12E-2);//0.0512
        
        // 通常情况下，应该使用 double 型，因为它比 float 型更精确。
        // [举例说明]double num9 = 2.1234567851;float num10 = 2.1234567851F;
        double num9 = 2.1234567851;
        float num10 = 2.1234567851F;
        System.out.println(num9);
        System.out.println(num10);
        
        // 浮点数使用陷阱: 2.7 和 8.1 / 3 比较
        double num11 = 2.7;
        double num12 = 8.1 / 3; //2.7
        System.out.println(num11);//2.7
        System.out.println(num12);//接近 2.7 的一个小数，而不是 2.7
        // 得到一个重要的使用点: 当我们对运算结果是小数的进行相等判断是，要小心
        // 应该是以两个数的差值的绝对值，在某个精度范围类判断
        if (num11 == num12) {
            System.out.println("num11 == num12 相等");
        }
        // 正确写法
        if (Math.abs(num11 - num12) < 0.000001) {
            System.out.println("差值非常小，到我的规定精度，认为相等...");
        }
    
        // 可以通过 java API 来看 下一个视频介绍如何使用 API
         System.out.println(Math.abs(num11 - num12));
        //细节:如果是直接查询得的的小数或者直接赋值，是可以判断相等
      }
    }
    ```
#### 3. 字符类型
- 字符类型可以表示单个字符,字符类型是 char，char 是两个字节(可以存放汉字)，多个字符我们用字符串 String
- 字符常量是`' '`括起的单字符,还允许使用转义字符
- 在java中,char的本质是一个整数,在输出时,会按照对应的unicode字符输出`char c4 = 97 => a`
    ```Java
    public class charDetail {
      public static void main(String[] args) {
        // char本质就是数字,在默认输出时,是unicode码对应的字符,要输出对应数字可以使用(int)
        char c1 = 97;
        System.out.println(c1); // a
        char c2 = 'a'; 
        System.out.println((int)c2); //输出'a' 对应的 数字,97
        
        //char 类型是可以进行运算的，相当于一个整数，因为它都对应有 Unicode 码.
        System.out.println('a' + 10);//107
      }
    }
    ```
#### 4. 布尔类型
- boolean类型只允许取值true/false,占1个字符,适于逻辑运算
### 字符编码表

| 字符编码表   | 解析                                      |
|:-------:|:---------------------------------------:|
| ASCII   | 一个字节表示,一共128个字符,实际上字符可以表示256个字符,只用128个  |
| Unicode | 固定大小的编码 使用两个字符来表示字符,字母和汉字统一都占用2个字符,浪费空间 |
| utf-8   | 大小可变的编码,字母使用一个字节,汉字使用3个字节               |
| gbk     | 可以表示汉字,范围广,字母1个字节,汉字2个字节                |
| gb2312  | 可以表示汉字,gb2312 < gbk                     |
| Big5    | 繁体中文                                     |
## 三、程序控制
### 1. 打印直角三角
```Java
public static void main(String args[])
{
  // 控制层数
  for(int i = 1; i <= 5; i++) {
    // 打印*,n层n个*
    for(int j = 1; j <= i; j++ ) {
      System.out.print("*");
    }
    // 换行
    System.out.println("");
  }
}
```
### 2. 打印等腰三角
```text
    *       第一层有1个*, 每层都差2, 2n-1
   ***      第二层有3个*, 
  *****     第三层有5个*,
 *******    第四层有7个*,
*********   第五层有9个*,
```
```Java
public static void main(String args[])
{
    // 控制层数
     for(int i = 1; i <= 5;i++) {
        for(int k = 1; k <= 5-i; k++) {
          System.out.print(" ");
        }
        for(int j = 1; j<= 2 * i - 1;j++) {
            System.out.print("*");
        }
        System.out.println("");
    }
}
```
### 3. 打印镂空等腰三角
```text
    *        第一层1个*,起始和终止各一个,第一层起始位置是1,终止位置也是1  
   * *       第二层2个*, 
  *   *      第三层2个*,
 *     *     第四层2个*,
*********    第五层全部*,
```
```Java
 public static void main(String args[]) {
    for (int i = 1; i <= 5; i++) {
        for (int k = 1; k <= 5-i; k++) {
          System.out.print(" ");
        }
        for (int j = 1; j <= 2 * i - 1; j++) {
            if (j == 1 || j == 2 * i - 1 || i == 5) {
                System.out.print("*");
            } else {
                System.out.print(" ");
            }
        }
        System.out.println("");
    }
}
```
### 4. 打印镂空棱形
```text
        * 
      *   * 
    *       * 
  *           * 
* * * * * * * * * 
  *           * 
    *       * 
      *   * 
        * 
```
```Java
for (int i = 1; i <= 5; i++) {
  for (int j = 1; j <= 5 - i; j++) {
    System.out.print("  ");
  }
  for (int j = 1; j <= 2 * i - 1; j++) {
    if (j == 1 || j == 2 * i - 1 || i == 5) {
      System.out.print("* ");
    } else {
      System.out.print("  ");
    }
  }
  System.out.println();
}
for (int i = 5 - 1; i >= 1; i--) {
  for (int j = 5 - i; j >= 1; j--) {
    System.out.print("  ");
  }
  for (int j = 2 * i - 1; j >= 1; j--) {
    if (j == 1 || j == 2 * i - 1 || i == 5) {
      System.out.print("* ");
    }else {
      System.out.print("  ");
    }
  }
  System.out.println();
}
```
## 四、数组
数组的下标是从 0 开始的,下标必须在指定范围内使用，否则报：下标越界异常
### 1. 数组赋值机制
1. 基本数据类型赋值，这个值就是具体的数据，而且相互不影响。
2. 数组在默认情况下是引用传递，赋的值是地址。
    ![](media/16629433472925/16629646771265.jpg)
### 2. 数组拷贝
```java
public class ArrayCopy {
  public static void main(String[] args) {
    int[] arr1 = {10,20,30};
    int[] arr2 = new int[arr1.length]; // 开辟空间
    for(int i = 0; i < arr1.lenght; i++) {
      arr2[i] = arr1[i];
    }
    arr2[0] = 100;
    //输出 arr1 
    System.out.println("====arr1 的元素===="); 
    for(int i = 0; i < arr1.length; i++) {
       System.out.println(arr1[i]);// 10,20,30
    }
    //输出 arr2 
    System.out.println("====arr2 的元素===="); 
    for(int i = 0; i < arr2.length; i++) {
       System.out.println(arr2[i]); // 100,20,30
    }
  }
}
```
### 3. 数组反转
```java
public class ArrayReverse {
  public static void main(String[] args) {
    int[] arr = {11,22,33,44,55,66};
    int temp = 0;
    int len = arr.length;
    for (int i = 0; i < len / 2; i++) {
        temp = arr[len-1-i];
        arr[len-1-i] = arr[i];
        arr[i] = temp;
    }
    System.out.println("===翻转后数组==="); 
    for(int i = 0; i < arr.length; i++) { 
      System.out.print(arr[i] + "\t");//66,55,44,33,22,11
    }
  }
}
```
- 使用逆序赋值方式
```java
public class ArrayReverse {
  public static void main(String[] args) {
    int[] arr = {11,22,33,44,55,66};
    int[] arr2 = new int[arr.length];
    // 逆序赋值
    for(int i = arr.length - 1, j = 0; i > 0; i--,j++) {
      arr2[j] = arr[i];
    }
    // 让 arr 指向 arr2 数据空间,此时 arr 原来的数据空间就没有变量引用
    // 会被当做垃圾，销毁
    arr = arr2;
    System.out.println("====arr 的元素情况=====");
    for(int i = 0; i < arr.length; i++) {
      System.out.print(arr[i] + "\t");
    }
  }
}
```
### 4. 数组扩容
```java
public class ArrayAdd {
  public static void Main(String[] args) {
    int[] arr = {1,2,3};
    int[] arrNew = new int[arr.length];
    for(int i = 0; i < arr.length; i++) {
      arrNew[i] = arr[i];
    }
    arrNew[arrNew.length - 1] = 4;
    arr = arrNew;
    for(int i = 0; i < arr.length; i++) {
     System.out.print(arr[i]);
    }
  }
}
```
```java
public static void main(String args[]) {
    Scanner myScanner = new Scanner(System.in);
    int[] arr = {1, 2, 3};
    do {
        int[] arrNew = new int[arr.length + 1];
        for (int i = 0; i < arr.length; i++) {
            arrNew[i] = arr[i];
        }
        System.out.println("请输入你要添加的元素");
        int addNum = myScanner.nextInt();
        arrNew[arrNew.length - 1] = addNum;
        arr = arrNew;
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + "\t");
        }
        //问用户是否继续
        System.out.println("是否继续添加 y/n");
        char key = myScanner.next().charAt(0);
        if (key == 'n') {
            //如果输入 n ,就结束
            break;
        }
    } while (true);
}
```
* 数组定位扩容
```java
public static void main(String[] args) {
    int[] arr = {10, 20, 31, 55};
    int insertNum = 23;
    // 定位
    int index = -1;
    for (int i = 0; i < arr.length; i++) {
      if (insertNum <= arr[i]) {
        index = i;
        break;
      }
    }
    if (index == -1) {
      index = arr.length;
    }
    // 扩容
    int[] arrNew = new int[arr.length + 1];
    for (int i = 0, j = 0; i < arrNew.length; i++) {
      if (i != index) { // 说明可以把arr的元素拷贝到arrNew
        arrNew[i] = arr[j];
        j++;
      } else { // 这个i的位置就是要插入的位置
        arrNew[i] = insertNum;
      }
    }
    arr = arrNew;
    for (int i = 0; i < arr.length; i++) {
      System.out.print(arr[i] + "\t");
    }
}
```
![未命名](media/16629433472925/16630557191872.png)
### 5. 数组缩减
```java
int[] arr = {1, 2, 3};
int[] arr1 = new int[arr.length - 1];
for(int i = 0; i < arr1.length; i++) {
  arr1[i] = arr[i];
}
arr = arr1;
for(int i = 0; i < arr.length; i++){
    System.out.print(arr[i]);
}
```
### 6. 二维数组
1. 从定义形式: `类型[][] Array = new 类型[大小][大小]` 
    1. 第一个`[大小]`是二维数组的个数,第二个是二维数组里的一维数组里的个数
    2. 还可以`先声明,后开辟空间`
    ```java
    int arr[][]; //声明二维数组 
    arr = new int[2][3];//再开空间
    ```
2. 可以这样理解，原来的一维数组的每个元素是一维数组, 就构成二维数组
```text
请用二维数组输出:
  0 0 0 0 0 0
  0 0 1 0 0 0
  0 2 0 3 0 0
  0 0 0 0 0 0
```
```java
public class TwoDimensionalArray {
  public static void main(String[] args) {
    int[][] arr = {{0, 0, 0, 0, 0, 0}, {0, 0, 1, 0, 0, 0},
        {0, 2, 0, 3, 0, 0}, {0, 0, 0, 0, 0, 0}};
    // 输出二维图像
    for (int i = 0; i < arr.length; i++) {
      // 遍历二维数组的每一项
      for (int j = 0; j < arr[i].length; j++) {
        System.out.print(arr[i][j] + " ");
      }
      System.out.println();
    }
  }
}
```
3. 二维数组内存形式
![TwodimensionalArray](media/16629433472925/TwodimensionalArray.png)

4. 二维数组动态初始化

| i\\j  | j = 0 | j = 1 | j = 2 |
|:-----:|:-----:|:-----:|:-----:|
| i = 0 | 1     |       |       |
| i = 1 | 2     | 2     |       |
| i = 2 | 3     | 3     | 3     |

```java
public class TwoDimensionalArray {
  public static void main(String[] args) {
    int[][] arr = new int[3][]; // 创建二维数组，但只确定个数(先声明)
    // 二维数组遍历出一维数组
    for (int i = 0; i < arr.length; i++) {
      arr[i] = new int[i + 1]; // （后开辟空间）  
      // 遍历一维数组并赋值
      for (int j = 0; j < arr[i].length; j++) {
        arr[i][j] = i + 1;
      }
    }
    // 遍历
    for (int i = 0; i < arr.length; i++) {
      for (int j = 0; j < arr[i].length; j++) {
        System.out.print(arr[i][j] + " ");
      }
      System.out.println();
    }
  }
}
```
5. 注意点
    1. 数组的声明方式有:
        - 一维: `int[] x 或者 int x[]`
        - 二维: `int[][] y 或者 int[] y[]或者 int y[][]`
    2. 二维数组实际上是由多个一维数组组成的，它的各个一维数组的长度可以相同，也可以不相同

6. 练习
    1. `int arr[][]={{4,6},{1,4,5,7},{-2}};` 遍历该二维数组，并得到和
        ```Java
        public static void main(String[] args) {
            int arr[][]={{4,6},{1,4,5,7},{-2}};
            int sum = 0;
            for(int i = 0; i < arr.length; i++) {
              for(int j = 0; j < arr[i].length; j++) {
                sum += arr[i][j];
              }
            }
            System.out.println("Sum= " + sum);
        }
        ```
    2. 杨辉三角
        ![yanghui](media/16629433472925/yanghui.gif)
        规律: `首尾都为1` ,从第三行开始 `不是首尾的数arr[i][j]` 的值为 `前一行相邻两数之和`
    
        ```java
        arr[i][j] = arr[i-1][j] + arr[i-1][j-1];
        arr[2][1] = arr[1][1] + arr[1][0];
        // x = 2 + 1
        ```
        ```java
        public static void main(String[] args) {
          int[][] arr = new int[10][];
          for(int i = 0; i < arr.length; i++) {
            // 给一维数组开辟空间并赋值
            arr[i] = new int[i + 1];
            for(int j = 0; j < arr[i].length; j++) {
              if(j == 0 || j == arr[i].length) {
                arr[i][j] = 1;
              } else {
                arr[i][j] = arr[i - 1][j] + arr[i - 1][j - 1];
              }
            }
            // 输出
            for (int i = 0; i < arr.length; i++) {
              for (int j = 0; j < arr[i].length; j++) {
                System.out.print(arr[i][j] + "\t");
              }
              System.out.println();
            }
          }
        }
        ```
        
    

## 五、排序
### 1. 冒泡排序
```Java
public static void main(String args[]) {
  int[] arr = {24, 69, 80, 57, 13, -1, 30, 200, -110};
  int temp = 0; //用于辅助交换的变量
  for(int i = 0 ; i < arr.length - 1; i++) {
      for(int j = 0;j < arr.length - i - 1; j++) {
          if(arr[j] > arr[j + 1]) {
            temp = arr[j]; 
            arr[j] = arr[j+1];
            arr[j+1] = temp;
          }
      }
      System.out.println("\n==第"+(i+1)+"轮=="); 
      for(int j = 0; j < arr.length; j++) { 
          System.out.print(arr[j] + "\t");
      }
  }
}
```
## 六、查找
### 1.顺序查找 `SeqSearch.java`
```Java
public class SeqSearch {
    public static void main(String[] args) {
        String[] names = {"白眉鹰王", "金毛狮王", "紫衫龙王", "青翼蝠王"};
        Scanner input = new Scanner(System.in);
        System.out.println("请输入名字：");
        String findName = input.next();
        int index = -1;
        // 遍历数组，逐一比较，有则提示
        for (int i = 0; i < names.length; i++) {
            // 比较字符串 equals
            if (findName.equals(names[i])) {
                System.out.println(findName + "找到了！" + "\t" + "下标为" + i);
                index = i;
                break;
            }
        }
        if (index == -1) {
            System.out.println("没有找到" + findName);
        }
    }
}
```
综合练习
```java
public static void main(String[] args) {
    int[] arr = new int[10];
    for (int i = 0; i < arr.length; i++) {
      // 1 - 100 随机数
      arr[i] = (int) (Math.random() * 100) + 1;
    }
    // 正序
    for (int i = 0; i < arr.length; i++) {
      System.out.print(arr[i] + "\t");
    }
    System.out.print("\n倒序：");
    for (int i = arr.length - 1; i >= 0; i--) {
      System.out.print(arr[i] + "\t");
    }
    double sum = 0;
    int max = arr[0];
    int maxIndex = 0;
    for (int i = 0; i < arr.length; i++) {
      sum += arr[i];
      if (max < arr[i]) {
        max = arr[i];
        maxIndex = i;
      }
    }
    System.out.println("\nMax = " + max + "\t" + "MaxIndex = " 
        + maxIndex + "\t" + "平均值 = " + sum / arr.length);
    // 查找数组中是否有8
    int findNum = 8;
    int flag = -1;
    for (int i = 0; i < arr.length; i++) {
      if (findNum == arr[i]) {
        System.out.println(findNum + " 找到了,下标为" + i);
        flag = 1;
        break;
      }
    }
    if (flag == -1) {
      System.out.println(findNum + " 没有找到");
    }
  }
```
### 2.二分查找 [二分法]
## 七、面向对象(基础)
### 1. 对象内存布局
![](media/16629433472925/16631164312342.jpg)
### 2.方法调用机制
![](media/16629433472925/16631165357276.jpg)
### 3.方法使用细节
* 同一类,方法直接调用
* 不同类方法调用,需要通过对象名调用
* 练习
    ```java
    import java.util.Scanner;
    
    public class MethodExercise01 {
      // 编写类 AA ，有一个方法：判断一个数是奇数 odd 还是偶数, 返回 boolean
      public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        AA a = new AA();
        do {
          System.out.print("请输入一个数字：");
          int num = input.nextInt();
          if (a.isOdd(num)) {
            System.out.println("是偶数");
          } else {
            System.out.println("是奇数");
          }
          System.out.print("是否继续？（y/n）");
          char flag = input.next().charAt(0);
          if (flag == 'n') {
            break;
          }
        } while (true);
        // 根据行、列、字符打印 对应行数和列数的字符
        a.printChar(4, 3, '*');
      }
    }
    
    class AA {
      public boolean isOdd(int num) {
        return num % 2 == 0;
      }
    
      public void printChar(int row, int col, char c) {
        for (int i = 1; i <= row; i++) {
          for (int k = 1; k <= row - i; k++) {
            System.out.print(" ");
          }
          for (int j = 1; j <= 2 * i - 1; j++) {
            System.out.print(c);
          }
          System.out.println();
        }
      }
    }
    ```
### 4.方法传参机制
#### 1. 基本类型传参
* 基本数据类型,传递的是值(值拷贝), 形参的任何改变不影响实参
    ```java
    public class MemberTransmit {
      public static void main(String[] args) {
        int a = 10;
        int b = 11;
        AA obj = new AA();
        obj.swap(a, b);
      }
    
    static class AA {
        public void swap(int a, int b) {
          System.out.println("交换前, a: " + a + " b: " + b);
          int tmp = a;
          a = b;
          b = tmp;
          System.out.println("交换后, a: " + a + " b: " + b);
        }
     }
    }
    ```
    ![](media/16629433472925/16631472148467.jpg)

#### 2. 引用类型传参
* 引用类型传递的是地址（传递也是值，但是值是地址），可以通过形参影响实参
    ```java
    public class yyMemberTransmit {
      public static void main(String[] args) {
        B b = new B();
        int[] arr = {1, 2, 3};
        b.test100(arr);
        System.out.println("Main方法的arr数组");
        for (int i = 0; i < arr.length; i++) {
          System.out.print(arr[i] + "\t");
        }
        System.out.println();
        Person p = new Person();
        p.name = "fishx";
        p.age = 20;
        b.test200(p);
        System.out.println("Main方法里修改Obj");
        System.out.println(p.name);
      }
    
      static class Person {
        String name;
        int age;
      }
    
      static class B {
        public void test100(int[] arr) {
          arr[0] = 200; // 修改元素
          // 遍历数组
          System.out.println("test100方法的arr数组");
          for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + "\t");
          }
          System.out.println();
        }
    
        public void test200(Person p) {
          p.name = "小靖";
          System.out.println("test200方法里修改Obj");
          System.out.println(p.name);
        }
      }
    }
    ```
    ```text
    test100方法的arr数组：200	2	3	
    Main方法的arr数组：200	2	3	
    test200方法里修改Obj：小靖
    Main方法里修改Obj：小靖
    ```
    ![引入类型](media/16629433472925/16632392436575.png)

### 5.克隆对象
```java
public class ObjectCopy {
  public static void main(String[] args) {
    Person person = new Person();
    person.name = "Tom";
    person.age = 81;

    Tools tools = new Tools();
    Person p2 = tools.copyPerson(person);

    // person 和 p2都是Person对象，但是是两个独立的对象，属性相同
    System.out.println("person的属性 age = " + person.age +"\tname = "+ person.name);
    System.out.println("p2的属性     age = " + p2.age +"\tname = "+ p2.name);
  }

  static class Person {
    String name;
    int age;
  }

  static class Tools {
    public Person copyPerson(Person p) {
      Person p2 = new Person();
      p2.name = p.name;
      p2.age = p.age;
      return p2;
    }
  }
}
```
### 6.方法递归
* 递归就是方法自己调用自己,每次调用时传入不同的变量.递归有助于编程者解决复杂问题,同时可以让代码变得简洁
#### 1.递归案例
* 递归打印 
    ```java
    public class recursion {
      public static void main(String[] args) {
        T t = new T();
        t.test(4);
      }
    }
    
    class T {
      public void test(int n) {
        if (n > 2) test(n - 1);
        System.out.println("n = " + n);
      }
    }
    ```
    ![](media/16629433472925/16632902159681.jpg)
* 阶乘
    ```Java
    public class recursion {
      public static void main(String[] args) {
        T t = new T();
        int res = t.factorial(3);
        System.out.println("3 的阶乘 res = " + res);
      }
    }
    
    class T {
      public int factorial(int a) {
        if (a == 1) {
          return 1;
        } else {
          return factorial(a - 1) * a;
        }
      }
    }
    ```
    ![](media/16629433472925/16632939774222.jpg)
#### 2.递归规则
1. 执行方法时,就会`创建`一个`新的`、`受保护的`独立空间(`栈`)
2. `基本类型` 的局部变量是`独立的`,而 `引用类型` 的变量(指向地址)就会`共享`该引用类型的数据
3. 递归必须向退出递归的条件逼近,否则就是无限递归,出现`StackOverflowError`
4. 当一个方法执行完毕,或遇到return,就会返回,遵守谁调用,就将结果返回给谁,同时当方法执行完毕或返回时,该方法也就执行完毕
#### 3.递归练习
##### 1.斐波那契数
```test
使用递归求出：斐波那契数（1，1，2，3，5，8，13...）给你一个n，求出它的值
思路分析：
  当n = 1，1
  当n = 2，1
  当n >= 3，前两项之和
```
```Java
public class recursion {
  public static void main(String[] args) {
    T t = new T();
    int res = t.fibonacci(7);
    if (res == -1) {
      System.out.println("请输入大于1的数！");
      return;
    }
    System.out.println("res结果：" + res);
  }
}
    
class T {
  public int fibonacci(int n) {
    if (n >= 1) {
      if (n == 1 || n == 2) {
        return 1;
      } else {
        return fibonacci(n - 1) + fibonacci(n - 2);
      }
    } else {
      return -1;
    }
  }
}
```
##### 2.猴子吃桃问题
```test
猴子吃桃问题：
      一堆桃子，第一天吃了一半再+1，以后每天都吃一半+1，
      当第10天，发现只有一个了，问：最初有多少个
      思路分析（逆推）：
        当 day = 10, 1
        当 day = 9, （day10 + 1）* 2  = 4
        当 day = 8, （day9 + 1）* 2  = 10
        规律(day1-day9)：当天的桃子数 = （后天的桃子 + 1）* 2
```
```Java
public class recursion {
  public static void main(String[] args) {
    T t = new T();
    int day = 1;
    int peachNum = t.peach(day);
    if (peachNum != -1) {
      System.out.println("第 " + day + "天有" + peachNum + "个桃子");
    }
  }
}
    
class T {
  public int peach(int day) {
    if (day == 10) {
      return 1;
    } else if (day >= 1 && day <= 9) {
      return (peach(day + 1) + 1) * 2;//规则，自己要想
    } else {
      System.out.println("day 在 1-10");
      return -1;
    }
  }
}
```
##### 3.老鼠走迷宫
```Java
public class Maze {
  public static void main(String[] args) {
    // 创建迷宫8行7列，最外行为圈起作为城墙，0：ok，1：on
    int[][] map = new int[8][7];
    for (int i = 0; i < 7; i++) {
      map[0][i] = 1;
      map[7][i] = 1;
      map[i][0] = 1;
      map[i][6] = 1;
    }
    map[3][1] = 1;
    map[3][2] = 1;
    for (int i = 0; i < map.length; i++) {
      for (int j = 0; j < map[i].length; j++) {
        System.out.print(map[i][j] + " ");
      }
      System.out.println();
    }
    H h = new H();
    h.findWay(map, 1, 1);
    System.out.println("\n====找路的情况如下=====");
    for (int i = 0; i < map.length; i++) {
      for (int j = 0; j < map[i].length; j++) {
        System.out.print(map[i][j] + " ");
      }
      System.out.println();
    }
  }
}
    
class H {
  /* @turn boolean
   * @description findWay专门找出迷宫路径
   * map数组即（Maze）：0：可以走，1：障碍物，2：可走，3：走过但死路
   * map[6][5] = 2表示找到出口，终止，否则继续
   * 策略：下 => 右 => 上 => 左
   * */
  public boolean findWay(int[][] map, int i, int j) {
    if (map[6][5] == 2) { // 找到出口了
      return true;
    } else {
      if (map[i][j] == 0) { // 可以走
        map[i][j] = 2;  // 假定能通,使用策略:下 => 右 => 上 => 左
        if (findWay(map, i + 1, j)) { // 尝试往下走
          return true;
        } else if (findWay(map, i, j + 1)) { //尝试往右走
          return true;
        } else if (findWay(map, i - 1, j)) { //尝试往上走
          return true;
        } else if (findWay(map, i, j - 1)) { //尝试往左走
          return true;
        } else {
          map[i][j] = 3;
          return false;
        }
      } else {
        return false;
      }
    }
  }
}
```
##### 4.汉诺塔
```Java
public class HanoiTower {
  public static void main(String[] args) {
    Tower tower = new Tower();
    tower.move(3, 'A', 'B', 'C');
  }
}
    
class Tower {
  // 汉诺塔：A塔移动到B塔或C塔，大的必须在下
  public void move(int num, char a, char b, char c) {
    // 如果只有一个盘
    if (num == 1) {
      System.out.println(a + " ==> " + c);
    } else {
      // 多盘看作两个盘：最下的和上面所有盘（num - 1）
      //（1）先移动上面所有盘到B塔，借助C塔
      move(num - 1, a, c, b);
      //（2）把最下面的这个盘，移动到 c
      System.out.println(a + " ==> " + c);
      //（3）再把 b 塔的所有盘，移动到 c ,借助 a
      move(num - 1, b, a, c);
    }
  }
}
```
### 7. 方法重载
java 中允许`同一个类`中，`多个同名方法`的存在，但要求 `形参` 列表不一致
#### 1. 构成方法重载的要求:
* 构成方法重载的要求:
    * 方法名: `必须相同`
    * 形参列表: `必须不同` `=>` `[1.类型不同, 2.个数不同, 3.顺序不同] `                              
    * 返回类型: `没有要求`
* 方法重载练习
    ```java
    public class OverLoad {
      public static void main(String[] args) {
        Methods m = new Methods();
        System.out.println(m.max(13.3, 13.1));
      }
    }
    
    class Methods {
      /*
        定义三个重载方法 max()，
        第一个方法，返回两个 int 值中的最大值，
        第二个方法，返回两个 double 值中的最大值，
        第三个方法，返回三个 double 值中的最大值，并分别调用三个方法
      */
      public int max(int a, int b) {
        return Math.max(a, b);
      }
    
      public double max(double a, double b) {
        return Math.max(a, b);
      }
    
      public double max(double a, double b, double c) {
        double num = Math.max(a, b);
        return Math.max(num, c);
      }
    }
    ``` 
#### 2. 可变参数
* java 允许将`同一个类`中`多个同名同功能`但`参数个数不同`的方法，封装成一个方法
    ```java
    public class VarParameter {
      public static void main(String[] args) {
        HspMethod hsp = new HspMethod();
        System.out.println(hsp.sum(11, 21, 42, 84,2));
      }
    }

    class HspMethod {
      /*    
        1. int... 表示接受的是可变参数，类型是 int ,即可以接收多个 int(0-多) 
        2. 使用可变参数时，可以当做数组来使用 即 nums 可以当做数组 
        3. 遍历 nums 求和即可
      */
      public int sum(int... nums) {
        int res = 0;
        for (int i = 0; i < nums.length; i++) {
          res += nums[i];
        }
        return res;
      }
    }
    ```
* 可变参数注意点
    * 可变参数的实参可以为0个或任意多个
    * 可变参数的实参可以为数组
    * 可变参数的本质就是数组
    * 可变参数可以和普通类型的参数一起放在形参列表,但必须保证可变参数在最后
    * 一个形参列表中只能出现一个可变参数
* 练习
    ```Java
    public class VarParameter {
      public static void main(String[] args) {
        HspMethod hsp = new HspMethod();
        System.out.println(hsp.sum("fishx", 91, 75,88,66));
      }
    }
    
    class HspMethod {
      /*
        有三个方法，
        分别实现返回姓名和两门课成绩(总分)
        返回姓名和三门课成绩(总分)
        返回姓名和五门课成绩（总分）
        ==> 封装成一个可变参数的方法
      */
      public String sum(String name, double... scores) {
        double totalScore = 0;
        for (int i = 0; i < scores.length; i++) {
          totalScore += scores[i];
        }
        return name + " 成绩为：" + totalScore;
      }
    }
    ```
### 8.作用域
#### 1. 全局变量
* 全局变量：也就是属性，作用域为整个类体 Cat 类：cry eat 等方法使用属性
    * 全局变量(属性)可以不赋值，直接使用，因为有默认值
#### 2. 局部变量
* 局部变量: 局部变量一般是指在成员方法中定义的变量
    * 局部变量必须赋值后，才能使用，因为没有默认值
    ```Java
    public class VarParameter {
      // 编写一个 main 方法
      public static void main(String[] args) { }
    }
    
    class Cat {
      // 全局变量：也就是属性，作用域为整个类体 Cat 类：cry eat 等方法使用属性
      // 属性在定义时，可以直接赋值
      int age = 10; //指定的值是 10
    
      // 全局变量(属性)可以不赋值，直接使用，因为有默认值，
      double weight; //默认值是 0.0
    
      public void hi() {
        //局部变量必须赋值后，才能使用，因为没有默认值
        int num = 1;
        String address = "北京的猫";
        System.out.println("num=" + num);
        System.out.println("address=" + address);
        System.out.println("weight=" + weight);//属性
      }
    
      public void cry() {
        // 1. 局部变量一般是指在成员方法中定义的变量
        // 2. n 和 name 就是局部变量
        // 3. n 和 name 的作用域在 cry 方法中
        int n = 10;
        String name = "jack";
        System.out.println("在 cry 中使用属性 age=" + age);
      }
    }
    ```
#### 3. 作用域注意点
1. 属性和局部变量可以重复，访问时遵循就近原则
2. 在同一个作用域中,比如在同一个成员方法中,两个局部变量,不能同名
3. `属性`生命周期较长,伴随着对象`创建/销毁`而`创建/销毁`,`局部变量`生命周期较短,伴随`代码块的结束而销毁`
4. 作用域范围不同
    1. 全局变量/属性: 可以被本类使用,或其他类使用(通过对象调用)
    2. 局部变量: 只能在本类中对应的方法中使用
5. 修饰符不同
    1. 全局变量/属性可以加修饰符
    2. 局部变量不可以加修饰符
    ```Java
    public class VarScopeDetail {
      public static void main(String[] args) {
        Person p = new Person();
        /*
          属性生命周期较长(伴随着对象创建而创建)
          局部变量生命周期较短(随着代码块的结束而销毁)
        */
        p.say(); // 当执行 say 方法时，say 方法的局部变量比如 name,会创建，当 say 执行完毕后
        // name 局部变量就销毁,但是属性(全局变量)仍然可以使用
    
        tool t1 = new tool();
        t1.test(); //第 1 种跨类访问对象属性的方式
        t1.test2(p);//第 2 种跨类访问对象属性的方式
      }
    }
    
    class tool {
      public void test() {
        Person p1 = new Person();
        System.out.println(p1.name);
      }
    
      public void test2(Person p) {
        System.out.println(p.name);
      }
    }
    
    class Person {
      String name = "Jack";
    
      public void say() {
        // 细节：属性和局部变量可以重复，访问时遵循就近原则
        String name = "fishx";
        System.out.println("say() name = " + name);
      }
    }
    ```
### 9.构造方法/构造器
#### 1. 基础
* 构造方法又叫构造器(constructor)，是类的一种特殊的方法，它的主要作用是完成对新`对象的初始化`
    1. 构造器的修饰符可以默认，也可以是 `public、protected、private`
    2. 构造器没有返回值
    3. 方法名 和类名字必须一样
    4. 参数列表 和 成员方法一样的规则
    5. 构造器的调用, 由系统完成
* 特点:  
    1. 方法名和类名相同
    2. 没有返回值
    3. 在创建对象时，系统会自动的调用该类的构造器完成对象的初始化。
    ```Java
    public class Constructor {
      public static void main(String[] args) {
        // 当我们 new 一个对象时，直接通过构造器指定名字和年龄
        Man m = new Man("fishx", 20);
        System.out.println(m.name + "\t" + m.age);
      }
    }
    
    class Man {
      String name;
      int age;
    
      /*构造器:
        1. 构造器没有返回值, 也不能写 void
        2. 构造器的名称和类 Person 一样*/
      public Man(String pName, int pAge) {
        name = pName;
        age = pAge;
      }
    }
    ```
* 使用细节和注意事项
    1. 构造器重载: 一个类可以定义多个不同的构造器
    2. 构造器名和类名要相同,而且没有返回值
    3. 构造器是完成对象的初始化,并不是创建对象
    4. 在创建对象时,系统自动的调用该类的构造方法
    5. 如果没有定义构造器,系统会自动给类生成一个默认无参构造器(默认构造器)
        ```Java
        Dog dog = new Dog(); // 没有定义构造器,使用默认无参构造器
        
        class Dog{
          // 系统默认生成, javap反编译查看
          public Dog(){};
        }
        ```
    6. 一旦定义了自己的构造器,默认的构造器就覆盖了,就不能再使用默认无参构造器,除非显示定义一下
        ```Java
        Dog dog = new Dog("fishx");// 默认无参构造器被自定义覆盖了
        Dog dog = new Dog(); 
        
        class Dog{
          public Dog(String name){};
          // 显示定义
          public Dog(){};
        }
        ```
* 练习
    ```Java
    public class Constructor {
      public static void main(String[] args) {
        Man man1 = new Man();
        System.out.println("Man1的信息：name = " + man1.name + ", age = " + man1.age);
        Man man2 = new Man("fishx",20);
        System.out.println("Man2的信息：name = " + man2.name + ", age = " + man2.age);
      }
    }
    
    class Man {
      String name;
      int age;
      
      // 第一个无参构造器：利用构造器设置所有人的 age 属性初始值都为 18
      public Man() {
        age = 18;
      }
    
      // 第二个带 pName 和 pAge 两个参数的构造器
      public Man(String pName, int pAge) {
        name = pName;
        age = pAge;
      }
    }
    ```
#### 2. 对象创建的流程分析
![](media/16629433472925/16634192950081.jpg)

![](media/16629433472925/16634192355724.jpg)

### 10.this关键字
#### 1.this
* `this` : Java虚拟机会给每个对象分配this,代表对象
    * 简单说, 哪个对象调用, this就代表哪个对象
#### 2.注意事项和使用细节
1. this 关键字可以用来访问本类的属性、方法、构造器
2. this 用于区分当前类的属性和局部变量
3. 访问成员方法的语法：this.方法名(参数列表); 
4. 访问构造器语法：this(参数列表); 注意只能在构造器中使用(即只能在构造器中访问另外一个构造器, 必须放在第一 条语句)
5. this 不能在类定义的外部使用，只能在类定义的方法中使用。
### 11. 本章练习
* 查找某字符串是否在字符串数组中,并返回索引,否则返回-1
    ```Java
    public class Homework02 {
      public static void main(String[] args) {
        A2 a2 = new A2();
        String[] arr = {"fishx", "11", "17"};
        System.out.println(a2.find(arr, "fishx"));
      }
    }
    
    class A2 {
      public int find(String[] arr, String str) {
        for (int i = 0; i < arr.length; i++) {
          if (arr[i].equals(str)) {
            return i;
          }
        }
        return -1;
      }
    }
    ``` 
* 实现更改某本书的价格
    ```Java
    public class Homework03 {
      public static void main(String[] args) {
        Book book = new Book("神雕侠侣", 182.1);
        book.info();
        book.updatePrice();
        book.info();
      }
    }
    
    class Book {
      String name;
      double price;
    
      public Book(String name, double price) {
        this.name = name;
        this.price = price;
      }
    
      public void updatePrice() {
        if (this.price > 150) {
          this.price = 150;
        } else if (this.price > 100) {
          this.price = 100;
        }
      }
    
      public void info() {
        System.out.println("书名: 《" + name + "》 价格: " + price);
      }
    }
    ```
* 实现数组的复制功能copyArr
    ```Java
    public class Homework04 {
      public static void main(String[] args) {
        A03 a03 = new A03();
        int[] arr = {11, 22, 1, 2,};
        int[] newArr = a03.copyArr(arr);
        for (int j : newArr) {
          System.out.print(j + "\t");
        }
      }
    }
    
    class A03 {
      public int[] copyArr(int[] oldArr) {
        int[] newArr = new int[oldArr.length];
        System.arraycopy(oldArr, 0, newArr, 0, oldArr.length);
        return newArr;
      }
    }
    ```
* 定义圆类Circle(定义属性: R,方法: 1.显示圆周长 2.显示圆面积)
    ```Java
    public class Homework05 {
      public static void main(String[] args) {
        Circle circle = new Circle(4);
        System.out.println("周长：" + circle.getC());
        System.out.println("面积：" + circle.getS());
      }
    }
    
    class Circle {
      int r;
    
      public Circle(int r) {
        this.r = r;
      }
    
      public double getC() {
        return 2 * r * 3.14;
      }
    
      public double getS() {
        return r * 3.14 * 3.14;
      }
    }
    ```
* 定义Cale计算类(定义2个变量表示两个操作数,定义四个方法[+,-,*,/],要求除数为0要提示,创建两个对象)
    ```java
    public class Homework06 {
      public static void main(String[] args) {
        Cale cale = new Cale(3, 0);
        System.out.println("加：" + cale.sum());
        System.out.println("减：" + cale.minus());
        System.out.println("乘：" + cale.mul());
        Double divRes = cale.div();
        if (divRes != null){
          System.out.println("除：" + cale.div());
        }
      }
    }
    
    class Cale {
      double num1;
      double num2;
    
      public Cale(double num1, double num2) {
        this.num1 = num1;
        this.num2 = num2;
      }
    
      public double sum() {
        return num1 + num2;
      }
    
      public double minus() {
        return num1 - num2;
      }
    
      public double mul() {
        return num1 * num2;
      }
    
      public Double div() {
        if (num2 == 0) {
          System.out.println("不能为0");
          return null;
        } else {
          return num1 / num2;
        }
      }
    }
    ```
* 输出结果
    ```Java
    public class Homework08 { // 公共类
      int count = 9; // 属性
    
      public void count1() { // 类的成员方法
        count = 10;
        System.out.println("count1 = " + count);
      }
    
      public void count2() { // 类的成员方法
        System.out.println("count1 = " + count++); // 先赋值,后自增
      }
    
      // main方法，任何一个类，都可有main
      public static void main(String[] args) {
        new Homework08().count1(); // 匿名
        Homework08 h8 = new Homework08(); // 另辟空间
        h8.count2();
        h8.count2();
      }
    }
    ```
* 定义Music类(属性:name、times,方法:play、getInfo)
    ```Java
    public class Homework09 {
      public static void main(String[] args) {
        Music music = new Music("难忘今宵", 24);
        music.play();
        System.out.println(music.getInfo());
      }
    }
    
    class Music {
      String name;
      int times;
    
      public Music(String name, int times) {
        this.name = name;
        this.times = times;
      }
    
      public void play() {
        System.out.println("音乐名《" + name + "》正在播放... 时长：" + times + "秒");
      }
    
      public String getInfo() {
        return "音乐名: " + name + "\t时长：" + times + "s";
      }
    }
    ```
* 试写出以下代码运行结果
    ```Java
    class Demo {
      int i = 100;
    
      public void m() {
        int j = i++;
        System.out.println("i = " + i);
        System.out.println("j = " + j);
      }
    }
    
    public class Homework10 {
      public static void main(String[] args) {
        Demo d1 = new Demo();
        Demo d2 = d1;
        d2.m();
        System.out.println(d1.i);
        System.out.println(d2.i); 
      }
    }
    ```
    ![未命名](media/16629433472925/16635053000220.png)
* 要求复用构造器
    ```Java
    public class Homework11 {
      public static void main(String[] args) {
        Employee employee = new Employee("fishx", "男", 20, "后端工程师", 12);
        Employee employeeInfo = new Employee("fishx", "男", 20);
        Employee employeePosition = new Employee("后端工程师", 12);
      }
    }
    
    class Employee {
      String name;
      String sex;
      int age;
      String position;
      int salary;
    
      public Employee(String position, int salary) {
        this.position = position;
        this.salary = salary;
      }
    
      public Employee(String name, String sex, int age) {
        this.name = name;
        this.sex = sex;
        this.age = age;
      }
    
      public Employee(String name, String sex, int age, String position, int salary) {
        this(name, sex, age);
        this.position = position;
        this.salary = salary;
      }
    }
    ```
* 将对象作为参数传递给方法
    ```text
    题目要求:
     (1) 定义一个Circle类，包含-个double型的radius属性代表圆的半径，findArea()方法返回圆的面积。
     (2) 定义一个类PassObject,在类中定义一个方法printAreas()
     (3) 在printAreas方法中打印输出1到times之间的每个整数半径值，以及对应的面积。
     (4) 在main方法中调用printAreas()方法，调用完毕后输出当前半径值。
    ```
    ```Java
    public class Homework12 {
      public static void main(String[] args) {
        HCircle hCircle = new HCircle();
        PassObject passObject = new PassObject();
        passObject.printAreas(hCircle, 5);
      }
    }
    
    class HCircle {
      double radius;
    
      // 输出面积
      public double findArea() {
        return Math.PI * radius * radius;
      }
    
      // 修改半径
      public void setRadius(double radius) {
        this.radius = radius;
      }
    }
    
    class PassObject {
      public void printAreas(HCircle hCircle, int times) {
        System.out.println("Radius\t\tarea");
        for (int i = 1; i <= times; i++) { // 输出 1～times 的 R 和 S
          hCircle.setRadius(i); // 修改半径
          System.out.println((double) i + "\t\t" + hCircle.findArea());// 输出面积
        }
      }
    }
    ```
## 八、面向对象(中级)
### 1. 包
#### 1. 包的本质分析
* 实际上就是创建不同文件夹/目录来保存类文件
#### 2. 包的作用
* 区分相同名字的类
* 当类很多时,可以很好的管理类
* 控制访问范围
#### 3. 注意事项
```Java
// package 的作用是声明当前类所在的包，需要放在类(或者文件)的最上面，一个类中最多只有一句
package com.fishx; 
//import 指令 位置放在 package 的下面，多句没有顺序要求,⚠️ 按需导入
import java.util.Arrays;

public class ArraySort {
  public static void main(String[] args) {
    int[] arr = {-11, 22, 1, 5, -9};
    Arrays.sort(arr);
    for (int i = 0; i < arr.length; i++) {
      System.out.print(arr[i] + " ");
    }
  }
}
```
#### 4. 命名规则
* 只能包含`数字、字母、下划线、小圆点`,但不能用数字开头,不能是关键字或保留字
### 2. 访问修饰符
#### 1. 基本介绍
* java 提供四种访问控制修饰符号，用于控制方法和属性(成员变量)的访问权限（范围）:
    * 公开级别:用 public 修饰,对外公开
    * 受保护级别:用 protected 修饰,对子类和同一个包中的类公开
    * 默认级别:没有修饰符号,向同一个包的类公开.
    * 私有级别:用 private 修饰,只有类本身可以访问,不对外公开.
#### 2. 访问修饰符的访问范围 
![](media/16629433472925/16635531776167.jpg)
#### 3. 使用的注意事项
1. 修饰符可以用来修饰类中的属性,成员方法以及类
2. 只有`默认`和`public`才能`修饰类`,并遵循上述访问权限的特点
3. 成员方法的访问规则和属性完全一样
### 3. 面向对象编程三大特征 —— 封装
#### 1. 封装介绍
* 封装就是把抽象出的数据[`属性`]和对数据的操作[`方法`]封装在一起
* 数据被保护在内部,程序的其它部分只有通过被授权的操作[`方法`],才能对数据进行操作
#### 2. 封装的好处
1. 隐藏实现细节: 方法 `<=` 调用(传参)
2. 可以对数据进行验证,保证安全合理
#### 3. 封装的实现步骤 (三步)
1. 将属性进行私有化 `private` [不能直接修改属性]
2. 提供一个公共(`public`)的set方法,用于对属性判断并赋值
    ```Java
    public void setXxx(类型 参数名) { // Xxx表示某个属性
      // 数据校验的业务逻辑
      属性 = 参数名;
    }
    ```
3. 提供一个公共(`public`)的get方法,用于获取属性的值
    ```java
    public 数据类型 getXxx() { // 权限判断,Xxx某个属性
      return xx;
    }
    ``` 
#### 4. 封装的快速入门
```text
创建程序,在其中定义两个类：
    Account 和 AccountTest 类体会 Java 的封装性。 
Account 类要求具有属性：
    姓名（长度为 2 位 3 位或 4 位）、余额(必须>20)、密码（必须是六位）, 
    如果不满足，则给出提示信息，并给默认值(程序员自己定) 
通过 setXxx 的方法给 Account 的属性赋值。 
在 AccountTest 中测试
```
```java
package encap;

import java.util.Scanner;

public class Account {
  public String name;
  private double balance;
  private String password;

  public Account() {
  }

  public Account(String name, double balance, String password) {
    setName(name);
    setBalance(balance);
    setPassword(password);
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = (name.length() >= 2 && name.length() <= 4) ? name : "佚名";
    if (this.name.equals("佚名")) {
      System.out.println("姓名（长度为 2 位 3 位或 4 位,默认为佚名");
    }
  }

  public double getBalance() {
    return balance;
  }

  public void setBalance(double balance) {
    if (balance > 20) {
      this.balance = balance;
    } else {
      System.out.println("余额必须大于20,默认为0");
      this.balance = 0;
    }
  }

  public String getPassword() {
    return password;
  }

  public void setPassword(String password) {
    this.password = password.length() == 6 ? password : "0000";
    if (this.password.equals("0000")) {
      System.out.println("密码（必须是六位）,默认为0000");
    }
  }

  public void info() {
    Scanner scanner = new Scanner(System.in);
    System.out.println("请输入管理员密码：");
    if (scanner.next().equals("admin123")) {
      System.out.println("name：" + name + "余额：" + balance + "密码：" + password);
    }else{
      System.out.println("你没有权限查看～");
    }
  }
}
```
```java
package encap;

public class AccountTest {
  public static void main(String[] args) {
    System.out.println("====默认无参构造====");
    Account account = new Account();
    account.setName("jack");
    account.setBalance(60);
    account.setPassword("123456");
    account.info();
    System.out.println("====自定义构造====");
    Account account1 = new Account("fish", 12, "root");
    account1.info();
  }
}
```
### 4. 面向对象编程三大特征 —— 继承
#### 1. 继承介绍
* 继承可以解决代码复用,抽象出父类相同的属性和方法，所有的子类不需要重新定义这些属性和方法
* 只需要通过 extends 来 声明继承父类即可
![](media/16629433472925/16635630408086.jpg)
#### 2. 继承的基本语法
```java
class 子类 extends 父类 {
 // 1. 子类即拥有父类定义的属性和方法
 // 2. 父类又名超类、基类, 子类又叫派生类  
}
```
#### 3. 继承的好处
1. 代码的复用性提高了
2. 代码的扩展性和维护性提高了
#### 4. 继承细节
```java
package extend;

public class Base {
  public int n1 = 100;
  protected int n2 = 200;
  int n3 = 300;
  private int n4 = 400;

  public Base() {
    System.out.println("Base()...");
  }

  public int N4() {
    return n4;
  }

  public void test100() {
    System.out.println("test100");
  }

  public void test200() {
    System.out.println("test200");
  }

  void test300() {
    System.out.println("test300");
  }

  private void test400() {
    System.out.println("test400");
  }

  public void T400() {
    test400();
  }
}
```
1. 子类继承了所有的属性和方法，非私有的属性和方法可以在子类直接访问, 但是`私有属性和方法`不能在`子类直接访问`，要通过父类提供公共的方法去访问
    ```java
    package extend;
    
    public class Sub extends Base {
      public Sub() {
        System.out.println("Sub()...");
      }
    
      public void sayOk() {
        // 可以发现：父类除了private修饰的属性和对象，都可以访问
        // 非私有的属性和方法可以在子类直接访问, 但是私有属性和方法不能在子类直接访问
        System.out.println(n1 + ", " + n2 + ", " + n3);
        // System.out.println(n4); x 具有 private 访问权限
        System.out.println("n4 = " + N4());
        test100();
        test200();
        test300();
        // test400(); x 具有 private 访问权限
        T400();
      }
    }    
    ```
1. 子类必须调用父类的构造器,完成父类的初始化
2. 当创建子类对象时，不管使用子类的哪个构造器，默认情况下`总会`去`调用`父类的`无参构造器`，如果父类没有提供无参构造器,则必须在子类的构造器中用 super 去指定使用父类的哪个构造器完成对父类的初始化工作，否则，编译不会通过
3. 如果希望指定去调用父类的某个构造器，则显式的调用一下 : super(参数列表)
    ```java
    package extend;
    
    public class Base {
      public int n1 = 100;
      protected int n2 = 200;
      int n3 = 300;
      private int n4 = 400;
      
      // 父类没有提供无参构造器
      /* public Base() {
        System.out.println("Base()...");
      } */
      
       public Base(String name) {
          System.out.println("父类 Base(String name)构造器被调用....");
       }
       
       public Base(String name, int age) {
          System.out.println("父类 Base(String name, int age)构造器被调用....");
       }
    }
    ```
    ```java
    package extend;
    
    public class Sub extends Base {
      // 必须在子类的构造器中用 super 去指定使用父类的哪个构造器完成对父类的初始化工作
      public Sub() {
        super("fishx"); // 必须先有父类 => 子类
        // super("fishx",20) 可以指定多个构造器中一个
        System.out.println("子类 Sub()...");
      }
    }
    ```
1. super 在使用时，必须放在构造器第一行(super 只能在构造器中使用)
2. super() 和 this() 都只能放在构造器第一行，因此这两个方法不能共存在一个构造器
3. java 所有类都是 Object 类的子类, Object 是所有类的基类(父类).
4. 父类构造器的调用不限于直接父类！将一直往上追溯直到 Object 类(顶级父类)
    ```java
    package extend;
    
    public class TestExtend {
      public static void main(String[] args) {
        System.out.println("====第1个对象====");
        Sub sub = new Sub();
      }
    }
    ```
    ```Java
    package extend;
    
    public class Sub extends Base {
      public Sub() {
        // 默认调用super(); 往上追溯Base的无参构造
        System.out.println("子类 Sub()...");
      }
    }
    ```
    ```java
    package extend;
    
    public class Base extends TopBase{
      public Base() {
        // 默认调用super(); 往上追溯TopBase的无参构造
        System.out.println("父类 Base()...");
      }
    }
    ```
    ```java
    package extend;
    
    public class TopBase {
      public TopBase() {
        // 隐藏的 super(); 接着往上追溯，但Object基类无输出
        System.out.println("构造器 TopBase() 被调用...");
      }
    }
    ```
9. 子类`最多`只能继承`一个父类`(指直接继承)，即 java 中是单继承机制。
10. 不能滥用继承，子类和父类之间必须满足 is-a 的逻辑关系 `Cat is a Animal` `合理` `Cat extends Animal`
#### 5. 继承本质分析
> 当子类对象创建好后,建立**查找关系**
```java
package extend;

public class ExtendsTheory {
  public static void main(String[] args) {
    Son son = new Son();
    // (1) 首先看子类是否有该属性 
    // (2) 如果子类有这个属性，并且可以访问，则返回信息 
    // (3) 如果子类没有这个属性，就看父类有没有这个属性(如果父类有该属性，并且可以访问，就返回信息..) 
    // (4) 如果父类没有就按照(3)的规则，继续找上级父类，直到 Object...
  }
}

class GrandPa {
  String name = "大头爷爷";
  String hobby = "旅游";
}

class Father extends GrandPa {
  String name = "大头爸爸";
  int age = 39;
}

class Son extends Father {
  String name = "大头儿子";
}
```
![](media/16629433472925/16639427571455.jpg)

![继承本质分析](media/16629433472925/16635756522665.png)
#### 6. 练习
* `B b = new B();`输出结果
```Java
package extend;

public class ExtendsExercise {
  public static void main(String[] args) {
    B b = new B();
  }
}

class A {
  A() {
    System.out.println("a");
  }
  
  A(String name) {
    System.out.println("a name " + name);
  }
}

class B extends A {
  B() {
    // this和super必须在第一行，且不能同时存在
    // 要么执行super，要么执行this
    this("abc"); // 执行本类带String类型的构造器
    System.out.println("b");
  }
  
  B(String name) {
    // 没有this，也没有显式super(参数列表)，就隐式调用 super();
    super("fishx");
    System.out.println("b name " + name);
  }
}
```
* 实现
```test
编写 Computer 类，包含 CPU、内存、硬盘等属性，getDetails 方法用于返回 Computer 的详细信息
编写 PC 子类，继承 Computer 类，添加特有属性【品牌 brand】 
编写 NotePad 子类，继承 Computer 类，添加特有属性【color】 
main 方法中创建 PC 和 NotePad 对象，分别给对象中特有的属性赋值，
        以及从 Computer 类继承的 属性赋值，并使用方法并打印输出信息.
```
```Java
package extend.exercise;

public class ExtendsExercise01 {
  public static void main(String[] args) {
    PC pc = new PC("8G", "64G", "128G", "戴尔");
    NotePad notePad = new NotePad("8G", "128G",
        "128G", "银色");
    pc.getPcShow();
    notePad.getNotePadShow();
  }
}

class Computer {
  private String CPU;
  private String Memory;
  private String HardDisk;

  public Computer(String CPU, String memory, String hardDisk) {
    this.CPU = CPU;
    Memory = memory;
    HardDisk = hardDisk;
  }

  public String getDetails() {
    return "CPU: " + CPU + " 内存：" + Memory + " 硬盘：" + HardDisk;
  }
}

class PC extends Computer {
  private String brand;

  public PC(String CPU, String memory, String hardDisk, String brand) {
    super(CPU, memory, hardDisk);
    this.brand = brand;
  }

  public String getBrand() {
    return brand;
  }

  public void setBrand(String brand) {
    this.brand = brand;
  }

  public void getPcShow() {
    System.out.println("Pc信息: ");
    System.out.println(getDetails() + " brand：" + brand);
  }
}

class NotePad extends Computer {
  private String color;

  public NotePad(String CPU, String memory, String hardDisk, String color) {
    super(CPU, memory, hardDisk);
    this.color = color;
  }

  public String getColor() {
    return color;
  }

  public void setColor(String color) {
    this.color = color;
  }

  public void getNotePadShow() {
    System.out.println("NotePad信息: ");
    System.out.println(getDetails() + " color：" + color);
  }
}
```
### 5. 面向对象编程三大特征 —— 多态
#### 1. 多态基本介绍
多`[多种]`态`[状态]`, 方法或对象具有多种形态。是面向对象的第三大特征，多态是建立在封装和继承基础之上的。
![多态](media/16629433472925/%E5%A4%9A%E6%80%81.png)

#### 2. 方法的多态
```Java
package polymorphic.playMethod;

public class playMethod {
  public static void main(String[] args) {
    // 方法重载体现多态
    A a = new A(); // 这里我们传入不同的参数， 就会调用不同 sum 方法，就体现多态
    System.out.println(a.sum(10, 20));
    System.out.println(a.sum(10, 20, 30));

    // 方法重写体现多态
    B b = new B();
    a.say();
    b.say();
  }
}

class B { // 父类
  public void say() {
    System.out.println("B say() 方法被调用...");
  }
}

class A extends B { // 子类
  public int sum(int n1, int n2) { // 和下面 sum 构成重载
    return n1 + n2;
  }

  public int sum(int n1, int n2, int n3) {
    return n1 + n2 + n3;
  }

  public void say() {
    System.out.println("A say() 方法被调用...");
  }
}
```
#### 3. 对象的多态
1. 一个对象的编译类型和运行类型可以不一致
2. 编译类型在定义对象时,就确定了,不能改变
3. 运行类型是可以变化的
4. 编译类型看定义时 = 号 的左边,运行类型看 = 号的右边 `[编译类型 = 运行类型]`
    ```java
    package polymorphic.playMethod;
    
    public class PolyObject {
      public static void main(String[] args) {
        // animal 编译类型即 Animal（确定✅, 不再更改）, 运行类型 Dog
        Animal animal = new Dog();
        // 因为运行时，执行该行时, animal运行类型时Dog, 所以Cry就是Dog的Cry
        animal.Cry(); // 小狗汪汪叫
    
        // animal 编译类型即 Animal, 运行类型就是Cat
        animal = new Cat();
        animal.Cry(); // 小猫喵喵叫
      }
    }
    
    public class Animal {
      public void Cry() {
        System.out.println("Animal cry() 动物在叫....");
      }
    }
    
    public class Cat extends Animal{
      public void Cry() {
        System.out.println("Cat cry() 小猫喵喵叫...");
      }
    }
    
    public class Dog extends Animal{
      public void Cry() {
        System.out.println("Dog cry() 小狗汪汪叫...");
      }
    }
    ```
#### 4. 多态注意事项和细节
* 多态的前提是：两个对象(类)存在继承关系
##### 1. 多态向上转型:
1. 本质: 父类的引用指向子类的对象
2. 语法: `父类类型 引用名 = new 子类类型()`
3. 特点: 
    ```java
    package polymorphic.detail;
    
    public class Animal {
      String name;
      int age;
      public void sleep() {
        System.out.println("睡");
      }
      public void run() {
        System.out.println("跑");
      }
      public void eat() {
        System.out.println("吃");
      }
      public void show() {
        System.out.println("Hello~");
      }
    }
    ``` 
    ```java
    package polymorphic.detail;
    
    public class Cat extends Animal {
    
      public void eat() {
        // 重写eat方法
        System.out.println("猫吃鱼～");
      }
    
      public void catchMouse() {
        System.out.println("猫抓老鼠！");
      }
    }
    ```
    ```java
    package polymorphic.detail;
    
    public class detail {
      public static void main(String[] args) {
        // 向上转型： 父类引用 指向 子类对象
        // 语法：父类类型 引用名 = new 子类类型();
        Animal animal = new Cat();
        // Object obj = new Cat(); // Object => Animal => Cat
        // 在遵守访问权限条件下, 可以调用父类中的所有成员(属性和方法)
        animal.show();
        animal.sleep();
        // 最终运行效果看子类(运行类型)的具体实现
        // 即(调用方法时，先查找子类(运行类型) => 父类,然后调用)
        animal.eat(); // 子类有 => 输出
        animal.run(); // 子类无 => 父类有 => 输出
        // 但不能调用子类特有成员,
        // 因为在编译阶段, 能调用哪些成员, 是由编译类型来决定的
        // animal.catchMouse(); // (x) 'Animal' 中的没有此方法
      }
    }
    ```
    1. 编译类型看左边,运行类型看右边 `[编译类型 = 运行类型]`
    2. 可以调用父类中所有成员(需要遵守访问权限)
    3. 不能调用子类中特有成员
    4. 最终运行效果看子类的具体实现
##### 2. 多态向下转型:
1. 语法: `子类类型 引用名 = (子类类型) 父类引用`
2. 只能强转父类引用,不能强转父类对象
3. 要求父类的引用必须指向的是当前目标类型的对象
   ![无标题画板](media/16629433472925/16643754303627.jpg)
4. 当向下转型后,可以调用子类类型中所有的成员
5. <span style="color: red;">在实际开发中,为了代码简便性,一般会直接使用父类作为数据类型,当子类出现特定方法时就需要使用向下转型,进行特定方法的使用</span>
6. 之所以要用父类引用然后向下转型，就是为了可扩展性啊，如果直接创建具体对象，以后维护代码岂不是要处处改动，设计者用多态应该就是抽象出共性然后以最小侵入实现灵活性
7. 向上转型是为了用多态，向下转型是为了用特有方法
```Java
public static void main(String[] args) {
    Animal animal = new Cat();
    // 语法：子类类型 引用名 = (子类类型) 父类引用
    // 编译类型 Cat，运行类型 Cat
    Cat cat = (Cat)animal;
    cat.catchMouse();
    // ((Cat) animal).catchMouse();
    // 原则: 要求父类的引用必须指向的是当前目标类型的对象
    // 父类和向下转型的目标必须指向的是一致的
    // Dog dog = (Dog) animal;
    // 将指向猫的运行类型强转为狗 ？=> 抛出：类异常
}
```
##### 3. 属性重写
* 属性没有重写之说, 属性的值看编译类型
    ```Java
    package polymorphic.detail;
    
    public class PolyDetail {
      public static void main(String[] args) {
        // 属性没有重写, 属性的值看编译类型
        Base base = new Sub(); // 向上转型
        System.out.println(base.count); // Base => (count = 10)
        Sub sub = new Sub();
        System.out.println(sub.count); // Sub => (count = 20)
      }
    }
    
    class Base { // 父类
      int count = 10; // 属性
    }
    
    class Sub extends Base { // 子类
      int count = 20; // 属性
    }
    ```
##### 4. instanceOf 
* instanceOf 比较操作符，用于判断对象的运行类型是否为 XX 类型或 XX 类型的子类型
```Java
package polymorphic.detail;

public class PolyDetail01 {
  // 用于判断对象的运行类型是否为 XX 类型或 XX 类型的子类型
  public static void main(String[] args) {
    BB bb = new BB(); // bb 运行类型 BB
    System.out.println(bb instanceof BB); // true
    System.out.println(bb instanceof AA); // true

    // aa 编译类型 AA, 运行类型是 BB
    AA aa = new BB(); // BB运行类型 是 AA运行类型的子类型
    System.out.println(aa instanceof AA); // true
    // aa 编译类型是 AA, 运行类型 BB
    System.out.println(aa instanceof BB); // true

    Object obj = new Object();
    // obj 运行类型、编译类型是 Object, obj 是 AA 的子类     
    System.out.println(obj instanceof AA); // false
    String str = "hello";
    // System.out.println(str instanceof AA); false
    // str 是 Object类型
    System.out.println(str instanceof Object); // true
  }
}

class AA { }

class BB extends AA { }
```
#### 5.练习
```Java
package polymorphic.Exercise;

public class PolyExercise01 {
  public static void main(String[] args) {
    Sub sub = new Sub();
    System.out.println(sub.count); // 20
    sub.display(); // 20

    Base base = sub;
    System.out.println(base == sub);// true
    System.out.println(base.count); // 属性没有重写，看编译类型 => 10
    base.display(); // 20 (调用方法时，先查找子类(运行类型) => 父类,然后调用)
  }
}

class Base {
  int count = 10;

  public void display() {
    System.out.println(this.count);
  }
}

class Sub extends Base {
  int count = 20;

  public void display() {
    System.out.println(this.count);
  }
}
```
#### 6.动态绑定机制
![](media/16629433472925/16636779145741.jpg)
#### 7.多态数组
```text
现有一个继承结构如下：
    要求创建 1 个 Person 对象、2 个 Student 对象和 2 个 Teacher 对象,
    统一放在数组中，并调用每个对象 say 方法.
应用实例升级：如何调用子类特有的方法，
```
```Java
package polymorphic.Array.beforehand;

public class Person {
  private String name;
  private int age;

  public Person(String name, int age) {
    setAge(age);
    setName(name);
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }

  public int getAge() {
    return age;
  }

  public void setAge(int age) {
    this.age = age;
  }

  public String Say() {
    return "name: " + name + "\tage: " + age;
  }
}
```
```Java
package polymorphic.Array.beforehand;

public class Teach extends Person {
  private double salary;

  public Teach(String name, int age, double salary) {
    super(name, age);
    setSalary(salary);
  }

  public double getSalary() {
    return salary;
  }

  public void setSalary(double salary) {
    this.salary = salary;
  }

  public void teach() {
    System.out.println(getName() + "老师正在讲课...");
  }

  @Override
  public String Say() {
    return super.Say() + "\tsalary = " + salary;
  }
}
```
```Java
package polymorphic.Array.beforehand;

public class Student extends Person {
  private double score;

  public Student(String name, int age, double score) {
    super(name, age);
    setScore(score);
  }

  public double getScore() {
    return score;
  }

  public void setScore(double score) {
    this.score = score;
  }

  public void study() {
    System.out.println(getName() + "同学正在听课...");
  }

  @Override
  public String Say() {
    return super.Say() + "\tscore = " + score;
  }
}
```
```Java
package polymorphic.Array.beforehand;

public class beforehand {
  public static void main(String[] args) {
    Person[] people = new Person[5];
    people[0] = new Person("fishx", 20);
    people[1] = new Student("小名", 19, 90);
    people[2] = new Student("小张", 20, 50.5);
    people[3] = new Teach("Mr.Wang", 31, 4000);
    people[4] = new Teach("Ms.Lee", 29, 5000);

    for (int i = 0; i < people.length; i++) {
      // people的编译类型是 Person
      // 运行类型随之而变[Person,Student,Student,Teach,Teach]
      System.out.println(people[i].Say());

      // 多态向下转型调用子类特有方法
      // 判断, people[i]的运行类型 是不是 Student（必须是继承关系 => 多态）
      if (people[i] instanceof Student) {
        // ((Student) people[i]).study();
        Student student = (Student) people[i];
        student.study();
      } else if (people[i] instanceof Teach) {
        ((Teach) people[i]).teach();
      } else if (people[i] != null) {
        System.out.println("Person没有特有方法");
      } else {
        System.out.println("类型有误！");
      }
    }
  }
}
```
#### 8.多态参数
* 方法定义的形参类型为父类类型,实参类型允许为子类类型
```test
定义员工类Employee(属性: name、月salary; 方法: 计算年薪getAnnual)
普工和经理都继承员工类,且都重写getAnnual方法
    经理多了奖金bonus属性和管理方法,普工多了work方法
测试类添加方法showEmpAnnual(Employee e),实现任何员工对象的年薪,并调用
再添加testWork方法,普工 => work、经理 => manage
```
```Java
package polymorphic.parameter;

public class Employee {
  private String name;
  private double salary;

  public Employee(String name, double salary) {
    setName(name);
    setSalary(salary);
  }

  public String getName() {
    return name;
  }
  public void setName(String name) {
    this.name = name;
  }
  public double getSalary() {
    return salary;
  }
  public void setSalary(double salary) {
    this.salary = salary;
  }

  public double getAnnual() {
    return salary * 12;
  }
}
```
```Java
public class Common extends Employee {
  public Common(String name, double salary) {
    super(name, salary);
  }

  public void work() {
    System.out.println(getName() + "正在工作中...");
  }

  @Override
  public double getAnnual() { // 普工没有额外奖金
    return super.getAnnual();
  }
}
```
```Java
package polymorphic.parameter;

public class Manager extends Employee {
  private double bonus;

  public Manager(String name, double salary, double bonus) {
    super(name, salary);
    setBonus(bonus);
  }

  public double getBonus() {
    return bonus;
  }

  public void setBonus(double bonus) {
    this.bonus = bonus;
  }

  public void manage() {
    System.out.println(getName() + "管理中...");
  }

  @Override
  public double getAnnual() { // 管理层有额外奖金
    return super.getAnnual() + bonus;
  }
}
```
```Java
package polymorphic.parameter;

public class parameter {
  public static void main(String[] args) {
    Common tom = new Common("Tom", 1);
    Manager fishx = new Manager("fishx", 1, 4000);
    parameter parameter = new parameter();
    parameter.showEmpAnnual(tom);
    parameter.showEmpAnnual(fishx);
    parameter.testWork(tom);
    parameter.testWork(fishx);
  }

  public void showEmpAnnual(Employee e) {
    System.out.println(e.getAnnual()); //动态绑定
  }

  public void testWork(Employee e) {
    if (e instanceof Manager) {
      ((Manager) e).manage();
    } else if (e instanceof Common) {
      ((Common) e).work();
    }
  }
}
```
### 6. super 关键字
#### 1. 基本介绍
super 代表父类的引用，用于访问父类的属性、方法、构造器
#### 2. 基本语法
```Java
// 1.访问父类的属性,不能访问父类的private属性
super.age;
// 2.访问父类的方法,不能访问父类的private方法
super.getInfo("fishx");
// 3.访问父类的构造器
super("fishx",20); // 必须在构造函数的第一句且只能出现一句
```
```java
public class B extends A {
  public B() {
    // 3.访问父类的构造器, super(参数列表);
    super("fishx", 20);
  }

  public void hi() {
    // 1.访问父类的属性,不能访问父类的private属性
    System.out.println(super.n1 + " " + super.n2 + " " + super.n3);
    // 🙅 super.属性, super.n4具有 private 访问权限

    // 2.访问父类的方法,不能访问父类的private方法
    super.test100();
    super.test200();
    super.test300();
    // 🙅 super.方法(), super.test400(); 具有 private 访问权限
  }
}
```
#### 3. super细节
1. 调用父类的构造器的好处(分工明确,父类属性由父类初始化,子类的属性由子类初始化)
2. 当子类中有和父类中的成员(属性和方法)重名时,为了访问父类的成员,必须通过super,如果没有重名,使用super、this、直接访问是一样的
3. super的访问不限于直接父类,如果多个基类(上级类)中都有同名的成员,使用super访问遵循就近原则 
```java
public void sum() {
    System.out.println("B 类的 sum()");
    //希望调用父类-A 的 cal 方法
    //这时，因为子类 B 没有 cal 方法，因此我可以使用下面三种方式
    
    // 找 cal 方法时(cal() 和 this.cal())，顺序是:
    // (1)先找本类，如果有，则调用
    // (2)如果没有，则找父类(如果有，并可以调用，则调用)
    // (3)如果父类没有，则继续找父类的父类,整个规则，就是一样的,直到 Object 类
    // 提示：如果查找方法的过程中，找到了，但是不能访问， 则报错, cannot access
    // 如果查找方法的过程中，没有找到，则提示方法不存在
    cal();
    this.cal(); //等价 cal
    //找 cal 方法(super.call()) 的顺序是直接查找父类，其他的规则一样
    super.cal();
    
    // 演示访问属性的规则:
    // n1 和 this.n1 查找的规则是
    // (1) 先找本类，如果有，则调用
    // (2) 如果没有，则找父类(如果有，并可以调用，则调用)
    // (3) 如果父类没有，则继续找父类的父类,整个规则，就是一样的,直到 Object 类
    // 提示：如果查找属性的过程中，找到了，但是不能访问， 则报错, cannot access
    // 如果查找属性的过程中，没有找到，则提示属性不存在
    System.out.println(n1);
    System.out.println(this.n1);
    
    //找 n1 (super.n1) 的顺序是直接查找父类属性，其他的规则一样
    System.out.println(super.n1);
}
```
#### 4. super 和 this 的比较

| 区别点   | this                      | super               |
|:-----:|:-------------------------:|:-------------------:|
| 访问属性  | 访问本类属性,如果本类没有此属性则从父类中继续查找 | 从父类开始查找属性           |
| 调用方法  | 访问本类中方法,如果本类没有此方法则从父类继续查找 | 从父类开始查找方法           |
| 调用构造器 | 调用本类构造器,必须放在构造器的首行        | 调用父类的构造器,必须子类构造器的首行 |
| 特殊    | 表示当前对象                    | 子类中访问父类对象           |

### 7. 方法重写/覆盖(override)
#### 1.简单的说: 
* 方法覆盖(重写)就是`子类`有一个`方法`,和`父类`的某个`方法的名称、返回类型、参数一样`,那么`子类`的这个方法`覆盖`了父类的方法
```Java
package override;

public class overrideTest {
  public static void main(String[] args) {
    Cat cat = new Cat();
    cat.cry();
  }
}

public class Cat extends Animal {
  public void cry() {
    System.out.println("小猫叫");
  }
}

public class Animal {
  public void cry() {
    System.out.println("动物叫唤");
  }
}
```
#### 2.注意事项和使用细节
* 子类的方法的`形参列表,方法名称`,要和父类方法的`形参列表,方法名称`完全一样
* 子类方法的`返回类型`和父类方法`返回类型`一样, 或者是父类返回类型的子类
* 子类方法不能缩小父类方法的访问权限`[public => protected => 默认 => private]`
#### 3.overload 和 override 的比较

| 名称        | 发生范围 | 方法名  | 形参列表                    | 返回类型              | 修饰符 |
|:------------:|:----:|:----:|:-----------------------------:|:-----------------:|:---:|
| 重载(overload) | 本类 | 必须相同 | 类型,个数或顺序至少有一个不同  | 无要求               | 无要求 |
| 重写(override) | 父子类| 必须相同| 相同| 子类重写的方法返回的类型和父类返回的类型一样,或者是其子类 | 子类方法不能缩写父类方法的访问范围 |  无要求   |

#### 4. 练习
1. 编写一个 Person 类，包括属性/private（name、age），构造器、方法 say(返回自我介绍的字符串）。
2. 编写一个 Student 类，继承 Person 类，增加 id、score 属性/private，以及构造器，定义 say 方法(返回自我介绍的信息)。
3. 在 main 中,分别创建 Person 和 Student 对象，调用 say 方法输出自我介绍
```java
package override;

public class Person {
  private String name;
  private int age;

  public Person(String name, int age) {
    setName(name);
    setAge(age);
  }

  public String getName() {
    return name;
  }
  public void setName(String name) {
    this.name = name;
  }
  public int getAge() {
    return age;
  }
  public void setAge(int age) {
    this.age = age;
  }

  public String Say() {
    return "大家好，我叫" + name + "，今年" + age;
  }
}
```
```Java
package override;

public class Student extends Person {
  private String id;
  private double score;

  public Student(String name, int age, String id, double score) {
    super(name, age);
    this.id = id;
    this.score = score;
  }

  public String Say() {
    // (1).return "大家好，我叫" + getName() + "，今年" + getAge();//x 代码复用率低 🙅‍
    // (2).super从父类开始查找方法
    return super.Say() + "\n我的学号：" + id + " 我的score：" + score; // √
    // (3).this和Say()都是从本类开始查找方法，导致形成递归，因为本类方法没有return导致堆栈溢出错误 🙅‍
    // return this.Say() + "\n我的学号：" + id + " 我的score：" + score; // x
  }
}
```
```Java
package override;

public class overrideTest {
  public static void main(String[] args) {
    Student student = new Student("fishx", 20, "S2001", 81);
    System.out.println(student.Say());
  }
}
```
### 8. Object 类详解
#### 1. equals 方法
##### 1.==和 equals 的对比

| 名称 | \== | equals |
| :-: | :-: | :-: |
| 类型 | 是一个比较运算符 | 是Object类中方法 |
| 范围 | 既可以判断基本类型,又可判断引用类型 | 默认判断地址是否相等 |
| 其它 | 基本类型: 判断值是否相等,<br/> 引用类型: 判断地址是否相等(是否为同一对象) | 默认判断地址是否相等,子类往往重写,用于判断内容是否相等 |

##### 2.练习
```Java
package Object_detail;

import java.util.Objects;

public class judge {
  public static void main(String[] args) {
    Person person = new Person("fishx", 20, '男');
    Person person1 = new Person("fishx", 20, '男');
    // equals方法改写之前    
    System.out.println(person.equals(person1)); // 开辟了两个空间=>false
   /* object的equals是默认的地址比较(即是不是同一个对象)
    public boolean equals (Object obj){
      return (this == obj);
    } */
  }
}

class Person {
  private String name;
  private int age;
  private char gender;

  public Person(String name, int age, char gender) {
    setAge(age);
    setGender(gender);
    setName(name);
  }

  @Override
  public boolean equals(Object o) {
    // 先判断这两个是否相等
    if (this == o) {
      return true;
    }
    // 再比较这两个对象的属性是否相同
    if (o instanceof Person) {
      // 向下转型，取子类特殊属性和方法
      Person p = (Person) o;
      // return this.age.equals(p.age); x 是Object类中方法,只能判断引用类型
      return this.name.equals(p.name) && this.age == p.age && this.gender == p.gender;
    }

    return false;
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }

  public int getAge() {
    return age;
  }

  public void setAge(int age) {
    this.age = age;
  }

  public char getGender() {
    return gender;
  }

  public void setGender(char gender) {
    this.gender = gender;
  }
}
```
```Java
package Object_detail.equals.Exercise;

public class judgeExercise {
  public static void main(String[] args) {
    Person p1 = new Person();
    p1.name = "fishx";
    Person p2 = new Person();
    p2.name = "fishx";
    // == (基本类型：值比较, 引用类型：地址比较)
    System.out.println(p1 == p2); // 开辟了两个空间 => false
    
    // p1.name类型是String, String对象有equals方法重写,比较的是内容
    System.out.println(p1.name.equals(p2.name)); // 内容相同 => true
    
    // 类型是Person且没有重写equals方法，因此是Object默认的equals比较地址
    System.out.println(p1.equals(p2)); // 开辟了两个空间 => false

    String s1 = new String("fish");
    String s2 = new String("fish");
    // String对象有equals方法重写,比较的是内容
    System.out.println(s1.equals(s2)); // true
    
    // 类型是String对象,对象就是引用类型,引用类型比较的是地址
    System.out.println(s1 == s2);// false
  }
}

class Person {
  public String name;
}
```
#### 2. hashCode 方法
> `public int hashCode() // 返回该对象的哈希码值.支持此方法是为了提高哈希表的性能`<br/>
> 实际上,Object类定义的hashCode方法确实会针对不同对象返回不同的整数<br/>
> 这一般是通过将该对象的内部地址转换成一个整数来实现的,但Java不需要这个实现技巧

1. 提高具有哈希结构的容器的效率！
2. 两个引用，如果指向的是同一个对象，则哈希值肯定是一样的！
3. 两个引用，如果指向的是不同对象，则哈希值是不一样的
4. 哈希值主要根据地址号来的！， 不能完全将哈希值等价于地址。
```Java
package Object_detail.hasCode;

public class HasCode {
  public static void main(String[] args) {
    A a = new A();
    A a1 = new A();
    A a2 = a;
    System.out.println("a.hashCode() = " + a.hashCode());// 21685669
    System.out.println("a1.hashCode() = " + a1.hashCode());// 2133927002
    System.out.println("a2.hashCode() = " + a2.hashCode());// 21685669
  }
}

class A { }
```
#### 3. toString 方法
```java
public String toString() // 返回该对象的字符串
// toString方法会返回一个“以文本形式方式表示”此对象的字符串
// Object类的toString方法返回一个字符串(由类名、@和此对象哈希码的无符号十六进制表示组成)
getClass().getName() + '@' + Integer.toHexString(hashCode()) // 默认返回
// 子类往往重写 toString 方法，用于返回对象的属性信息
```
* 基本介绍 
    * 默认返回：全类名+@+哈希值的十六进制,子类往往重写 toString 方法，用于返回对象的属性信息
* 重写 toString 方法，打印对象或拼接对象时，都会自动调用该对象的 toString 形式.
```java
package Object_detail.toString;

public class ToString {
  public static void main(String[] args) {
    /* Object类中默认的toString()
      public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
      }
    */
    Monster monster = new Monster("fishx", "看守", 1200);
    System.out.println(monster.toString());
    System.out.println("==当直接输出一个对象时，toString 方法会被默认的调用==");
    System.out.println(monster); //等价 monster.toString()
  }
}

class Monster {
  private String name;
  private String job;
  private double salary;

  public Monster(String name, String job, double salary) {
    setName(name);
    setSalary(salary);
    setJob(job);
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }

  public String getJob() {
    return job;
  }

  public void setJob(String job) {
    this.job = job;
  }

  public double getSalary() {
    return salary;
  }

  public void setSalary(double salary) {
    this.salary = salary;
  }

  @Override
  public String toString() {
    return "Monster{" +
        "name='" + name + '\'' +
        ", job='" + job + '\'' +
        ", salary=" + salary +
        '}';
  }
}
```
#### 4. finalize 方法
1. 当对象被回收时，系统自动调用该对象的 finalize 方法。子类可以重写该方法，做一些释放资源的操作【演示】
2. 什么时候被回收：当某个对象没有任何引用时，则 jvm 就认为这个对象是一个垃圾对象，就会使用垃圾回收机制来 销毁该对象，在销毁该对象前，会先调用 finalize 方法。
3. 垃圾回收机制的调用，是由系统来决定(即有自己的 GC 算法), 也可以通过 System.gc() 主动触发垃圾回收机制，测
```java
package Object_detail.finalize;

public class finalize {
  public static void main(String[] args) {
    Car bmw = new Car("宝马");
    bmw = null;
    System.gc();
  }
}

class Car {
  private String name;

  public Car(String name) {
    setName(name);
  }

  public void setName(String name) {
    this.name = name;
  }

  @Override
  protected void finalize() throws Throwable {
    System.out.println("我们销毁了汽车" + name);
    System.out.println("释放类某些资源...");
  }
}
```
### 9. 本章作业
#### 1. 项目-零钱通
##### 1. 项目需求说明
使用 Java 开发 零钱通项目 , 可以完成收益入账，消费，查看明细，退出系统等功能.
##### 2. 项目的界面
![](media/16629433472925/16639257276441.jpg)
1. 先完成显示菜单，并可以选择
2. 完成零钱通明细.
3. 完成收益入账
4. 消费
5. 退出
#### 2. 对象数组、排序、重写ToString方法
##### 项目需求说明
定义Person类(属性: name, age, job), 初始化Person对象数组[3], 并按age从大到小排序 `冒泡排序`
输出属性

#### 4. 综合
![](media/16629433472925/16639372176672.jpg)

```Java
package chapterWork.Homework09;

public class Person {
  private String name;
  private char sex;
  private int age;

  public Person(String name, char sex, int age) {
    setName(name);
    setAge(age);
    setSex(sex);
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }

  public char getSex() {
    return sex;
  }

  public void setSex(char sex) {
    this.sex = sex;
  }

  public int getAge() {
    return age;
  }

  public void setAge(int age) {
    this.age = age;
  }

  public void play() {
    System.out.println(name + "爱玩");
  }

  public String printInfo() {
    return "姓名：" + name + "\n" + "年龄：" + age + "\n" + "性别：" + sex + "\n";
  }
}
``` 
```Java
public class Student extends Person {
  private String stu_id;

  public Student(String name, char sex, int age, String stu_id) {
    super(name, sex, age);
    setStu_id(stu_id);
  }

  public String getStu_id() {
    return stu_id;
  }

  public void setStu_id(String stu_id) {
    this.stu_id = stu_id;
  }

  public void study() {
    System.out.println("我承诺，我会好好学习");
  }

  @Override
  public void play() {
    System.out.println(getName() + "爱踢足球");
  }

  @Override
  public String printInfo() {
    return "学生的信息：\n" + super.printInfo()+ "学号：" + getStu_id();
  }
}
```
```Java
public class Teacher extends Person {
  private double work_age;

  public Teacher(String name, char sex, int age, double work_age) {
    super(name, sex, age);
    setWork_age(work_age);
  }

  public double getWork_age() {
    return work_age;
  }

  public void setWork_age(double work_age) {
    this.work_age = work_age;
  }

  public void teach() {
    System.out.println("我承诺，我会认真教学");
  }

  @Override
  public void play() {
    System.out.println(getName() + "爱玩象棋");
  }

  @Override
  public String printInfo() {
    return "老师的信息：\n" + super.printInfo() + "工龄：" + getWork_age() + "年";
  }
}
```
```Java
public class homework09 {
  public static void main(String[] args) {
    Person[] people = new Person[4];
    people[0] = new Student("骁骁", '男', 20, "S2021");
    people[1] = new Student("乐乐", '女', 19, "S2021");
    people[2] = new Teacher("Ms.Lee", '女', 31, 4.5);
    people[3] = new Teacher("Mr.Wang", '男', 34, 6.0);
    Person tmp;
    for (int i = 0; i < people.length - 1; i++) {
      for (int j = 0; j < people.length - 1 - i; j++) {
        if (people[j].getAge() < people[j + 1].getAge()) {
          tmp = people[j];
          people[j] = people[j + 1];
          people[j + 1] = tmp;
        }
      }
    }
    for (int i = 0; i < people.length; i++) {
      System.out.println(people[i].printInfo());
      if (people[i] instanceof Student) {
        ((Student) people[i]).study();
      }
      if (people[i] instanceof Teacher) {
        ((Teacher) people[i]).teach();
      }
      people[i].play();
      if (i != 3) {
        System.out.println("-----------------------------------------");
      }
    }
  }
}
```
#### 5. super调用
```Java
package chapterWork.Homework10;

public class homework10 {
  public static void main(String[] args) {
    C c = new C();
    // 调用super(): 父类B的无参构造，初始化
  }
}

public class C extends B {
  public C() {
    this("hello"); // 调用本类有参方法
    System.out.println("我是C类的无参构造");// 输出(4)
  }

  public C(String name) {
    super(name); // 调用父类B有参方法
    System.out.println(name + "我是C类的有参构造");// 输出(3)
  }
}

public class B extends A {
  public B() {
    System.out.println("我是B类的无参构造");
  }

  public B(String name) {
    // 默认调用super()，父类方法
    System.out.println(name + "我是B类的有参构造");// 输出(2)
  }
}

public class A {
  public A() {
    System.out.println("我是A类"); // 输出(1)
  }
}
```
#### 6. 总结
##### 1. 多态
多态: 方法或对象具有多种形态,是OPP的第三大特征,是建立在封装和继承基础之上
##### 2. 多态具体体现
* 方法多态`p358`
    * ( 1 ) 重载体现多态
    * ( 2 ) 重写体现多态
* 对象多态
    * ( 1 ) 对象的编译类型和运行类型可以不一致,编译类型在定义时,就确定,不能变化
    * ( 2 ) 对象的运行类型是可以变化的,可以通过getClass()来查看运行类型
    * ( 3 ) 编译类型看定时是 = 号左边, 运行类型看 = 号右边
##### 3. 动态绑定机制
1. 当调用对象的方法时, 该方法会和对象的内存地址/运行类型绑定`p359`
2. 当调用对象的属性时, 没有动态绑定机制,哪里声明,哪里使用 
## 九、面向对象 (高级)
### 1. 类变量

#### 3. 写出四种访问修饰符和各自访问权限

| 访问级别 | 修饰符 | 同类 | 同包 | 子类 | 不同包 |
| :-: | :-: | :-: | :-: | :-: | :-: |
| 公开 | public | √ | √ | √ | √ |
| 受保护 | protected | √ | √ | √ | x |
| 默认 | 没有修饰符 | √ | √ | x | x |
| 私有 | private | √ | x | x | x |

![](media/16629433472925/16639265655957.jpg)
![](media/16629433472925/16639265866322.jpg)
![](media/16629433472925/16639265914857.jpg)
![](media/16629433472925/16639266044391.jpg)
![](media/16629433472925/16639306886983.jpg)
```java
package chapterWork.Homework08;

public class Perosn {
  public void run() {
    System.out.println("Person run");
  }

  public void eat() {
    System.out.println("Person eat");
  }
}
```
```java
package chapterWork.Homework08;

public class Student extends Perosn {
  @Override
  public void run() {
    System.out.println("student run");
  }

  public void study() {
    System.out.println("student study");
  }
}
```
```java
public static void main(String[] args) {
    // 向上转型(父类的引用 ==指向=> 子类)
    Perosn p = new Student();
    // 编译阶段，代码只看编译类型
    p.run(); // 运行阶段就看运行类型 => student run
    p.eat(); // 运行类型是Student，子类没有 => 找父类
    
    // 向下转型
    Student s = (Student) p;
    s.run(); // student run
    s.study(); // student study
    s.eat(); // Person eat
}
```

#### 1. 类变量概念
* 类变量也叫静态变量/静态属性,是该类的所有对象共享的变量
    * 任何一个该类的对象去访问它时,取到的都是相同的值
    * 同样任何一个该类的对象去修改它时,修改的是同一个变量
#### 2. 如何定义类变量
```java
class chlid {
  // 访问修饰符 static 数据类型 变量名; 
  public static int count;
}
```
#### 3. 如何访问类变量
* 类变量的访问必须遵循访问修饰符的访问范围
    ```java
    child.count++; // 类名.类名变量名
    ```
#### 4. 类变量内存布局
- static变量是对象共享
- 不管static变量在哪里
    - 1.static变量是同一个类所有对象共享
    - 2.static类变量,在类加载时就生成了
    
![类变量内存分析](media/16629433472925/16640942585986.png)
#### 5. 注意事项和细节
> 1. 什么时候需要用类变量
* 当某个类的所有对象需要共享一个变量时,就可以考虑使用类变量(静态变量)
> 2. 类变量与实例变量的区别
* 类变量是该类所有对象共享的,而实例对象是每个对象独享的
> 3. 加上`static`称为类变量或静态变量,否则称为实例变量/普通变量/非静态变量
* 类变量可以通过 `类名.类变量名` or `对象名.类变量名`来访问
* 实例变量不能通过 `类名.类变量名` 来访问
* 前提是 满足访问修饰符的访问权限和范围
> 4. 类变量是在类加载就完成了初始化, 就是说哪怕**没有创建对象**,**只要类加载了**,就**可以使用类变量**了

> 5. 类变量的生命周期是随着类的加载开始,随着类消亡而销毁

### 2. 类方法
#### 1. 类方法概念
* 类方法就叫静态方法,被`static`所定义的方法
    ```Java
    class child {
      public static void Say() {
        System.out.print("你好");
      }
    }
    ```
* 静态方法就可以访问静态属性

#### 2. 类方法的调用
* `前提是: `满足访问修饰符的访问权限范围
    ```java
    Child child = new Child();
    child.Say();
    ```
#### 3. 类方法的使用场景
> 当方法中不涉及到任何与对象相关的成员,则可以将方法设计成静态方法,提高开发效率
* 工具类,不用对对象实例化,即可使用(比如:Math类、Array类...)
* `static`定义后,静态方法不需要实例化对象占用内存即可使用
#### 4. 注意事项和细节
1. 类方法和普通方法都是随着类的加载而加载,将结构信息存储在方法区
    * 类方法中没有this的参数, 实例方法中有
2. 类方法可以通过类名调用,也可以通过对象名调用
3. 普通方法和对象有关,需要通过对象名调用 `对象名.方法名(参数)`,不能通过类名调用
4. 类(静态)方法中不允许使用与对象有关的关键字,比如`this、super`
5. 类方法中只能访问静态变量或静态方法
6. 普通成员方法,既可以访问非静态成员,也可以访问静态成员
7. `总结: (必须遵循访问权限)`
    * `静态方法,只能访问静态的成员`
    * `非静态的方法,可以访问静态成员和非静态成员`

### 3. 理解main方法语法
![](media/16629433472925/16641028852338.jpg)
- `静态方法,只能访问静态的成员,非静态的方法,可以访问静态成员和非静态成员`
### 4. 代码块
#### 1. 基本介绍
代码块又称为初始化块,属于类中的成员,类似于方法[但无方法名、无返回、无参数、只有方法体],而且不用通过对象或类显示调用,而是`加载类时`或`创建对象时` `隐式调用`
```Java
class Movie {
  private String name;
  {
    System.out.println("电影屏幕开启...");
    System.out.println("观众入场...");
    System.out.println("电影正式放映...");
  }
  public Movie(String name) {
    System.out.println("Movie(String name) 被调用...");
    this.name = name;
  }
}
```
* 说明注意:
    * 修饰符可选,但只能是static
    * 逻辑语句可以为任何逻辑语句(输入、输出、方法调用、循环、判断等)
#### 2. 代码块的优点
* 代码块相当于另一种形式的构造器(是对构造器的补充机制),可以做初始化的操作
#### 3. 使用场景
* 场景: 多个构造器都有重复语句,可以使用代码块做初始化,提高代码复用性 
#### 4. 注意事项和细节
1. 被static修饰的代码块叫静态代码块(作用: 对类进行初始化)
    1. 静态代码块,随`类的加载`而执行且`只执行一次`
    2. 实例代码块,每`创建`一个`对象`就执行一次
2. 类的加载时机
    1. 创建对象实例时 `new xxx()`
    2. 创建子类对象实例,父类也会加载
    3. 使用类的静态成员时(静态属性、静态方法)
3. 普通代码块,在创建对象实例时,就会被隐式调用;只是使用静态成员时,并不会执行
4. 创建一个对象时,在`一个类`的调用顺序是: `静态 => 实例 => 构造,(多个就看代码先后顺序)`
    1. 调用静态代码块和静态属性初始化(静态代码块和静态属性初始化调用的优先级一样)(如果有多个静态代码块和静态变量初始化,则按代码先后顺序执行)
    2. 调用实例代码块和实例属性的初始化(同上)
    3. 调用构造方法
5. 构造器的最前面其实隐含了super()和调用普通代码块
    - 静态相关的代码块,属性初始化,在类加载时,就执行完毕,因此优先于构造器和实例代码块的执行 
    ```Java
    package codeblock;
    public class CodeBlockDetailExtend {
      public static void main(String[] args) {
        new BBB(); 
        // (1)AAA()无参构造被调用...(2)BBB()实例代码块被执行...(3)BBB()无参构造被调用...
      }
    }
    
    class AAA {
      public AAA() {
        // 1.默认调用super()调用父类无参
        // 2.执行本类的代码块
        System.out.println("AAA()无参构造被调用...");
      }
    }
    
    class BBB extends AAA {
      private String name;
      {
        System.out.println("BBB()实例代码块被执行...");
      }
      public BBB() {
        // 1.默认调用super()调用父类无参
        // 2.执行本类的代码块
        System.out.println("BBB()无参构造被调用...");
      }
    }
    ```
6. 创建子类对象(继承关系),调用顺序:
    1. 父类的静态代码块、静态属性
    2. 子类的静态代码块、静态属性
    3. 父类的实例代码块、实例属性
    4. 父类构造方法
    5. 子类的实例代码块、普通属性初始化
    6. 子类构造方法
7. 静态代码块只能直接调用静态成员,普通代码块可以调用任意成员
- 小结:
    - 1.static代码块是类加载时,执行,只会执行一次
    - 2.普通代码块是在创建对象时调用的,创建一次,调用一次

#### 5. 练习
1. 输出结果是什么
    ```Java
    package codeblock;
    
    public class CodeBlockExercise {
      public static void main(String[] args) {
        System.out.println(Person.total);
        System.out.println(Person.total);
        // 使用类的静态成员时 => 类就会被加载 => 先调静态代码块(只会执行一次) 
        // => 静态相关的代码块,属性初始化,在类加载时,就执行完毕
      }
    }
    
    class Person {
      public static int total;
      static {
        total += 100;
        System.out.println("I'm static block");
      }
    }
    ```
2. 下面的代码输出什么 
    ```Java
    package codeblock;
    
    public class CodeBlockExercise02 {
      public static void main(String[] args) {
        Test a = new Test();  // 实例化Test对象
      }
    }
    
    class Sample {
      Sample(String s) {
        System.out.println(s);
      }
    
      Sample() {
        System.out.println("默认无参构造被调用...");
      }
    }
    
    class Test {
      static Sample sam = new Sample("静态成员 sam 初始化 ");
    
      static {
        System.out.println("static 块执行");//
        if (sam == null) System.out.println("sam is null");
      }
    
      Sample sam1 = new Sample("sam1 成员初始化");
    
      Test() {
        System.out.println("Test 默认构造函数被调用");
      }
    }
    ``` 
### 5. 单例设计模式
#### 1. 什么是单例模式   
* 单例(单个的实例)
    * 类的单例设计模式,就是采取一定方法在整个软件系统中,对某个类只能存在一个对象实例,并且该类只提供一个取得其对象实例的方法
    * 单例模式有两种:
        * 1.饿汉式
        * 2.懒汉式
#### 2. 单例模式[饿汉式]应用实例
1. 构造器私有化 => 防止直接new
2. 类的内部创建对象
3. 向外暴露一个静态的公共方法
```Java
package singleton;

public class HungryHanStyle {
  public static void main(String[] args) {
    girlFriend instance = girlFriend.getInstance();
    System.out.println(instance);//王冰冰
    girlFriend instance1 = girlFriend.getInstance();
    System.out.println(instance1);//王冰冰
    System.out.println(instance.equals(instance1));//T
  }
}

class girlFriend {
  /**
   * 单例模式【饿汉式】
   * steps1.构造器私有化 => 防止直接new
   * steps2.类的内部创建对象
   * steps3.向外暴露一个静态的公共方法
   */
  private static girlFriend gf = new girlFriend("王冰冰");

  private String name;

  private girlFriend(String name) {
    this.name = name;
  }

  public static girlFriend getInstance() {
    return gf;
  }

  @Override
  public String toString() {
    return "girlFriend{" +
        "name='" + name + '\'' +
        '}';
  }
}
```
#### 3. 单例模式[懒汉式]应用实例
```java
package singleton;

public class LazyStyle {
  public static void main(String[] args) {
    Cat instance = Cat.getInstance();
    System.out.println(instance);
    Cat instance1 = Cat.getInstance();
    System.out.println(instance);
    System.out.println(instance.equals(instance1));
  }
}

class Cat {
  private static Cat cat;
  private String name;

  /**
   * 步骤：
   * 1.仍然构造器私有化
   * 2.定义一个 static 静态属性对象
   * 3.提供一个 public 的 static 方法，可以返回一个 Cat对象
   * 4.懶漢式，只有当用戶使用 getInstance 時，才返回cat对象, 后面再次调用时，会返回上次创建的 cat 对象
   */
  private Cat(String name) {
    this.name = name;
  }

  public static Cat getInstance() {
    if (cat == null) {
      cat = new Cat("小花猫");
    }
    return cat;
  }

  @Override
  public String toString() {
    return "小猫姓名：" + name;
  }
}
```

#### 4. 饿汉式 VS 懒汉式

| 比较项 | 饿汉式 | 懒汉式 |
| :-: | :-: | :-: |
| 时机 | 是在类加载就创建了对象实例 | 是在使用时才创建 |
| 线程安全 | 不存在线程安全问题 | 存在线程安全问题 |
| 资源浪费 | 存在资源浪费的可能 | 不存在资源浪费的可能 |

### 6. final 关键字
#### 1. 基本介绍
final(最后的,最终的)可以修饰类、属性、方法和局部变量
#### 2. 使用场景
1. 当不希望类被继承时,可以用final修饰
2. 当不希望父类的某个方法被子类覆盖/重写(override)时,可以使用
3. 当不希望类的某个属性的值被修改,可以使用final修饰
4. 当不希望某个局部变量被修改,可以使用
```java
public class final_pre {
  public static void main(String[] args) {
    A a1 = new A();
    // a1.a = "234";
  }
}

class A {
  // 3.不希望类的某个属性的值被修改
  final public String a = "1234";

  // 2.父类方法不希望被重写/覆盖
  final public void printA() {
    // 4.不希望某个局部变量被修改
    final int n = 1;
    // System.out.println("n = " + (n = 2));
  }
}

// 1.认为是最终类,不希望被继承
final class B extends A {
  // public void printA(){}
}
// class C extends B{}
```
#### 3. 注意事项和细节
1. final修饰的属性又叫常量,一般用XX__XX_XX命名
2. final修饰的属性在定义时,必须赋初始值且不能修改
    1. 定义时,完成赋初始值`public final String WX_CERT= “SYDS4554587”`
    2. 在构造器中
    3. 在代码块中
3. 如果final修饰的属性是静态的,则初始化的位置只能是`1.定义时 2.在静态代码块 不能在构造器中赋值`
4. final类不能继承,但可以实例化对象
5. 如果类不是final类,但含有final方法,则该方法不能重写,但是可以被继承
6. 一般来说,如果一个类已经是final类了,就没必要修饰成final方法.
7. final不能用来修饰构造器
8. final和static往往搭配使用,效率更高,`不会导致类加载`
9. 包装类(Integer,Double,Float,Boolean...),String都是final类

### 7. 抽象类
#### 1. 父类方法的不确定性
当父类的某些方法,需要声明,但是又不确定如何实现时,可以将其声明为抽象方法,那么这个类就是抽象类
```test
父类方法不确定性的问题 
  考虑将该方法设计为抽象(abstract)方法 
  所谓抽象方法就是没有实现的方法 
  所谓没有实现就是指，没有方法体 
  当一个类中存在抽象方法时，需要将该类声明为 abstract 类 
  一般来说，抽象类会被继承，有其子类来实现抽象方法.
```
#### 2. 抽象类的介绍
1. 用abstract修饰的类,这个类就叫抽象类
2. 抽象方法`public abstract void printInfo(String name);//没有抽象体`
3. 抽象类的价值更多是在设计,是设计者设计好后,让子类继承并实现
#### 3. 注意事项和细节
1. 抽象类不能被实例化
2. 抽象类不一定要包含abstract方法. 也就是说,抽象类可以没有abstract方法
3. 一旦类包含了abstract方法,则这个类必须声明为abstract方法
4. abstract只能修饰类和方法,不能修饰属性和其他的
    ```Java
    public class AbstractDetail01 {
      public static void main(String[] args) {
        //抽象类，不能被实例化
        // new A();
      }
    }
    
    // 抽象类不一定要包含 abstract 方法。也就是说,抽象类可以没有 abstract 方法
    // 还可以有实现的方法。
    abstract class A {
      public void hi() {
        System.out.println("hi");
      }
    }
    
    //一旦类包含了 abstract 方法,则这个类必须声明为abstract
    abstract class B {
      public abstract void hi();
    }
    
    //abstract 只能修饰类和方法，不能修饰属性和其它的
    class C {
      // public abstract int n1 = 1;
    }
    ```
5. 抽象类可以有任意成员[`抽象类本质还是类`]
6. 抽象方法不能有主体
7. 如果一个类继承了抽象类,则它必须实现抽象类的所有抽象方法,除非自己也声明为abstract类
8. 抽象方法不能使用private、final和static来修饰,因为这些关键字与重写相违背

#### 4. 抽象模板模式
* 基本介绍
    * 抽象类体现的就是一种模版模式的设计,抽象类作为多个子类的通用模板,子类在抽象类的基础上拓展,但子类总体上会保留抽象类的行为
* 模板设计模式能解决问题
    * 当功能内部一部分实现时确定,一部分实现时不确定的.把不确定的部分暴露出去,让子类实现
    * 编写一个抽象父类,父类提供了多个子类的通用方法,并把一个或多个方法留给其他子类实现,就是一种模板模式 
```Java
package Abstract.Template;

abstract public class Template {
  public abstract void job();

  public void calculateTime() {
    long start = System.currentTimeMillis(); // 开始时间
    job(); // 动态绑定机制
    long end = System.currentTimeMillis();// 结束时间
    System.out.println("任务执行时间 " + (end - start));
  }
}
```
```Java
public class AA extends Template {
  @Override
  public void job() {
    int num = 0;
    for (int i = 1; i <= 90000; i++) {
      num += i;
    }
  }
}
```
```Java
public class BB extends Template {
  @Override
  public void job() {
    int num = 1;
    for (int i = 1; i < 88888; i++) {
      num *= i;
    }
  }
}
```
```Java
public class TemplateTest {
  public static void main(String[] args) {
    new AA().calculateTime();
    new BB().calculateTime();
  }
}
```
### 8. 接口
#### 1. 基本介绍
接口就是给出一些没有实现的方法,封装到一起,到某个类要使用的时候,再根据具体情况把这些方法写出来
```Java
interface 接口名 {
  // 属性
  // 抽象方法
}

class 类名 implements 接口 {
  // 属性、方法
  // 必须实现接口的抽象方法
  // 在接口中可以省略abstract
}
```
小结: 
1. 接口是更抽象的抽象的类,抽象类里的方法可以有方法体,接口里的所有方法都没有方法体`JDK7.0`
2. 接口体现了程序设计的多态和高内聚低耦合的设计思想

* 特别说明:
    * `JDK8.0`后接口类可以有静态方法、默认方法,也就是说接口中可以有方法的具体实现
    * 但是需要使用`defalut`修饰,静态方法需要`static`修饰
#### 2. 实例 
```Java
package interface_;

public interface DBInterface {
  public void connect();
  public void close();
}
```
```Java
public class MysqlDB implements DBInterface {
  @Override
  public void connect() {
    System.out.println("连接Mysql...");
  }

  @Override
  public void close() {
    System.out.println("关闭Mysql连接...");
  }
}
```
```Java
public class OracleDB implements DBInterface {
  @Override
  public void connect() {
    System.out.println("连接Oracle...");
  }

  @Override
  public void close() {
    System.out.println("关闭Oracle连接...");
  }
}
```
```Java
public class TestDB {
  public static void main(String[] args) {
    MysqlDB mysqlDB = new MysqlDB();
    t(mysqlDB);
    OracleDB oracleDB = new OracleDB();
    t(oracleDB);
  }

  public static void t(DBInterface db) {
    db.connect();
    db.close();
  }
}
```
#### 3. 注意事项和细节
1. 接口不能被实例化
2. 接口中所有方法是public方法,接口中抽象方法,可以不用abstract修饰
3. 一个普通类实现接口,就必须将该接口的所有方法都实现
4. 抽象类实现接口,可以不用实现接口的方法
5. 一个类同时可以实现多个接口
6. 接口中的属性,只能是final,默认是`public static final 修饰符`
7. 接口中属性的访问形式: `接口名.属性名`
8. 接口不能继承其它的类,但是可以继承多个别的接口 `interface A extends B,C {}`
9. 接口的修饰符 只能是public和默认

#### 4. 实现接口 vs 继承类
1. 接口和继承解决问题不同
    1. 继承: 解决代码复用性和可维护性
    2. 接口: 设计,设计好各种规范(方法),让其它类去实现这些方法,即更加灵活
2. 接口比继承更加灵活
    * 接口比继承更加灵活,继承是满足is - a的关系,而接口只需满足 like - a的关系
3. 接口在意的程度上实现了代码解耦(即:接口规范性 + 动态绑定机制)
4. 小结:
   1. 当子类继承了父类，就自动的拥有父类的功能
   2. 如果子类需要扩展功能，可以通过实现接口的方式扩展.
   3. 可以理解 实现接口 是 对 java 单继承机制的一种补充.

#### 5. 接口的多态特性
1. 多态参数
    * 既可接收phone对象,又可以接收camera对象,就体现了接口多态(接口引用可以指向实现了接口的类的对象)
    ```Java
    public class InterfacePolyParameter {
      public static void main(String[] args) {
        // 接口的多态体现
        // 接口类型的变量 if01 可以指向 实现了 IF 接口类的对象实例
        IF if01 = new Monster();
        if01 = new Car();
        
        // 继承体现的多态
        // 父类类型的变量 a 可以指向 继承 AAA 的子类的对象实例
        AAA a = new BBB();
        a = new CCC();
      }
    }
    interface IF { }
    class Monster implements IF { }
    class Car implements IF { }
    class AAA { }
    class BBB extends AAA { }
    class CCC extends AAA { }
    ```
2. 多态数组
    ```Java
    public class interfacePloyArr {
      public static void main(String[] args) {
        //多态数组 => 接口类型数组
        Usb[] usbs = new Usb[2];
        // 向上转型 => 多态
        usbs[0] = new Phone();
        usbs[1] = new Camera();
        for (int i = 0; i < usbs.length; i++) {
          usbs[i].work();
          if (usbs[i] instanceof Phone){
            // 向下转型 => 使用特定方法
            ((Phone) usbs[i]).call();
          }
        }
      }
    }
    
    interface Usb {
      void work();
    }
    
    class Phone implements Usb {
      public void call() {
        System.out.println("手机可以打电话...");
      }
    
      @Override
      public void work() {
        System.out.println("手机正在工作...");
      }
    }
    
    class Camera implements Usb {
    
      @Override
      public void work() {
        System.out.println("相机正在工作...");
      }
    }
    ```
3. 接口存在多态传递性
    ```Java
    package interface_;
    
    public class InterfacePloyPass { }
    
    interface IH {
      void hi();
    }
    
    interface IG extends IH { }
    
    // 类 "Test" 必须声明为抽象，或为实现 "IH" 中的抽象方法 "hi()"
    class Test implements IG {
      @Override
      public void hi() { }
    }
    ```
#### 6. 练习
```Java
// 代码有没有错误并修改
interface A {
  int x = 0;
}

class B {
  int x = 1;
}

class C extends B implements A {
  public void pX() {
    System.out.println(x);
  }

  public static void main(String[] args) {
    new C().pX();
  }
}
```
```Java
interface A {
  int x = 0;
}

class B {
  int x = 1;
}

class C extends B implements A {
  public void pX() {
    // x 指意不明
    // 访问接口的 x 就使用 A.x 
    // 访问父类的 x 就使用 super.x
    System.out.println("接口A的x = "+A.x + "\nClassB的x = "+super.x);
  }

  public static void main(String[] args) {
    new C().pX();
  }
}
```
### 9. 内部类
#### 1. 基本介绍
- 一个类的内部有`完整的嵌套`另一个类结构
    - 被嵌套的类称为内部类(inner class),嵌套其它的类叫外部类(outer class)
    - 是类的第五大成员[`属性、方法、构造器、代码块、内部类`]
    - 最大特点: 可以直接访问私有属性,并且可以体现类与类之间的包含关系

#### 2. 基本语法  
```Java
// 外部类
class Outer { 
  // 内部类
  class Inner { }
}
// 外部其他类
class Other { } 
```   
#### 3. 内部类的分类
- 如果定义类在外部类局部位置(方法中/代码块)上:
    - (1) 局部内部类 (有类名)
    - (2) 匿名内部类 (没有类名)
- 定义在外部类的成员位置上: 
    - (1) 成员内部类 (没用static修饰)
    - (2) 静态内部类 (使用static修饰)
#### 4. 局部内部类的使用
- 记住: 
    1. 局部内部类是定义在外部类的局部位置 [比如: `方法中/代码块`],并且有类名
    2. 作用域在方法体或者代码中
    3. 本质依然是一个类
1. 可以直接访问外部类的所有成员,包含私有的
2. 不能添加访问修饰,因为它本身就是一个局部变量
    - 局部变量是不能使用修饰符的,但可以使用final
3. 作用域: 仅仅在定义它的方法或代码块中
4. 局部作用域 `=访问=>` 外部类的成员 [`访问方式: 直接访问`]
5. 外部类 `=>` 局部内部类的成员 [`访问方式: 先创建对象,再访问(必须在作用域内)`]
6. 外部其他类 `=x>` 局部内部类(因为: 局部内部类本身就是一个局部变量)
7. 如果外部类和局部内部类的成员重名时,默认遵循就近原则; 
    * 如果想访问外部类的成员,则可以使用[`外部类名.this.成员`]去访问
    ```java
      public static void main(String[] args) {
        outer_local_classes outer = new outer_local_classes();
        outer.m1();
        // 6. 外部其他类 ==不能访问=> 局部内部类
        // (x) localInner localInner = new localInner();
      }
    }
    
    class outer_local_classes {
      private int n1 = 9; // 私有属性
    
      private void m2() {
        System.out.println("Outer私有方法m2()");
      }
    
      public void m1() {
        // 1. 可以直接访问外部类的所有成员
        System.out.println(n1);
        // 2.不能添加修饰符，但是可以使用final
        // 3.作用域：仅仅在定义它的方法或代码块中
        final class localInner { // 内部类本身就是一个局部变量
          private final String name = "fishx";
          private int n1 = 12;
    
          public void f1() {
            // 4.局部作用域=可以直接访问=>外部成员
            m2();
            // 7. 如果外部类和局部内部类成员重名 => 遵循就近原则
            System.out.println(n1);
            // 7.1 如果要访问外部类的成员就使用 外部类名.this.成员
            System.out.println(localInner.this.n1);
          }
        }
        // 5.外部类=创建访问=>内部类成员
        localInner localInner = new localInner();
        localInner.f1();
        System.out.println(localInner.name);
      }
    }
    ```
#### 5. 匿名内部类
* 匿名内部类:
    * 本质还是类
    * 内部类
    * 该类没有类名(系统分配,不能使用)
##### 1. 匿名内部类的使用
基于接口匿名的内部类
```java
// 匿名内部类,可以理解为快照
interface IA { // 接口
  public void cry();
}

/**
 * 演示，匿名内部类的使用
 */
public class AnonymousInnerClass {
  public static void main(String[] args) {
    Outer outer = new Outer();
    outer.method();
  }
}

class Outer { // 外部类
  private int n1 = 10; // 私有属性

  public void method() { // 私有方法
    // 基于接口匿名的内部类
    // 1. 解决：仅使用一次的类实现接口 => 浪费内存 => 匿名内部类简化开发
    // 2. tiger的编译类型: IA ; tiger的运行类型:匿名内部类(Outer$1)
    // 3. jdk 底层在创建匿名内部类 Outer$1,立即马上就创建了 Outer$1 实例，并且把地址,返回给 tiger
    // 4. 匿名内部类使用一次，就不再使用
    IA tiger = new IA() {
      @Override
      public void cry() {
        System.out.println("老虎叫...");
      }
    };
    System.out.println("tiger的运行类型：" + tiger.getClass());
    tiger.cry();
  }
}

/*
 class Tiger implements IA {
   @Override
   public void cry() {
     System.out.println("老虎叫...");
   }
 }
*/
``` 
基于类的匿名内部类
```java
/**
 * 演示，匿名内部类的使用
 */
public class AnonymousInnerClass {
  public static void main(String[] args) {
    Outer outer = new Outer();
    outer.method();
  }
}

class Outer { // 外部类
  private int n1 = 10; // 私有属性

  public void method() { // 私有方法
    // 基于类的匿名内部类
    Father father = new Father("Jack") {
      @Override
      public void test() {
        System.out.println(name + "重写test()");
      }
    };
    father.test();

    // 基于抽象类的匿名内部类
    Animal animal = new Animal() {
      // 构造器类名缺失,不能重写构造器
      @Override
      void eat() {
        System.out.println("小狗吃骨头...");
      }
    };
    animal.eat();
  }
}

class Father {
  public String name;

  public Father(String name) {
    this.name = name;
  }

  public void test() {
  }
}

abstract class Animal {
  abstract void eat();
}
```
##### 2. 注意事项和细节
1. 匿名内部类既是一个类的定义,同时它本身也是一个对象 
    * [`从语法上看: 它既有定义类的特征,也有创建对象的特征`],因此可以调用匿名内部类方法
2. 可以直接访问外部类的所有成员,包含私有的
3. 作用域: 仅在定义它的方法或代码块中
4. 匿名内部类 `直接访问` 外部类成员
5. 外部其他类 `不能访问` 匿名内部类
6. 如果外部类和局部内部类的成员重名时,默认遵循就近原则; 
    * 如果想访问外部类的成员,则可以使用[`外部类名.this.成员`]去访问
##### 3. 最佳实践
###### 当作实参直接传递,简洁高效
```Java
// 接口
interface JK {
  void show();
}

public class InnerClassExercise01 {
  public static void main(String[] args) {
    f1(new JK() {
      @Override
      public void show() {
        System.out.println("当作实参直接传递,简洁高效");
      }
    });
  }
  public static void f1(JK jk) {
    jk.show();
  }
}
```
##### 4. 练习
```text
1. Bell接口,方法ring
2. Cellphone类,方法alarmClock(参数是Bell类型)
3. 测试手机类闹钟的功能,通过匿名内部类(对象)作为参数,打印:“起床了,懒猪”
4. 再传另一个匿名内部类(对象),打印:“小伙伴上课了”
```
```Java
interface Bell {
  void ring();
}

public class InnerClassExercise02 {
  public static void main(String[] args) {
    CellPhone cellPhone = new CellPhone();
    cellPhone.alarmClock(new Bell() {
      @Override
      public void ring() {
        System.out.println("懒猪起床了～");
      }
    });
    cellPhone.alarmClock(new Bell() {
      @Override
      public void ring() {
        System.out.println("小伙伴们上课了！");
      }
    });
  }
}

class CellPhone {
  public void alarmClock(Bell bell) {
    bell.ring();
  }
}
```
#### 6. 成员内部类
成员内部类: `定义在外部类`的成员位置,并且`没有static修饰`
1. 可以直接访问外部类的所有成员
2. 可以添加任意访问修饰符,因为它本身就是一个成员
3. 作用域: 和外部类的其他成员一样,为整个类体
4. 成员内部 `直接访问` 外部类成员
5. 外部类 `创建后访问` 成员内部类
6. 外部其他类 `可以访问` 成员内部类
7. 如果外部类和内部类的成员重名时,内部类访问默认遵循就近原则
    * 如果想访问外部类的成员,则可以使用 `外部类名.this.成员` 去访问
```java
public class MemberInnerClass { // 外部其他类
  public static void main(String[] args) {
    Outer08 outer08 = new Outer08();
    outer08.t1();
    // 6. 外部其他类 可以访问 成员内部类(2种)
    // 第一种：
    // outer08.new Inner08();相当于把new Inner08()当作是outer08成员
    Outer08.Inner08 inner08 = outer08.new Inner08();
    inner08.f1();
    // 第二种： 外部类中，编写一个方法，可以返回Inner08对象
    outer08.getInner08Instance().f1();
  }
}

class Outer08 { // 外部类
  public String name = "张三";
  private double salary = 9.9;
  private int n1 = 8;

  public void t1() { // 外部类方法
    // 5. 外部类 创建后访问 成员内部类
    // 使用成员内部类的对象，最后使用相关方法
    Inner08 inner08 = new Inner08();
    inner08.f1();
    System.out.println(inner08.salary);
  }

  // 2. 可以添加任意访问修饰符,因为它本身就是一个成员
  class Inner08 { // 成员内部类
    public int salary = 9;
    // 3.作用域: 整个(外部类)类体
    // 4. 成员内部 直接访问 外部类成员
    public void f1() {
      // 1. 可以直接访问外部类的所有成员
      // 7.如果外部类和内部类的成员重名时,内部类访问默认遵循就近原则
      System.out.println("n1 = " + (n1 = 10) + "\t" +
          "成员内部类salary =" + salary 
          + "\t外部类 = " + Outer08.this.salary);
    }
  }
  // 该方法，返回一个Inner08实例
  public Inner08 getInner08Instance() {
    return new Inner08();
  }
}
``` 

#### 7. 静态内部类 
静态内部类是`定义在外部类`的成员位置,并`有static修饰`
1. 可以直接访问外部类的`所有静态成员`,但不能直接访问非静态成员
2. 可以添加任意访问修饰符,它本身就是一个成员
3. 作用域: 同其他的成员,为整个类体
4. 静态内部类 `直接访问` 外部类
5. 外部类 `创建对象后访问` 静态内部类
6. 外部其他类 `可以访问` 静态内部类
7. 如果外部类和静态内部类的成员重名时,静态内部类访问默认遵循就近原则
    * 如果想访问外部类的成员,则可以使用 `外部类名.成员` 去访问
```java
public class staticInnerClass { // 外部其他类
  public static void main(String[] args) {
    Outer09 outer09 = new Outer09();
    outer09.m1();
    // 6. 外部其他类 可以访问 静态内部类(有两种)
    // 静态(类)变量可以通过(对象名.类变量名)来访问
     Outer09.Inner09 inner09 = new Outer09.Inner09();
     inner09.say();
     // 第二种
    Outer09.Inner09 inner091 = Outer09.getInner09();
    inner091.say();
  }
}

class Outer09 { // 外部类
  private static String name = "张三";
  private int n = 10;

  // 静态内部类，处于外部类的成员位置，被static所修饰
  // 1. 可以直接访问外部类的`所有静态成员`,但不能直接访问非静态成员
  // 2. 可以添加任意访问修饰符,它本身就是一个成员
  // 3. 作用域: 同其他的成员,为整个类体
  // 4. 静态内部类 直接访问 外部类
  public static class Inner09 { // 静态内部类
    private static String name = "fishx";
    
    public void say() {
      // 无法从 static 上下文引用非 static 字段 'n'
      // System.out.println(n);
      // 7.如果外部类和静态内部类的成员重名时,静态内部类访问默认遵循就近原则
      System.out.println("静态内部类: "+name+" 外部类: "+Outer09.name);
    }
  }
  // 5.外部类 创建对象后访问 静态内部类
  public void m1(){
    Inner09 inner09 = new Inner09();
    inner09.say();
  }
  public static Inner09 getInner09() {
    return new Inner09();
  }
}
``` 
#### 8. 练习
##### 1. Car执行结果
```java
public class homework01 {
  public static void main(String[] args) {
    Car car = new Car();
    Car car1 = new Car(100);
    System.out.println(car); // 9.0 red
    System.out.println(car1); // 100.0 red
    // color是被静态修饰的，是所有对象所共享的
  }
}

class Car {
  static String color = "white";
  double price = 10.0;

  public Car() {
    this.price = 9;
    color = "red";
  }

  public Car(double price) {
    this.price = price;
  }

  public String toString() {
    return price + "\t" + color;
  }
}
```
##### 2. 编程题:Frock类
```text
1. 在Frock类中声明私有的静态属性currentNum[int类型]，初始值为100000，作为衣服出
厂的序列号起始值。
2.声明公有的静态方法getNextNum, 作为生成上衣唯-序列号的方法。每调用一次，将
currentNum增加100,并作为返回值。
3.在TestFrock类的main方法中，分两次调用getNextNum方法，获取序列号并打印输出。
4.在Frock类中声明serialNumber(序列号)属性，并提供对应的get方法;
5.在Frock类的构造器中， 通过调用getNextNum方法为Frock对象获取唯一 序列号， 赋给
serialNumber属性。
6.在TestFrock类的main方法中， 分别创建三个Frock对象，并打印三个对象的序列号，验
证是否为按100递增。
```
##### 3. 编程题: 抽象类的使用
```text
=> 抽象类的使用
1.动物类Animal包含了抽象方法shout();
2. Cat类继承 了Animal,并实现方法shout,打印“猫会喵喵叫”
3. Dog类继承了Animal,并实现方法shout,打印"狗会汪汪叫”
4.在测试类中实例化对象Animal cat =new Cat(),并调用cat的shout方法
5.在测试类中实例化对象Animal dog=new Dog),并调用dog的shout方法
```
##### 4. 编程题: 匿名内部类的使用
```text
=> 匿名内部类的使用
1.计算器接口具有work方法，功能是运算，有一个手机类Cellphone,定义方法testWork
测试计算功能，调用计算接口的work方法，
2. 要求调用CellPhone对象的testWork方法，使用上匿名内部类
```
```java
// 接口就是给出一些没有实现的方法,封装到一起,到某个类要使用的时候,再根据具体情况把这些方法写出来
interface computer {
  public double work(double n1, double n2);
  // 至于方法如何实现，交给匿名内部类实现
}

public class homework04 {
  public static void main(String[] args) {
    Cellphone cellphone = new Cellphone();
    // 匿名内部类：定义类在外部类局部位置
    // 匿名内部类,当作实参直接传递,简洁高效
    // 匿名内部类可以虚拟实例化抽象类对象并作为参数传递
    cellphone.testWork(new computer() {
      @Override
      public double work(double n1, double n2) {
        return n1 * n2; //具体计算方法
      }
    }, 12, 3);
  }
}

class Cellphone {
  // why ？为什么要用参数传递 => 因为要调用computer接口中未实现的方法
  // => 匿名内部类的使用(最佳实践：当作参数传递)
  public void testWork(computer computer, double n1, double n2) {
    double result = computer.work(n1, n2);
    System.out.println("计算结果为：" + result);
  }
}
```
##### 5. 编程题: 内部类
```text 
1. 编一个类A，在类中定义局部内部类B，B中有一个私有final常量name,有一个方法
    show()打印常量name。进行测试
2. 进阶: A中也定义一个私有的变量name,在show方法中打印测试
```
```java
public class homework05 {
  public static void main(String[] args) {
    new A().f1();
  }
}

class A { // 外部类
  private String hello = "hello";

  public void f1() {
    // 局部内部类 (有类名): 定义类在外部类(方法中/代码块)上
    class B {
      private final String NAME = "fishx";

      public void show() {
        // 可直接使用外部类成员，重名 ？=> 就近(外部类.this.属性来指定)
        System.out.println(hello + "! name = " + NAME);
      }
    }
    B b = new B();
    b.show();
  }
}
```
##### 6. 编程题: 综合
```text
1.有一个交通工具接口类Vehicles,有work接口
2.有Horse类和Boat类分别实现Vehicles
3.创建交通工具工厂类，有两个方法分别获得交通工具Horse和Boat
4.有Person类， 有name和Vehicles属性， 在构造器中为两个属性赋值
5.实例化Person对象"唐僧”,要求一般情况下用Horse作为交通工具，遇到大河时用Boat
作为交通工具
```
```java
interface Vehicles {
  public void work();
}

public class homework06 {
  public static void main(String[] args) {
    Person tang = new Person("唐僧", new Horse());
    tang.passRiver();
    tang.common();
    tang.passFireHill();
  }
}

class Horse implements Vehicles {
  @Override
  public void work() {
    System.out.println("一般情况下，使用马儿前进...");
  }
}

class Boat implements Vehicles {
  @Override
  public void work() {
    System.out.println("走水路，要使用船前进...");
  }
}

class Plane implements Vehicles {
  @Override
  public void work() {
    System.out.println("坐飞机过火焰山... ");
  }
}

class VehiclesFactory {
  // 创建交通工具工厂类，有两个方法分别获得交通工具Horse和Boat
  private static Horse horse = new Horse();

  public static Horse getHorse() {
    return horse;
  }

  public static Boat getBoat() {
    return new Boat();
  }

  public static Plane getPlane() {
    return new Plane();
  }
}

class Person {
  private String name;
  private Vehicles vehicles;

  public Person(String name, Vehicles vehicles) {
    this.name = name;
    this.vehicles = vehicles;
  }

  public void passRiver() {
    if (!(vehicles instanceof Boat)) {
      vehicles = VehiclesFactory.getBoat();
    }
    vehicles.work();
  }

  public void passFireHill() {
    if (!(vehicles instanceof Plane)) {
      vehicles = VehiclesFactory.getPlane();
    }
    vehicles.work();
  }

  public void common() {
    if (!(vehicles instanceof Horse)) {
      vehicles = VehiclesFactory.getHorse();
    }
    vehicles.work();
  }
}
```
##### 7. 编程题: 内部类练习
```text
有一个Car类，有属性temperature (温度) ,车内有Air (空调)类，有吹风的功能flow,
Air会监视车内的温度，如果温度超过40度则吹冷气。如果温度低于0度则吹暖气，如果在这之
间则关掉空调。实例化具有不同温度的Car对象，调用空调的flow方法，测试空调吹的风是否
正确. //体现类与类的包含关系的案例类(内部类[成员内部类] )
```
## 十、枚举和注解
### 1. 枚举
#### 1. 楔子
* 先看一个需求: 要求创建季节(Season) 对象，请设计并完成
    ```Java
    public class Enumeration01 {
      public static void main(String[] args) {
        SeasonWedeg spring = new SeasonWedeg("春天", "温暖");
        // 季节是固定的4个，有限值且只读 => 不能让用户创建 => 枚举
        SeasonWedeg winter = new SeasonWedeg("华天", "～～");//（x）
      }
    }
    
    class SeasonWedeg {//类
      private String name;
      private String desc;
    
      public SeasonWedeg(String name, String desc) {
        this.name = name;
        this.desc = desc;
      }
    
      public String getName() {
        return name;
      }
    
      public void setName(String name) {
        this.name = name;
      }
    
      public String getDesc() {
        return desc;
      }
    
      public void setDesc(String desc) {
        this.desc = desc;
      }
    }
    ```
* 自定义枚举
    ```Java
    public class CustomEnum {
      public static void main(String[] args) {
        System.out.println(SeasonCustom.SPRING);
      }
    }
    
    class SeasonCustom {
      // 3. 在Season内部，直接创建固定的对象
      public static final SeasonCustom SPRING = new SeasonCustom("春天", "温暖");
      public static final SeasonCustom SUMMER = new SeasonCustom("夏天", "炎热");
      public static final SeasonCustom AUTUMN = new SeasonCustom("秋天", "凉爽");
      public static final SeasonCustom WINTER = new SeasonCustom("冬天", "寒冷");
      private String name;
      private String desc;
    
      // 1.将构造器私有化，防止直接被new
      private SeasonCustom(String name, String desc) {
        this.name = name;
        this.desc = desc;
      }
    
      // 2. 去掉setXXX，防止被修改
      public String getName() {
        return name;
      }
    
      public String getDesc() {
        return desc;
      }
    
      @Override
      public String toString() {
        return "SeasonCustom{" +
            "name='" + name + '\'' +
            ", desc='" + desc + '\'' +
            '}';
      }
    }
    ```
* 枚举关键字
    ```Java
    enum SeasonKeyword {
      // 使用关键字 enum 替代 class，且要求将定义常量对象写到前面
      // 3. 在Season内部，直接创建固定的对象
      // public static final Season SPRING = new Season("春天", "温暖");
      // 等价于 SPRING("春天", "温暖")
      SPRING("春天", "温暖"), SUMMER("夏天", "炎热"),
      AUTUMN("秋天", "凉爽"), WINTER("冬天", "寒冷");
    
      private String name;
      private String desc;
    
    
      // 1.将构造器私有化，防止直接被new
      private SeasonKeyword(String name, String desc) {
        this.name = name;
        this.desc = desc;
      }
    
      // 2. 去掉setXXX，防止被修改
      public String getName() {
        return name;
      }
    
      public String getDesc() {
        return desc;
      }
    
      @Override
      public String toString() {
        return "SeasonKeyword{" +
            "name='" + name + '\'' +
            ", desc='" + desc + '\'' +
            '}';
      }
    }
    
    public class keywordEnum {
      public static void main(String[] args) {
        System.out.println(SeasonKeyword.SPRING);
      }
    }
    ```
#### 2. 基本介绍
1. 枚举是一组常量的集合。
2. 可以这里理解：枚举属于一种特殊的类，里面只包含一组有限的特定的对象。
3. 枚举的二种实现方式:
    1. 自定义类实现枚举
        ```text
        1. 不需要提供setXxx方法,因为枚举对象值通常只读
        2. 对枚举对象/属性使用 final + static共同修饰,实现底层优化
        3. 枚举对象名通常使用全部大写,常量的命名规范
        4. 枚举对象根据需要,也可以有多个属性
        5. 小结:
            1. 构造器私有化
            2. 本类内部创建一组对象
            3. 对外暴露对象(通过为对象添加修饰符public final static)
            4. 可以提供getXxx方法,但不要提供setXxx方法
        ```
    2. 使用 enum 关键字实现枚举
        ```text
        1. 同上
        2. 使用关键字 enum 替代 class
        3. 如果有多个常量(对象)， 使用 ,号间隔即可
        4. 如果使用 enum 来实现枚举，要求将定义常量对象，写在前面
        5. 如果我们使用的是无参构造器，创建常量对象，则可以省略 ()
        6. 小结:
            1. 使用关键字enum开发一个枚举类时,默认会继承final的Enum类
            2. 如果使用无参构造器 创建 枚举对象，则实参列表和小括号都可以省略
            3. 当有多个枚举对象时，使用,间隔，最后有一个分号结尾
            4. 枚举对象必须放在枚举类的行首.
        ```
#### 3. enum 常用方法
##### 说明：
使用关键字 enum 时，会隐式继承 Enum 类, 这样我们就可以使用 Enum 类相关的方法。
```Java
// Enum源码
public abstract class Enum<E extends Enum<E>> 
  implements Comparable<E>, Serializable {
}
```
##### 常用方法应用实例
```Java
enum method {
  // 使用关键字 enum 替代 class，且要求将定义常量对象写到前面
  // 3. 在Season内部，直接创建固定的对象
  // public static final Season SPRING = new Season("春天", "温暖");
  // 等价于 SPRING("春天", "温暖")
  SPRING("春天", "温暖"), SUMMER("夏天", "炎热"),
  AUTUMN("秋天", "凉爽"), WINTER("冬天", "寒冷");

  private final String name;
  private final String desc;


  // 1.将构造器私有化，防止直接被new
  method(String name, String desc) {
    this.name = name;
    this.desc = desc;
  }

  // 2. 去掉setXXX，防止被修改
  public String getName() {
    return name;
  }

  public String getDesc() {
    return desc;
  }
}
```
```Java
public class enumMethod {
  public static void main(String[] args) {
    method spring = method.SPRING;
    // 1. name() 返回这个枚举常数的名称(建议优先选择toString)
    System.out.println(spring.name());
    // 2. toString()
    System.out.println(spring);
    // 3. ordinal() 得到当前枚举常量的次序(从0开始)
    System.out.println(method.WINTER.ordinal());
    // 4. 反编译可见values()，返回method[]
    method[] values = method.values();
    System.out.println("---- 遍历取出枚举对象 ----");
    for (method value : values) {
      System.out.println(value);
    }
    // 5.valueOf() 返回与参数匹配的枚举常量
    method spring1 = method.valueOf("SPRING");
    System.out.println("spring1 = " + spring1);
    System.out.println(spring == spring1);
    // 6.compareTo：比较两个枚举常量，比较的就是编号！
    // method.WINTER(编号4) - method.SPRING(编号1) = 3
    System.out.println(method.WINTER.compareTo(method.SPRING));
  }
}
```
#### 4. enum 实现接口
1. 使用 enum 关键字后，就不能再继承其它类了，(因为enum会隐式继承 Enum,而 Java是单继承机制)。
2. 枚举类和普通类一样，可以实现接口，如下形式。 
    ```java
    enum 类名 implements 接口 1， 接口 2{}
    
    enum Music implements IPlaying {
      CLASSICMUSIC; // 枚举常量
    
      @Override
      public void playing() {
        System.out.println("好听的音乐～");
      }
    }
    
    interface IPlaying {
      public void playing();
    }
    
    public class EnumDetail {
      public static void main(String[] args) {
        Music.CLASSICMUSIC.playing();
      }
    }
    ```
#### 5. 练习
* 下面代码是否正确
    ```Java
    enum Gender {
      // 默认调用Gender类无参构造器
      BOY,GIRL;// 实参列表和小括号可以省略      
    }
    ```
* 下面代码是否正确
    ```java
    enum Gender {
      BOY,GIRL;
    }
    Gender boy = Gender.BOY; // √
    Gender boy2 = Gender.BOY; // √
    // 调用了enum的toString()方法(返回这个枚举常数的名称)
    System.out.printIn(boy); // 输出BOY
    System.out.printIn(boy2 == boy); // √
    ```
* 声明 Week 枚举类，其中包含星期一至星期日的定义；使用 values返回所有的枚举数组
    ```java
    enum Week {
      MONDAY("星期一"), TUESDAY("星期二"), WEDNESDAY("星期三"),
      THURSDAY("星期四"), FRIDAY("星期五"), SATURDAY("星期六"),
      SUNDAY("星期日");
      
      private final String name;
    
      private Week(String name) {
        this.name = name;
      }
    
      public String getName() {
        return name;
      }
    
      @Override
      public String toString() {
        return name;
      }
    }
    ```
    ```java
    public class weekTest {
      public static void main(String[] args) {
        Week[] values = Week.values();
        System.out.println("====所有星期信息====");
        for (Week value : values) {
          System.out.println(value);
        }
      }
    }
    ```
### 2. 注解
1. 注解(Annotation)也被称为元数据(Metadata)，用于修饰解释 包、类、方法、属性、构造器、局部变量等数据信息。
2. 和注释一样，注解不影响程序逻辑，但注解可以被编译或运行，相当于嵌入在代码中的补充信息。
3. 在 JavaSE 中，注解的使用目的比较简单，例如标记过时的功能，忽略警告等。在 JavaEE 中注解占据了更重要的角 色，例如用来配置应用程序的任何切面，代替 java EE 旧版中所遗留的繁冗代码和 XML 配置等。
#### 1. 基本的 Annotation 介绍  
使用 Annotation 时要在其前面增加 @ 符号, 并把该 Annotation当成一个修饰符使用。用于修饰它支持的程序元素
* 三个基本的 Annotation:
    1. @Override: 限定某个方法，是重写父类方法, 该注解只能用于方法
        ```text
        1. @Override表示指定重写父类方法(从编译层面验证),如果父类没有fly方法,则会报错
        2. 如果不写@Override一样构成重写,但在编译阶段会检测
        3. @Override只能修饰方法,不能修饰其他类、包、属性等等
        4. 查看@Override注解源码为@Target(ElementType.METHOD)说明只能修饰方法
        5. @Target是修饰注解的注解,称为元注解
        ```
    2. @Deprecated: 用于表示某个程序元素(类, 方法等)已过时
        ```text
        1. 用于表示某个程序元素(类、方法等)已过时
        2. 可以修饰方法、类、字段、包、参数等等
        3. @Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, 
            METHOD, PACKAGE, PARAMETER, TYPE})
            public @interface Deprecated { }
        4. @Deprecated的作用可以做到新旧版本的兼容和过渡
        ```
    3. @SuppressWarnings: 抑制编译器警告
        ```text
        1. unchecked是忽略没有检查的警告
        2. rawtypes是忽略没有指定泛型的警告(传参时没有指定泛型的警告错误)
        3. unused是忽略没有使用某个变量的警告错误
        4. @SuppressWarnings可以修饰程序元素为,查看@Target
        5. 生成@SupperssWarnings时,直接点提示,不用背
        ```
#### 2. JDK 的元 Annotation(元注解)
##### 元注解的基本介绍
JDK 的元 Annotation 用于修饰其他 Annotation
##### 元注解的种类
1. Retention //指定注解的作用范围，三种 SOURCE,CLASS,RUNTIME
    1. 只能用于修饰一个 Annotation 定义, 用于指定该 Annotation 可以保留多长时间
    2. @Retention 的三种值:
        1. RetentionPolicy.SOURCE: 编译器使用后，直接丢弃这种策略的注释
        2. RetentionPolicy.CLASS:编译器将把注解记录在 class 文件中.当运行 Java 程序时, JVM 不会保留注解
        3. RetentionPolicy.RUNTIME:编译器将把注解记录在 class 文件中. 当运行 Java 程序时, JVM 会保留注解,程序可以通过反射获取该注解
2. Target // 指定注解可以在哪些地方使用
3. Documented //指定该注解是否会在 javadoc 体现
    1. 用于指定被该元注解修饰的Annotation类将被javadoc工具提取成文档
    2. 定义此注解必须设置Retention值为RUNTIME
4. Inherited //子类会继承父类注解
#### 3. 练习
##### 1. 编程题: 枚举类
```text
1.创建一个Color枚举类
2.有RED,BLUE,BLACK,YELLOW,GREEN这个五个枚举值/对象; 
3. Color有三个属性redValue, greenValue, blueValue,
4.创建构造方法，参数包括这三个属性，
5.每个枚举值都要给这三个属性赋值，三个属性对应的值分别是
6. red: 255,0,0 blue:0,0,255 black:0,0,0 yellow:255, 255,0 green:0,255,0
7.定义接口，里面有方法show,要求Color实现该接口
8. show方法中显示三属性的值
9.将枚举对象在switch语句中匹配使用
```
## 十一、异常-Exception
* 对异常进行捕获，从而保证程序的健壮性
### 1. 异常介绍
- 将程序执行中发生的不正常情况称为“ 异常 ”
    1. Error(错误): Java虚拟机无法解决的严重问题(如JVM系统内部错误等),是严重问题程序会崩溃
    2. Exception: 其他因编程错误或偶然的外在因素导致一般性问题,可以使用针对性的代码处理
        1. 运行时异常[程序运行时,发生异常]
        2. 编译时异常[编程时,发生异常]

### 2. 异常体系图一览
![](media/16629433472925/16643614914646.jpg)
#### 异常体系图的小结:
1. 异常分为两大类: 运行时、编译时
2. 运行时异常,编译器查不出.一般是指编程时逻辑错误,java.lang.RuntimeException类及它的子类都是运行时异常
3. 运行时异常很常见可以不做处理,若全处理可能会影响程序的可读性
4. 编译时异常,是编译器要求必须处理的异常

### 3. 常见的运行时异常
1. NullPointerException 空指针异常
2. ArithmeticException 数学运算异常
3. ArrayIndexOutOfBoundsException 数组下标越界异常
4. ClassCastException 类型转换异常
5. NumberFormatException 数字格式不正确异常

### 4. 编译异常
* 编译异常是指在编译阶段必须处理的异常,否则代码不给通过
    ```text
    SQLException //操作数据库时，查询表可能发生异常
    IOException //操作文件时，发生的异常
    FileNotFoundException //当操作一个不存在的文件时，发生异常
    ClassNotFoundException //加载类，而该类不存在时，异常
    EOFException //操作文件，到文件末尾，发生异常
    IllegalArguementException //参数异常
    ```
### 5. 异常处理
1. try-catch-finally 捕获错误,自行处理
    1. `try-catch-finally` 处理机制
    ![](media/16629433472925/16643643017019.jpg)
    2. try-catch 方式处理异常-注意事项
        1. 发生异常 `=>` 异常之后的代码不会执行 `=>` 直接进入catch块
        2. 没有发生异常 `=>` 执行完try,不进入catch
        3. 不管发没发生异常 `=>` 都将进入finally块
        4. 可以有多个catch捕获不同异常,要求父类在前子类异常在后
    3. 实践
        ```Java
        import java.util.Scanner;
        
        // 如果用户输入的不是一个整数，就提示他反复输入，直到输入一个整数为止
        public class tryCatch {
          /**
           * 思路
           * 1. 创建 Scanner 对象
           * 2. 使用无限循环，去接收一个输入
           * 3. 然后将该输入的值，转成一个 int
           * 4. 如果在转换时，抛出异常，说明输入的内容不是一个可以转成 int 的内容
           * 5. 如果没有抛出异常，则 break 该循环
           */
          public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);
            String inputStr = "";
            int num = 0;
            while (true) {
              System.out.print("请输入一个整数：");
              inputStr = scanner.next();
              try {
                num = Integer.parseInt(inputStr);
                break;
              } catch (NumberFormatException e) {
                System.out.println("你输入的不是一个整数:");
              }
            }
            System.out.println("你输入的值是 = " + num);
          }
        }    
        ```

2. throws 将异常抛出,交给方法调用者来处理,最顶层的处理者是JVM
    1. `throws`处理机制
    ![](media/16629433472925/16643643124192.jpg)
    2. `throws`使用注意事项
        ```text
        1. 对于编译异常，程序中必须处理，比如try-catch或者throws
        2. 对于运行时异常，程序中如果没有处理，默认就是throws的方式处理
        3. 子类重写父类的方法时，对抛出异常的规定:子类重写的方法，所抛出的异常类型要
        么和父类抛出的异常致，要么为父类抛出的异常的类型的子类型
        4. 在throws过程中，如果有方法try-catch，就相当于处理异常，就可以不必throws
        ```
3. 自定义异常      
    ```Java
    public class customException {
      public static void main(String[] args) {
        int age = 1;
        if (!(age >= 18 && age <= 120)) {
          // 通过构造器自定义异常信息
          throw new AgeException("年龄需要在18～120之间");
        }
        System.out.println("年龄范围正确");
      }
    }
    
    // 自定义异常
    // 一般情况默认为 RuntimeException 优点在于可以使用默认处理机制
    class AgeException extends RuntimeException {
      public AgeException(String message) {
        super(message);
      }
    }
    ```
### 6. 练习
#### 1. 编写应用程序EcmDef
```text
a. 编写应用程序EcmDef.java,接收命令行的两个参数(整数)，计算两数相除。
b. 计算两个数相除，要求使用方法cal(int n1, int n2)
c. 对数据格式不正确(NumberformatException)、 缺少命令行参数(ArrayIndexOutOfBoundsException)、
    除0进行异常处理(ArithmeticException)。
```
```Java
public class Homework01 {
  public static void main(String[] args) {
    try {
      if (args.length != 2) {
        throw new ArrayIndexOutOfBoundsException("参数个数不对");
      }
      int n1 = Integer.parseInt(args[0]);
      int n2 = Integer.parseInt(args[1]);
      double res = cal(n1, n2);
      System.out.println(res);
    } catch (ArrayIndexOutOfBoundsException e) {
      System.out.println(e.getMessage());
    } catch (NumberFormatException e) {
      System.out.println("参数格式不对，需要整数");
    } catch (ArithmeticException e) {
      System.out.println("参数不能为零～");
    }
  }

  public static double cal(int n1, int n2) {
    return n1 / n2;
  }
}
```
#### 2. 判断异常
```java
public class homework02 {
  public static void main(String[] args) {
    if(args[4].equals("Jack")) { //(x)主要是数组索引越界
      System.out.println("AA");
    } else {
      System.out.println("BB");
    }
    // String => Object(向上转型)(而编译类型成了Object => 为了多态)
    Object o = args[2]; // 此处o也成了父类引用(运行类型是String)
    // args的运行类型是String =(运行类型一致)= o向上转型后的运行类型依旧是String
    // i的运行类型是Integer != 父类引用o是String(说明Integer并不一致指向String)
    Integer i = (Integer)o; //(x) 会发生ClassCastException
    // 原则：要求父类的引用必须指向的是当前目标类型的对象(args就是父类的引用)
    // 不能将猫向下转型成狗 => String =x= Integer
  }
}
```
![](media/16629433472925/16643779765213.jpg)
#### 3. 输出结果
```Java
public class homework03 {
  public static void main(String[] args) {
    try {
      func(); // 一旦有异常,异常之后代码都不执行
      System.out.println("A");
    } catch (Exception e) {
      System.out.println("C");
    }
    System.out.println("D");
  }

  public static void func() {
    try {
      throw new RuntimeException();
    } finally {
      System.out.println("B");
    }
  }
}
```
![](media/16629433472925/16643785962687.jpg)

## 十二、常用类
### 1. 包装类
#### 1. 基本介绍
1. 针对八种基本数据类型相应的引用类型—包装类
2. 有了类的特点，就可以调用类中的方法
    ![](media/16629433472925/16643794070945.jpg)
#### 2. 包装类和基本数据的转换
1. `JDK5前`手动装箱和拆箱: `装箱: 基本类型 => 包装类型`
2. `之后`自动装箱和拆箱
3. 自动装箱底层调用的是valueOf方法
```Java
public class wrapperType {
  public static void main(String[] args) {
    // jdk5 前是手动装箱和拆箱
    // 手动装箱 int->Integer
    int n1 = 100;
    Integer integer = new Integer(n1);
    Integer integer1 = Integer.valueOf(n1);
    //手动拆箱 Integer -> int
    int i = integer.intValue();

    //jdk5 后，就可以自动装箱和自动拆箱 
    int n2 = 200;
    //自动装箱 int->Integer 
    Integer integer2 = n2; //底层使用的是 Integer.valueOf(n2) 
    // 自动拆箱 Integer->int 
    int n3 = integer2; //底层仍然使用的是 intValue()方法
  }
}
```
#### 3. 包装类型和 String 类型的相互转换
```Java
public class wrapperString {
  public static void main(String[] args) {
    // 包装类(Integer)->String
    Integer i = 100;
    String str1 = i + ""; // 方式1
    String str2 = i.toString(); // 方式2
    String str3 = String.valueOf(i);// 方式3

    // String->包装类(Integer)
    String str4 = "12345";
    Integer i2 = Integer.parseInt(str4);//自动装箱
    Integer integer = new Integer(str4);//构造器
  }
}
```
#### 4. 练习
```Java
public class wrapperExercise {
  public static void main(String[] args) {
    Double d = 100d;// 自动装箱(值 => 引用) 底层调用Double valueOf()
    // 等价于 Double d = new Double(100d);
    Float f = 1f;// 自动装箱
    Object obj1 = true ? new Integer(1):new Double(2);
    // 三元是一个整体 => 意味着精度小会被精度大替换掉
    System.out.println(obj1);// 1.0
    Object obj2;
    if (true) {
      obj2 = new Integer(1);
    }else {
      obj2 = new Double(2.0);
    }
    System.out.println(obj2);// 1 
  }
}
```
```Java
public class wrapperExercise02 {
  public static void main(String[] args) {
    Integer i = new Integer(1);
    Integer j = new Integer(1);
    System.out.println(i == j); //False
     /* 所以，这里主要是看范围 -128 ~ 127 就是直接返回
      1. 如果 i 在 IntegerCache.low(-128)~IntegerCache.high(127),就直接从数组返回
      2. 如果不在 -128~127,就直接 new
      Integer(i) public static Integer valueOf(int i) {
      if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
      }*/
    Integer m = 1; //底层 Integer.valueOf(1); -> 阅读源码
    Integer n = 1;//底层 Integer.valueOf(1);
    System.out.println(m == n); //T
    // 所以，这里主要是看范围 -128 ~ 127 就是直接返回，否则，就 new Integer(xx);
    Integer x = 128;//底层 Integer.valueOf(1);
    Integer y = 128;//底层 Integer.valueOf(1);
    System.out.println(x == y);//False
  }
}
```
### 2. String类
#### 1. String类的理解
 1. String对象用于保存字符串，也就是一组字符序列
 2. "jack" 字符串常量, 双引号括起的字符序列
 3. 字符串的字符使用 Unicode 字符编码，一个字符(不区分字母还是汉字)占两个字节
 4. String 类有很多构造器，构造器的重载
 5. String 类实现了接口 Serializable【String 可以串行化:可以在网络传输】
 6. String 是 final 类，不能被其他的类继承
 7. String 有属性 private final char value[]; 用于存放字符串内容
 8. value 是一个 final 类型,不可以修改[`即 value 不能指向新的地址，但是单个字符内容是可以变化`]
```Java
public static void main(String[] args) {
    // 1. String对象用于保存字符串，也就是一组字符序列
    // 2. "jack" 字符串常量, 双引号括起的字符序列
    String name = "Jack";
    // 3. 字符串的字符使用 Unicode 字符编码，一个字符(不区分字母还是汉字)占两个字节
    // 4. String 类有很多构造器，构造器的重载
    // 常用的有 String s1 = new String();
    // String s2 = new String(String original);
    // String s3 = new String(char[] a);
    // String s4 = new String(char[] a,int startIndex,int count)
    // String s5 = new String(byte[] b)
    // 5. String 类实现了接口 Serializable【String 可以串行化:可以在网络传输】
    // 接口 Comparable [String 对象可以比较大小]
    // 6. String 是 final 类，不能被其他的类继承
    // 7. String 有属性 private final char value[]; 用于存放字符串内容
    // 8. 一定要注意：value 是一个 final 类型,不可以修改(需要功力)
    // 即 value 不能指向新的地址，但是单个字符内容是可以变化
    name = "tom";
    final char[] value = {'a', 'b', 'c'};
    char[] v2 = {'t', 'o', 'm'};
    value[0] = 'H'; //value = v2; 不可以修改 value 地址
}
```
#### 2. 创建 String 对象
1. 方式一: 直接赋值 `String s = "fishx"`
    * 先从常量池查看是否有"fishx"数据空间,最终指向的是常量池的空间地址
        1. 如果有 `=>` 直接指向
        2. 如果没有 `=>` 重新创建,然后指向
2. 方式二: 调用构造器 `String s = new String("fishx")`
    * 先在堆中创建空间,里面维护了value属性,指向常量池的`"fishx"`空间
        1. 如果常量池没有`"fishx"`,重新创建
        2. 如果有直接 `=>` 通过value指向
3. 内存分析图
    ![](media/16629433472925/16652020685400.jpg)
#### 3. String 类的常见方法 
1. equals `比较内容是否相同，区分大小写`
2. equalsIgnoreCase `忽略大小写的判断内容是否相等`
    ```java
    String str1 = "hello";
    String str2 = "Hello";
    System.out.println(str1.equals(str2));// F
    System.out.println(str1.equalsIgnoreCase); // T    
    ```
3. length `获取字符的个数，字符串的长度`
    ```java
    System.out.println("fishx".length());
    ```
4. indexOf `获取字符在字符串对象中第一次出现的索引，索引从 0 开始，如果找不到，返回-1`
5. lastIndexOf `获取字符在字符串中最后一次出现的索引，索引从 0 开始，如果找不到，返回-1`
    ```java
    String s1 = "wer@terwe@g";
    int index = s1.indexOf('@');
    System.out.println(index);// 3
    s1 = "wer@terwe@g@";
    index = s1.lastIndexOf('@');
    System.out.println(index);//11
    System.out.println("ter 的位置=" + s1.lastIndexOf("ter"));//4
    ```
6. substring `截取指定范围的子串`
    ```java
    String name = "hello,张三"; //下面 name.substring(6) 从索引 6 开始截取后面所有的内容
    System.out.println(name.substring(6));//截取后面的字符
    //name.substring(0,5)表示从索引 0 开始截取，截取到索引 5-1=4 位置
    System.out.println(name.substring(2, 5));//llo
    ```
7. toUpperCase `转换成大写`
8. toLowerCase `转换成小写`
    ```java
    String str1 = "hello";
    String str2 = "HELLO";
    System.out.println(str1.toUpperCase()); // HELLO
    System.out.println(str2.toLowerCase()); // hello
    ```
9. concat `拼接字符串`
    ```java
    String str1 = "hello";
    System.out.println(str1.concat(",fishx")); // hello,fishx
    ```
10. replace `替换字符串中的字符`
    ```java
    s1 = "宝玉 and 林黛玉 林黛玉 林黛玉";
    System.out.println(s1.replace("林黛玉","ku"));
    // 宝玉 and ku ku ku
    // s1.replace() 方法执行后，返回的结果才是替换过的.
    // 注意对 s1 没有任何影响
    ```
11. split 分割字符串, 对于某些分割字符
    ```java
    String poem = "锄禾日当午,汗滴禾下土,谁知盘中餐,粒粒皆辛苦";
    String[] split = poem.split(",");
    System.out.println("==分割后内容===");
    for (String s : split) {
      System.out.println(s);
    }
    // ==分割后内容===
    // 锄禾日当午
    // 汗滴禾下土
    // 谁知盘中餐
    // 粒粒皆辛苦
    ```
12. toCharArray `转换成字符数组`
    ```java
    String s = "happy";
    char[] chs = s.toCharArray();
    for (char c : chs) {
      System.out.print(c + " "); // h a p p y 
    }
    ```
13. compareTo `比较两个字符串的大小，如果前者大则返回正数，后者大，则返回负数，如果相等，返回 0`
    ```java
    // (1) 如果长度相同，并且每个字符也相同，就返回 0
    // (2) 如果长度相同或者不相同，但是在进行比较时，可以区分大小
    String a = "jcck";// len = 3
    String b = "jack";// len = 4
    System.out.println(a.compareTo(b)); // 返回值是 'b' - 'a' = 2 的值
    ```
14. format `格式字符串`
    ```java
    String sname = "john";
    int age = 10;
    double score = 56.857;
    char gender = '男';
    String info = "我的姓名是" + sname + "年龄是" + age + ",成绩是" + score
        + "性别是" + gender + "。希望大家喜欢我！";
    System.out.println(info);
    // 1. %s 字符串, %c 字符, %d 整型, %.2f 浮点型 称为占位符, %c 使用 char 类型来替换
    // 2. 这些占位符由后面变量来替换
    // 3. %s 表示后面由 字符串来替换; %d 是整数来替换;
    // 4. %.2f 表示使用小数来替换,替换后,只会保留小数点两位,并且进行四舍五入的处理
    String formatStr = "我的姓名是%s 年龄是%d，成绩是%.2f 性别是%c.希望大家喜欢我！";
    String info2 = String.format(formatStr, name, age, score, gender);
    System.out.println("info2=" + info2);
    ```
#### 4. 练习
##### 1. 程序输出结果
```Java
public class StringExercise {
  public static void main(String[] args) {
    String s1 = "fishx"; // 先创建再指向的是常量池中的fishx
    String s2 = "java"; // 先创建再指向的是常量池java
    String s4 = "java"; // 直接指向的是常量池java
    // 在堆中创建空间[value -> java]
    String s3 = new String("java");// 指向的是堆中的对象
    // == 判断引用类型 => 比较地址
    System.out.println(s2 == s3); // 常量池 == 堆 ？(x)
    System.out.println(s2 == s4); // 常量池 == 常量池(√)
    // String包装类重写了equals方法 => 比较内容
    System.out.println(s2.equals(s3)); // java == java(√)
    System.out.println(s1 == s2);// 常量池(fishx) == 常量池(java) (x)
  }
}
``` 
```java
public static void main(String[] args) {
    Person p1 = new Person();
    p1.name = "fishx";
    Person p2 = new Person();
    p2.name = "fishx";
    System.out.println(p1.name.equals(p2.name));
    System.out.println(p1.name == p2.name);// 两属性是否相等
    System.out.println(p1.name == "fishx");
    String s1 = new String("bdc");
    String s2 = new String("bdc");
    System.out.println(s1 == s2); // 两对象是否相等
}
```
![String内存分析](media/16629433472925/16644362651195.jpg)
##### 2. 程序创建了几个对象
```Java
String s1 = "hello"; // 先查再创建最后指向
s1 = "haha"; // 并不是干掉了hello空间(没有引用就等着回收), 而是再创建了haha空间,然后重新指向此空间
// 因此创建了两对象
```
![](media/16629433472925/16646137004804.jpg)

```java
String s2 = new String("fishx");
s2 = "小靖";
```
![](media/16629433472925/16652022199405.jpg)


```java
String a = "hello" + "abc"; // 编译器自动优化
// 等价于 String a = "helloabc";
// 因此只创建了一个对象
```
```Java
public static void main(String[] args) {
    String a = "hello";
    String b = "fishx";
    // 1. 先创建一个StringBuilder sb = StringBuilder()
    // 2. 执行 sb.append("hello")
    // 3. sb.append("fishx")
    // 4. String c = sb.toString()
    public String toString() {
       return new String(value,0,count); 
    }
    // 最终c指向堆中对象(String) value[] -> 池中 "hellofishx"
    String c = a + b;
}
```
![](media/16629433472925/16644694562751.jpg)

###### 重要规则:
1. `String c1 = "ab" + "cd";` 常量相加,看的是池
2. `String c1 = a + b;` 变量相加,是在堆中

![](media/16629433472925/16644712727751.jpg)


### 3. StringBuffer 类
- 基本介绍: 
    - java.lang.StringBuffer代表可变的字符序列,可以对字符串内容进行增删
    - 很所多方法与String相同,但StringBuffer是可变长度的
    - StringBuffer是一个容器
    ```Java
    public final class StringBuffer extends AbstractStringBuilder
        implemenets java.io.Serializable, CharSequence {...}
    // 1. StringBuffer 是final类不能被继承
    // 2. 实现了Serializable接口,可以保存到文件or 网络传输
    // 3. 继承了抽象类AbstractStringBuilder
    // 4. AbstractStringBuilder属性char[] value, 存放字符序列,因此存放于堆中
    //  因此所有变化(增加/删除)不用每次都更换地址(即不是每次创建新对象)， 所以效率高于 String
    ```
- StringBuffer构造器的使用
    1. 创建一个 大小为16的 char[],用于存放字符内容
    2. 通过构造器指定char[]大小
    3. 通过给一个String创建StringBuffer,char[]大小就是str.length() + 16
    ```java
    // 1. 创建一个 大小为16的 char[],用于存放字符内容
    StringBuffer stringBuffer = new StringBuffer();
    // 2. 通过构造器指定char[]大小
    StringBuffer stringBuffer1 = new StringBuffer(100);
    // 3. 通过给一个String创建StringBuffer,char[]大小就是str.length() + 16
    StringBuffer hello = new StringBuffer("hello");
    ```
#### 1. String 🆚 StringBuffer

| String | StringBuffer |
| :-: | :-: |
| 保存的是字符串常量,值不能修改  | 保存的是字符串常量,值可以修改 |
| 每次更新实际上就是更改地址,效率低 | 每次更新实际上可以更新内容,效率高 |
| private final char value\[\]; | char\[\] value;//存放堆中 |

#### 2. String 和 StringBuffer 相互转换
* String ——> StringBuffer
    * 方式 1: 使用构造器
    * 方式 2: 使用的是 append 方法
* StringBuffer ——> String 
    * 方式 1: 使用 StringBuffer 提供的 toString 方法
    * 方式 2: 使用构造器来搞定
```java
//看 String——>StringBuffer
String str = "hello tom";
//方式 1 使用构造器 
// 注意： 返回的才是 StringBuffer 对象，对 str 本身没有影响
StringBuffer stringBuffer = new StringBuffer(str);
//方式 2 使用的是 append 方法 
StringBuffer stringBuffer1 = new StringBuffer();
stringBuffer1 = stringBuffer1.append(str);
    
//看看 StringBuffer ->String 
// 方式 1 使用 StringBuffer 提供的 toString 方法 
StringBuffer stringBuffer3 = new StringBuffer("韩顺平教育");
String s = stringBuffer3.toString();
//方式 2: 使用构造器来搞定 
String s1 = new String(stringBuffer3);
```
#### 3. StringBuffer 类常见方法
```java
public static void main(String[] args) {
    StringBuffer s = new StringBuffer("hello");
    //增
    s.append(',');// "hello,"
    s.append("张三丰");//"hello,张三丰"
    //删
    // 删除索引为>=start && <end 处的字符, 删除 1~4 的字符 [1, 4)
    s.delete(1, 4);
    System.out.println(s);//"ho,张三丰"
    //改
    // 使用 周芷若 替换 索引 3-6 的字符 [3,6)
    s.replace(3, 6, "周芷若");
    System.out.println(s);// "ho,周芷若"
    // 查
    // 找指定的子串在字符串第一次出现的索引，如果找不到返回-1
    int indexOf = s.indexOf("周芷若");
    System.out.println(indexOf);// 3
    // 插
    // 在索引为 3 的位置插入 "张三丰与",原来索引为 3 的内容自动后移
    s.insert(3,"张三丰与");
    System.out.println(s);// ho,张三丰与周芷若
    //长度
    System.out.println(s.length());// 10
}
```
#### 4. 练习
##### 1. 代码输出结果
```Java
String str = null;// 声明String对象
StringBuffer sb = new StringBuffer(); // 声明StringBuffer对象
// 查阅AbstractStringBuilder类中的appendNull()
sb.append(str); //char[] arr = {'n', 'u', 'l', 'l'};
System.out.println(sb.length());//4

System.out.println(sb); // null
// 下面构造器会抛出空值异常
StringBuffer sb1 = new StringBuffer(str);// super(str.length() + 16);
System.out.println(sb1);
```
##### 2. 处理千分位数字
```Java
String price = "123456789.10";
StringBuffer sb = new StringBuffer(price);
for (int i = sb.lastIndexOf(".") - 3; i > 0; i -= 3) {
  sb = sb.insert(i, ",");
}
System.out.println(sb);// 123,456,789.10
```
### 4. StringBuilder 类
1. 一个可变的字符序列
    1. 此类提供一个与StringBuffer兼容的API,但不保证同步(有线程安全问题)
    2. 此类被设计用作StringBuffer的一个简易替换,用于字符串缓冲区被单个线程使用时
    3. 如果可以建议优先采用此类,速度更快
2. 在StringBuilder上的主要操作是append和insert方法
    1. 可以重载这些方法
    2. 可以接受任意类型的数据
3. 解读: 
    1. StringBuffer 的直接父类 是 AbstractStringBuilder
    2. StringBuffer 实现了 Serializable, 即 StringBuffer 的对象可以串行化
    3. 在父类中 AbstractStringBuilder 有属性 `char[]` value,不是 final
        * 因此该 value 数组存放字符串内容，引出存放在堆中的
    4. StringBuffer 是一个 final 类，不能被继承
    5. 因为 StringBuffer 字符内容是存在 char[] value, 所有在变化(增加/删除)
        * 不用每次都更换地址 (即不是每次创建新对象)，所以效率高于 String

	![](media/16629433472925/16647018308427.jpg)
```java
public static void main(String[] args) {
    //老韩解读
    // 1. StringBuilder 继承 AbstractStringBuilder 类
    // 2. 实现了 Serializable ,说明 StringBuilder 对象是可以串行化(对象可以网络传输,可以保存到文件)
    // 3. StringBuilder 是 final 类, 不能被继承
    // 4. StringBuilder 对象字符序列仍然是存放在其父类 AbstractStringBuilder 的 char[] value;
    //  因此，字符序列是堆中
    // 5. StringBuilder 的方法，没有做互斥的处理,即没有 synchronized 关键字,因此在单线程的情况下使用
    StringBuilder stringBuilder = new StringBuilder();
}
```
### 以上三类比较

| String | StringBuffer | StringBuilder |
| :-: | :-: | :-: |
| 不可变字符序列 | 可变字符序列 | 可变字符序列 |
| 效率低 | 效率较高 | 效率高 |
| 复用率高 | 线程安全问题 | 线程不安全 |

* 使用原则,结论: 
    1. 如果字符串`存在大量修改`操作,一般使用`StringBuffer`或`StringBuilder`
    2. 如果字符串`存在大量的修改`操作,并在`单线程`的情况,使用`StringBuilder`
    3. 如果字符串`存在大量修改`操作,并在`多线程`的情况下,使用`StringBuffer`
    4. 如果我们字符串`很少修改`,被`多个对象引用`,使用`String`(比如一些配置)

### 5. Math
1. `abs(double/float/int/long a)`, 返回a的绝对值
2. 三角相关
    1. `acos(double a)`, 返回a的反余弦
    2. `atan(double a)`, 返回a的反正切
    3. `asin(double a)`, 返回a的反正弦
    4. `atan2(double y,double x)`,将矩形坐标转成极坐标,返回所得角
3. `cbrt(double a)`, 返回a值的立方根
4. pow 求幂
    ```Java
    double pow = Math.pow(2, 4);//2 的 4 次方 
    System.out.println(pow);//16
    ```
5. ceil 向上取整,返回>=该参数的最小整数(转成 double);
6. floor 向下取整，返回<=该参数的最大整数(转成 double)
7. round 四舍五入 Math.floor(该参数+0.5)
    ```Java
    double ceil = Math.ceil(3.9); 
    System.out.println(ceil);//4.0
    double floor = Math.floor(4.6);
    System.out.println(floor);//4.0
    double round = Math.round(1.6);
    System.out.println(round);//2.0 
    ``` 
8. sqrt 求开方
    ```java
    double sqrt = Math.sqrt(9.0); 
    System.out.println(sqrt);//3.0
    ```
9. random 求随机数, 返回的是 0 <= x < 1 之间的一个随机小数
    ```java
    // 请写出获取 a-b 之间的一个随机整数,a,b 均为整数
    // 公式就是 (int)(a + Math.random() * (b-a +1) )
    for (int i = 0; i < 100; i++) {
      // 2 ~ 7
      System.out.println((int) (2 + Math.random() * (7 - 2 + 1)));
    }
    ```
10. max , min 返回最大值和最小值 
    ```java
    //max , min 返回最大值和最小值 
    int min = Math.min(1, 9); 
    int max = Math.max(45, 90); 
    System.out.println("min=" + min); 
    System.out.println("max=" + max);
    ```
### 6. Arrays 类
Arrays里面包含了一系列静态方法,用于管理或操作数组(比如排序和搜索)

1. toString 返回数组的字符串形式
2. sort 排序(自然排序和定制排序)
3. binarySearch 通过二分搜索法进行查找,要求必须排好序
4. copyOf 数组元素复制
5. fill 数组元素的填充
6. equals 比较两个数组元素内容完全一致
7. asList 将一组值,转化成list
```text
练习: 书籍信息排序
    1. 价格从大到小排序
    2. 价格从小到大排序
    3. 按照书名长度从大到小
```
### 7. System 类
1. exit 退出当前程序
2. arraycopy 复制数组元素(适合底层调用),一般用Arrays.copyOf
    - arraycopy(源数组,起,目标数组,)
3. currentTimeMillens 返回当前时间戳
4. gc 运行垃圾回收机制

### 8. BigInteger 和 BigDecimal 类
需要处理很大的整数，精度不够用 `=>` BigInteger 和 BigDecimal 类解决
1. BigInteger 适合保存比较大的整形
2. BigDecimal 适合保存精度更高的浮点型
```Java
BigInteger bigInteger = new BigInteger("23788888899999999999999999999");
BigInteger bigInteger2 = new BigInteger("1009999999999999999999999");
System.out.println(bigInteger);
// 1. 在对 BigInteger 进行加减乘除的时候，需要使用对应的方法，不能直接进行 + - * / 
// 2. 可以创建一个 要操作的 BigInteger 然后进行相应操作 
BigInteger add = bigInteger.add(bigInteger2);
System.out.println(add); //加
BigInteger subtract = bigInteger.subtract(bigInteger2);
System.out.println(subtract);//减 
BigInteger multiply = bigInteger.multiply(bigInteger2);
System.out.println(multiply);//乘 
BigInteger divide = bigInteger.divide(bigInteger2);
System.out.println(divide);//除
```

```java
//当我们需要保存一个精度很高的数时，double 不够用, 可以是 BigDecimal
BigDecimal bigDecimal = new BigDecimal("1999.11");
BigDecimal bigDecimal2 = new BigDecimal("3");
System.out.println(bigDecimal);
// 1. 如果对 BigDecimal 进行运算，比如加减乘除，需要使用对应的方法 
// 2. 创建一个需要操作的 BigDecimal 然后调用相应的方法即可 
System.out.println(bigDecimal.add(bigDecimal2));
System.out.println(bigDecimal.subtract(bigDecimal2));
System.out.println(bigDecimal.multiply(bigDecimal2));
System.out.println(bigDecimal.divide(bigDecimal2));//可能抛出异常 ArithmeticException 
// 在调用 divide 方法时，指定精度即可. BigDecimal.ROUND_CEILING 
// 如果有无限循环小数，就会保留 分子 的精度 
System.out.println(bigDecimal.divide(bigDecimal2, BigDecimal.ROUND_CEILING));
```
### 9. 日期类
1. 第一代日期类
    1. Date 精确到毫秒,代表特定的瞬间(`获取当前时间(单位: 毫秒)`)
    2. SimpleDateFormat 格式和解析日期的类(日期 `<=>` 文本)(`格式化时间`)
2. 第二代日期类
    1. 主要是Calendar(日历)类
    2. `public abstract class Calendar extends Object implement Serializable, Cloneable, Comparable<Calendar>`
    3. 是一个抽象类,为特定日历字段之间的转换提供了一些方法
    ```java
    public static void main(String[] args) {
        //如果我们需要按照 24 小时进制来获取时间，
        // Calendar.HOUR ==改成=> Calendar.HOUR_OF_DAY
        Calendar c = Calendar.getInstance(); // 1.创建日历类对象, 比较简单，自由
        System.out.println("c=" + c);
        // 2.获取日历对象的某个日历字段
        // 这里为什么要 + 1, 因为 Calendar 返回月时候，是按照 0 开始编号
        System.out.println("年：" + c.get(Calendar.YEAR));
        System.out.println("月：" + (c.get(Calendar.MONTH) + 1));
        System.out.println("日：" + c.get(Calendar.DAY_OF_MONTH));
        System.out.println("小时：" + c.get(Calendar.HOUR));
        System.out.println("分钟：" + c.get(Calendar.MINUTE));
        System.out.println("秒：" + c.get(Calendar.SECOND));
        //Calender 没有专门的格式化方法，所以需要程序员自己来组合显示
        System.out.println(c.get(Calendar.YEAR) + "-" + (c.get(Calendar.MONTH) + 1 + "-" +
            c.get(Calendar.DAY_OF_MONTH) + " " + c.get(Calendar.HOUR_OF_DAY) + ":" +
            c.get(Calendar.MINUTE) + ":" + c.get(Calendar.SECOND)));
    }
    ```
3. 第三代日期类
    1. 第一代的大多方法因Calendar类的引入已经被弃用,而Calendar也存在问题:
        1. 可变性: 日期应该是不可变的
        2. 偏移性: Date中年份从1900开始的,而月份都从0开始
        3. 格式化: 格式化只对Date有用,Canlendar则不行
        4. 此外,它们也不是线程安全;不能处理闰秒等(每隔两天,多出1s)
    2. LocalDate(日期/年月日)、LocalTime(时间/时分秒)、LocalDateTime(日期时间/年月日时分秒)
        1. LocalDate 只包含日期
        2. LocalTime 只包含时间
        3. LocalDateTime 包含日期+时间 
```java
//1. 使用 now() 返回表示当前日期时间的 对象
LocalDateTime ldt = LocalDateTime.now();//LocalDate.now();LocalTime.now();
System.out.println(ldt);

// 2. 使用 DateTimeFormatter 对象来进行格式化
// 创建 DateTimeFormatter 对象
DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
String format = dateTimeFormatter.format(ldt);
System.out.println("格式化的日期=" + format);
System.out.println("年=" + ldt.getYear());
System.out.println("月=" + ldt.getMonth());
System.out.println("月=" + ldt.getMonthValue());
System.out.println("日=" + ldt.getDayOfMonth());
System.out.println("时=" + ldt.getHour());
System.out.println("分=" + ldt.getMinute());
System.out.println("秒=" + ldt.getSecond());   

 LocalDate now = LocalDate.now(); //可以获取年月日
LocalTime now2 = LocalTime.now();//获取到时分秒
System.out.println(now + " " + now2);// 2022-10-04 09:37:42.509

// 提供 plus 和 minus 方法可以对当前时间进行加或者减
// 看看 890 天后，是什么时候 把 年月日-时分秒
LocalDateTime localDateTime = ldt.plusDays(890);
System.out.println("890 天后 = " + dateTimeFormatter.format(localDateTime));
// 输出: 890 天后 = 2025-03-12 09:37:42
//看看在 3456 分钟前是什么时候，把 年月日-时分秒输出
LocalDateTime localDateTime2 = ldt.minusMinutes(3456);
System.out.println("3456 分钟前 日期 = " + dateTimeFormatter.format(localDateTime2)); 
// 输出: 3456 分钟前 日期=2022-10-02 00:01:42
```
### 10. 本章练习
#### 1. String翻转
```text
1. 将字符串中指定部分进行反转
2. 编写方法 public static String reverse(String str,int start,int end)解决
```

```java
public static void main(String[] args) {
    String str = "abcde";
    try {
      str = reverse(str, 1, 3);
    } catch (Exception e) {
      System.out.println(e.getMessage());
      return;
    }
    System.out.println("交换后： " + str);
}

  /**
   * String 反转
   * 思路分析：
   * （1）先把方法定义
   * （2）把String 转成char[]，char[]元素是可以交换的
   */
public static String reverse(String str, int start, int end) {
    if (!(str != null && start >= 0 && end > start && end < str.length())) {
      throw new RuntimeException("参数不正确！");
    }
    char[] chars = str.toCharArray();
    char temp;
    for (int i = start, j = end; i < j; i++, j--) {
      temp = chars[i];
      chars[i] = chars[j];
      chars[j] = temp;
    }
    return new String(chars);
}
```
#### 2. 注册校验
```Java
public class Homework02 {
  public static void main(String[] args) {
    String name = "fish";
    String pwd = "123456";
    String email = "2690866872@qq.com";
    try {
      userRegister(name, pwd, email);
      System.out.println("恭喜你，注册成功");
    } catch (Exception e) {
      System.out.println(e.getMessage());
    }
  }

  public static void userRegister(String name, String pwd, String email) {
    int userLength = name.length();
    if (!(userLength >= 2 && userLength <= 4)) {
      throw new RuntimeException("用户名长度为2或3或4");
    }
    if (!(pwd.length() == 6 && isDigital(pwd))) {
      throw new RuntimeException("密码长度为6，要求全是数字");
    }
    int i = email.indexOf("@");
    int j = email.indexOf('.');
    if (!(i > 0 && j > i)) {
      throw new RuntimeException("邮箱中包含@和. 并且@在.的前面");
    }
  }

  public static boolean isDigital(String str) {
    char[] chars = str.toCharArray();
    for (int i = 0; i < str.length(); i++) {
      if ((chars[i] < '0' || chars[i] > '9')) {
        return false;
      }
    }
    return true;
  }
}
```
#### 3. name大写处理
```Java
public static void main(String[] args) {
    String name = "zhong li jing";
    try {
      printName(null);
    } catch (Exception e) {
      System.out.println(e.getMessage());
    }
}

public static void printName(String str) {
  if (str == null) {
      System.out.println("不能为空～");
      return;
    }
    String[] name = str.split(" ");
    if (name.length != 3) {
    System.out.println("输入的字符串，格式不正确～");
    return;
  }
  String format = String.format("%s,%s .%c", 
        name[2], name[0], name[1].toUpperCase().charAt(0));
  System.out.println(format);
}
```
#### 4. 字符串统计
```Java
public static void main(String[] args) {
    countStr("Array123alan");
}

public static void countStr(String str) {
    if (str == null) {
      System.out.println("输入不能为空～");
      return;
    }
    int numCount = 0;
    int lowerCount = 0;
    int upperCount = 0;
    for (int i = 0; i < str.length(); i++) {
      if (str.charAt(i) >= '0' && str.charAt(i) <= '9') {
        numCount++;
      } else if (str.charAt(i) >= 'a' && str.charAt(i) <= 'z') {
        lowerCount++;
      } else if (str.charAt(i) >= 'A' && str.charAt(i) <= 'Z') {
        upperCount++;
      }
    }
    System.out.println("数字有 ：" + numCount);
    System.out.println("小写字母有：" + lowerCount);
    System.out.println("大写字母有：" + upperCount);
}
```
#### 5. String内存布局分析
![](media/16629433472925/16648895397837.jpg)
## 十三、集合
数组灵活性差 `=>` 集合
相当于可以存放各种基本数据类型的数组
1. 可以动态保存任意多个对象
2. 提供了一系列方便的操作对象的方法(add、remove、set、get...)
3. 使用集合添加、删除新元素的

### 1. 集合的框架体系
![](media/16629433472925/16648913714110.jpg)
![](media/16629433472925/16648913822164.jpg)
### 2. Collection 接口常用方法
1. collection实现子类可以存放多个元素,每个元素可以是Object
2. 有些Collection的实现类,可以存放重复的元素,有些不可以
3. 有些Collection的实现类,有些是有序的(List),有些不是有序(Set)
4. Collection接口没有直接的实现子类,是通过它的子接口Set和List来实现的

#### 1. 基本方法
```Java 
// Collection 接口常用方法,以实现子类 ArrayList 来演示
// 多态向上转型
List list = new ArrayList();
// add添加单个元素
list.add("Jack");
list.add(false);
list.add(12);// 自动装箱
System.out.println("list = " + list);

// remove：指定删除元素 or 索引
list.remove(false); // 指定删除
System.out.println("list = " + list);
list.remove(1); // 索引删除
System.out.println("list = " + list);

// contains: 查找元素是否存在
System.out.println(list.contains("Jack"));

// size：返回元素个数
System.out.println(list.size());

// clear: 清空
list.clear();
System.out.println(list);

// addAll: 添加多个元素[整体复制另一个集合]
ArrayList list2 = new ArrayList();
list2.add("红楼梦");
list2.add("三国演义");
list.addAll(list2);
list2.add("水浒传");
System.out.println("addAll之后: " + list);

// containsAll: 查找多个元素是否存在[a集合 == b集合]
System.out.println(list.containsAll(list2));

// removeAll: 删除多个元素[删除：a集合与b集合共有的]
list2.removeAll(list);
System.out.println("removeAll(list2)之后：" + list2);
```
#### 2. Collection 接口遍历元素方式: 
```Java
Collection collection = new ArrayList();
collection.add(new Book("三国演义", "罗贯中", 10.1));
collection.add(new Book("小李飞刀", "古龙", 5.1));
collection.add(new Book("红楼梦", "曹雪芹", 34.5));
System.out.println(collection);
```
##### (1) Iterator(迭代器)
* 主要用于: 遍历Collection集合中的元素
* 所有实现了Collection接口的集合类都有一个iterator()方法
    * 用以返回一个实现了iterator接口对象[`即可以返回一个迭代器`]
* iterator的结构
    ![](media/16629433472925/16649647928809.jpg)
* iterator仅用于遍历集合,iterator本身并不存放对象 
```Java
// A. 遍历集合 => iterator
// 1. 得到对应迭代器
Iterator iterator = collection.iterator();
// 2. while循环
while (iterator.hasNext()) { // 判断值是否存在
  Object obj = iterator.next();
  System.out.println("obj: " + obj);
}
```
##### (2) for 循环增强
- 增强for循环,可以替代iterator
- 特点: 简化版的iterator
```Java
// B. 增强for循环(既可以在集合使用，又可以在数组使用)
for (Object o : collection) {
  System.out.println("o: " + o);
}
```
### 3. List接口和常用方法
List接口是Collection接口的子接口
1. List集合类中元素有序(`即添加顺序和取出顺序一致`)、且可重复
2. List集合中的每个元素都有器对应的顺序索引(`即支持索引`)
3. List容器中的元素都对应一个整数型的序号记载其在容器中的位置,可以根据序号存取容器中的元素
#### 1. List 接口的常用方法
```Java
List list = new ArrayList();
list.add("Ace");
list.add("fishx");
System.out.println("原始数据：" + list);

// add: 可以指定插入索引
list.add(0, "Cat");
System.out.println("add插入后：" + list);
List list2 = new ArrayList();
list2.add("张三");
list2.add("莉丝");
list2.add("fishx");

// addAll：可以指定索引复制插入另一个集合
list.addAll(1, list2);
System.out.println("addAll之后：" + list);

// get：获取指定index位置的元素
System.out.println("第一个元素：" + list.get(0) +
    " 和 最后一个元素：" + list.get(list.size() - 1));

// indexOf: 第一次出现的位置；lastIndexOf：最后一个
System.out.println("'fishx'第一次出现的索引：" + list.indexOf("fishx")
    + " 最后一个的位置：" + list.lastIndexOf("fishx"));

// remove: 移除指定位置的元素，并返回此元素
list.remove("fishx");
System.out.println("remove之后：" + list);

// set: 设置指定位置的元素[相当于替换]
list.set(list.size() - 1, "fishc");
System.out.println("set之后：" + list);
```
#### 2. 排序练习
```text
使用List的实现类添加三本图书,并用冒泡排序遍历出来
```
```Java
public class List_Exercise02 {
  public static void main(String[] args) {
    List list = new ArrayList();
    list.add(new Book("红楼梦", "曹雪芹", 100));
    list.add(new Book("西游记", "吴承恩", 10));
    list.add(new Book("水浒传", "施耐庵", 19));
    list.add(new Book("三国", "罗贯中", 80));

    for (Object o : list) {
      System.out.println(o);
    }

    sort(list);
    System.out.println("\t\t\t-----排序后-----");
    for (Object o : list) {
      System.out.println(o);
    }
  }

  public static void sort(List list) {
    int listSize = list.size();
    for (int i = 0; i < listSize - 1; i++) {
      for (int j = 0; j < listSize - 1 - i; j++) {
        Book book1 = (Book) list.get(j);
        Book book2 = (Book) list.get(j + 1);
        if (book1.getPrice() > book2.getPrice()) {
          list.set(j, book2);
          list.set(j + 1, book1);
        }
      }
    }
  }
}
```
#### 3. ArrayList注意事项
1. ArrayList是由数组来实现数据存储的
2. ArrayList基本等同于Vector
    1. 除了ArrayList是线程不安全(执行效率高)
    2. 在多线程情况下,不建议用ArrayList
#### 4. ArrayList底层结构和源码分析
1. ArrayList中维护了一个Object类型的数组 `transient Object[] eleDate;`
    - `transient`: 短暂的,表示该属性不会被序列号
2. 当创建ArrayList对象时:
    1. 如果使用的是无参构造器,则初始eleDate容量为0
        - 第一次添加, 则扩容eleDate为10
        - 如需要再次扩容,则扩大eleDate的1.5倍
    2. 如果使用的是指定大小的构造器
        - 则初始eleDate容量为指定大小 
        - 如需要扩容,直接扩大eleDate的1.5倍
![](media/16629433472925/16651501271178.jpg)
![](media/16629433472925/16651881591180.jpg)
#### 5. Vector 的基本介绍
1. `public class Vector<E> extends AbstractList<E> implements List<E>...`
2. Vector底层也是一个对象数组. `protected Object[] elementDate;`
3. Vector是线程同步的[`线程安全`],Vector类的操作方法带有`synchronized`
4. 在开发中,需要线程同步安全时,考虑使用Vector
5. Vector与ArrayList比较

    | 比较项 | 底层结构 | 出现版本 | 线程安全效率 | 扩容倍数 |
    | :-: | :-: | :-: | :-: | :-: |
    | ArrayList | 可变数组 | jdk1.2 | 不安全,效率高 | 有参构造(1.5x),无参构造(默认10,之后1.5x) |
    | Vector | 可变数组 | jdk1.0 | 安全,效率不高 | 无参(默认10,之后2x),有参构造(2x) |

6. 源码分析
    ```Java
    public static void main(String[] args) {
        //无参构造器
        //有参数的构造
        Vector vector = new Vector(8);
        for (int i = 0; i < 10; i++) {
            vector.add(i);
        }
        vector.add(100);
        System.out.println("vector=" + vector);
        //老韩解读源码
        //1. new Vector() 底层
        /*
            public Vector() {
                this(10);
            }
         补充：如果是  Vector vector = new Vector(8);
            走的方法:
            public Vector(int initialCapacity) {
                this(initialCapacity, 0);
            }
         2. vector.add(i)
         2.1  //下面这个方法就添加数据到vector集合
            public synchronized boolean add(E e) {
                modCount++;
                ensureCapacityHelper(elementCount + 1);
                elementData[elementCount++] = e;
                return true;
            }
          2.2  //确定是否需要扩容 条件 ： minCapacity - elementData.length>0
            private void ensureCapacityHelper(int minCapacity) {
                // overflow-conscious code
                if (minCapacity - elementData.length > 0)
                    grow(minCapacity);
            }
          2.3 //如果 需要的数组大小 不够用，就扩容 , 扩容的算法
              //newCapacity = oldCapacity + ((capacityIncrement > 0) ?
              //                             capacityIncrement : oldCapacity);
              //就是扩容两倍.
            private void grow(int minCapacity) {
                // overflow-conscious code
                int oldCapacity = elementData.length;
                int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                                 capacityIncrement : oldCapacity);
                if (newCapacity - minCapacity < 0)
                    newCapacity = minCapacity;
                if (newCapacity - MAX_ARRAY_SIZE > 0)
                    newCapacity = hugeCapacity(minCapacity);
                elementData = Arrays.copyOf(elementData, newCapacity);
            }
         */
    }
    ```
#### 6. LinkedList
1. 双向链表基础
    ```java
    public class LinkedList {
      public static void main(String[] args) {
        Node jack = new Node("jack");
        Node tom = new Node("tom");
        Node fishx = new Node("fishx");
    
        // 连接三个节点，形成双向链表
        // 单向
        jack.next = tom;
        tom.next = fishx;
        // 双向
        fishx.pre = tom;
        tom.pre = jack;
    
        // 双向链表头节点
        Node first = jack;
        // 双向链表尾节点
        Node last = fishx;
    
        // 链表遍历
        System.out.println("———— 正序 ————");
        while (true) {
          if (first == null) {
            break;
          }
          System.out.println(first);
          first = first.next;
        }
    
        // 链表添加对象
        Node nick = new Node("Nick");
        tom.next = nick;
        nick.next = fishx;
        fishx.pre = nick;
        nick.pre = tom;
    
        // 链表遍历
        System.out.println("———— 倒序 ————");
        while (true) {
          if (last == null) {
            break;
          }
          System.out.println(last);
          last = last.pre;
        }
      }
    }
    
    class Node {
      public Node next;
      public Node pre;
      private Object item;
    
      public Node(Object name) {
        item = name;
      }
    
      @Override
      public String toString() {
        return "Node name = " + item;
      }
    }
    ```
2. 方法以及源码
    1. add()
        ```Java
        public static void main(String[] args) {
            LinkedList linkedList = new LinkedList();
            linkedList.add("fishx");
            linkedList.add(2);
            System.out.println("linkedList = " + linkedList);
        
            /* 1. LinkedList linkedList = new LinkedList();
                      public LinkedList() {}
               1. 这时 linkeList 的属性 first = null  last = null
               2. 执行 添加
                  public boolean add(E e) {
                       linkLast(e);
                       return true;
                   }
               4.将新的结点，加入到双向链表的最后
                void linkLast(E e) {
                   final Node<E> l = last;
                   final Node<E> newNode = new Node<>(l, e, null);
                   last = newNode;
                   if (l == null)
                       first = newNode;
                   else
                       l.next = newNode;
                   size++;
                   modCount++;
               }
              */
          }
        ```
        ![](media/16629433472925/16651969649646.jpg)
    2. remove()
        ```java
        public static void main(String[] args) {
            LinkedList linkedList = new LinkedList();
            linkedList.add("fishx");
            linkedList.add(2);
            System.out.println("linkedList = " + linkedList);
        
            //演示一个删除结点的
            linkedList.remove(); // 这里默认删除的是第一个结点
            //linkedList.remove(2);
            System.out.println("linkedList=" + linkedList);
            /*
                  linkedList.remove(); // 这里默认删除的是第一个结点
                  1. 执行 removeFirst
                    public E remove() {
                        return removeFirst();
                    }
                 1. 执行
                    public E removeFirst() {
                        final Node<E> f = first;
                        if (f == null)
                            throw new NoSuchElementException();
                        return unlinkFirst(f);
                    }
                  1. 执行 unlinkFirst, 将 f 指向的双向链表的第一个结点拿掉
                    private E unlinkFirst(Node<E> f) {
                        // assert f == first && f != null;
                        final E element = f.item;
                        final Node<E> next = f.next;
                        f.item = null;
                        f.next = null; // help GC
                        first = next;
                        if (next == null)
                            last = null;
                        else
                            next.prev = null;
                        size--;
                        modCount++;
                        return element;
                    }
                 */
          }
        ```
       ![](media/16629433472925/16651975424871.jpg)
    3. set()
        ```Java
        //修改某个结点对象
        linkedList.set(1, 999);
        System.out.println("linkedList=" + linkedList);
        ```
    4. get()
        ```Java
        Object o = linkedList.get(1);
        System.out.println(o);//999
        ```
    5. 因为LinkedList实现了List接口, 遍历方式(增强for,普通for,迭代器)
    
#### 7. ArrayList与LinkedList比较

| 比较项 | 底层结构 | 增删效率 | 改查效率 |
| :-: | :-: | :-: | :-: |
| ArrayList | 可变数组 | 较低\[数组扩容\] | 较高 |
| LinkedList | 双向链表 | 较高\[链表追加\] | 较低 |


##### 如何选择
1. 改查操作较多,选择ArrayList
2. 增删操作较多,选择LinkedList
3. 一般都是查较多,大部分情况下选择ArrayList
4. 在项目中,根据业务灵活选择,也可以一个模块使用ArrayList,另一个使用LinkedList

### 4. Set 接口和常用方法
#### 1. Set 接口基本介绍
1. 无序(添加和取出的顺序不一致),没有索引
2. 不允许重复元素,最多包含一个null

#### 2. Set 接口的常用方法
和 List 接口一样, Set 接口也是 Collection 的子接口，因此，常用方法和 Collection 接口一样.
#### 3. Set 接口的遍历方式
和Collection的遍历方式一样,因为Set接口是Collection接口的子接口
1. 可以使用迭代器、增强for
2. 不能使用索引方式来获取
```java
public static void main(String[] args) {
    // 1. 以 Set 接口的实现类 HashSet 来讲解 Set 接口的方法
    // 2. set 接口的实现类的对象(Set 接口对象), 不能存放重复的元素, 可以添加一个 null
    // 3. set 接口对象存放数据是无序(即添加的顺序和取出的顺序不一致)
    // 4. 注意：取出的顺序的顺序虽然不是添加的顺序，但是他的固定.
    Set set = new HashSet();
    set.add("John");
    set.add("lucy");
    set.add("John"); // 重复
    set.add("john");
    set.add(null);
    set.add(null);
    System.out.println("set = " + set);
    // 遍历
    System.out.println("———— 使用迭代器 ————");
    Iterator iterator = set.iterator();
    while (iterator.hasNext()) {
      Object obj = iterator.next();
      System.out.println("obj = " + obj);
    }
    
    // 删除
    set.remove(null);

    System.out.println("———— 增强for ————");
    for (Object o : set) {
      System.out.println("o = " + o);
    }
  }
```
#### 4. Set 接口实现类-HashSet
1. HashSet实现了Set接口
2. HashSet实际上是HashMap
3. 可以存放null值,但只有一个
4. HashSet不保证元素是有序的,取决于hash后,再确定索引的结果
5. 不能有重复元素/对象

```Java
public class HashSet01 {
  public static void main(String[] args) {
    HashSet set = new HashSet();

    //说明
    //1. 在执行add方法后，会返回一个boolean值
    //2. 如果添加成功，返回 true, 否则返回false
    //3. 可以通过 remove 指定删除哪个对象
    System.out.println(set.add("john"));//T
    System.out.println(set.add("lucy"));//T
    System.out.println(set.add("john"));//F
    System.out.println(set.add("jack"));//T
    System.out.println(set.add("Rose"));//T


    set.remove("john");
    System.out.println("set=" + set);//3个

    //
    set  = new HashSet();
    System.out.println("set=" + set);//0
    //4 Hashset 不能添加相同的元素/数据?
    set.add("lucy");//添加成功
    set.add("lucy");//加入不了
    set.add(new Dog("tom"));//OK
    set.add(new Dog("tom"));//Ok
    System.out.println("set=" + set);

    //在加深一下. 非常经典的面试题.
    //看源码，做分析， 先给小伙伴留一个坑，以后讲完源码，你就了然
    //去看他的源码，即 add 到底发生了什么?=> 底层机制.
    set.add(new String("hsp"));//ok
    set.add(new String("hsp"));//加入不了.
    System.out.println("set=" + set);


  }
}
class Dog { //定义了Dog类
  private String name;

  public Dog(String name) {
    this.name = name;
  }

  @Override
  public String toString() {
    return "Dog{" +
        "name='" + name + '\'' +
        '}';
  }
}
```
##### HashSet 底层机制说明**Mark**
分析HashSet底层是HashMap,HashMap底层是(数组+链表+红黑树)
![](media/16629433472925/16652087965522.jpg)

```Java
// 模拟简单的数组+链表结构
public class HashSetStructure {
  @SuppressWarnings({"all"})
  public static void main(String[] args) {
    // 模拟HashSet的底层(HashMap的底层)
    // 创建一个Node对象数组
    Node[] table = new Node[16];
    System.out.println("table = " + table);
    // 创建节点
    Node john = new Node("John", null);
    table[2] = john;
    Node jack = new Node("Jack", null);
    john.next = jack; // 将jack挂载到john
    Node rose = new Node("Rose", null);
    jack.next = rose;

    Node lucy = new Node("lucy", null);
    table[3] = lucy;
    System.out.println("table = " + table);
  }
}

class Node { // 节点,存储数据,可以指向下一个节点，从而形成链表
  Object item; // 存储数据
  Node next; // 指向下一个节点

  public Node(Object item, Node next) {
    this.item = item;
    this.next = next;
  }
}
```
##### 分析HashSet的添加元素底层实现
1. HashSet底层是HashMap
2. 添加一个元素时,先得到hash值,会转化为索引值
3. 找到存储数据表table,看这个索引位置是否已经存放元素
    1. 如果没有,直接加入
    2. 如果有,调用equals比较: 
        1. 如果相同,就放弃添加
        2. 如果不相同,则添加到最后
4. 在Java8中如果一条链表的元素个数到达`TREEIFY_THRESHOLD树_阈值(默认为8)`,并且table的大小 >= `MIN_TREEIFY_CAPACITY最小树化容量(默认为64)` 就会进行树化(红黑树)
- 第一次添加
    ```java
    HashSet hashSet = new HashSet();
    hashSet.add("java");
    hashSet.add("PHP");
    hashSet.add("JavaScript");
    System.out.println("set = " + hashSet);
    ```
    ```java
    // 1. 执行HashSet构造器
    public HashSet() {
      map = new HashMap<>();
    }
    ```
    
    ```java
    // 2. 执行add(E:泛型,e:传入的常量)方法
    public boolean add(E e) { // e = "java"
      return map.put(e, PRESENT) == null; // (static)PRESENT = new Object()
    }
    ```
    ```java
    // 3. 执行put方法，该方法会进入hash()
    public V put(K key, V value) { // key = "java", value = PRESENT(共享)
      return putVal(hash(key), key, value, false, true);
    }
    ```
    ```java
    // 4. 进入hash方法，计算返回唯一的hash值(h = key.hashCode() ^ h >>> 16)
    static final int hash(Object key) {
      int h;
      return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
    ```
    ```java 
    // 5. 执行代码(核心代码)
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,boolean evict) {
      Node<K, V>[] tab; Node<K, V> p; int n, i; // 定义辅助变量
      
      // table 就是 HashMap 的一个数组，类型是 Node[]
      // if 判断当前的table是否为空 or 大小为0 => 就第一次扩容至16个空间
      if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    
      // (1)根据key,得到hash 去计算该key应该存放到table表的哪个索引位置并把这个位置的对象赋给p
      /* (2) if 判断对象p是否为null 
          true => 表示还没有存放元素, 就创建一个 Node (key="java",value=PRESENT)
          false => 就放在该位置 tab[i] = newNode(hash, key, value, null)
      */
      if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
      else {
        Node<K, V> e; K k;
        if (p.hash == hash && ((k = p.key) == key || (key != null && key.equals(k))))
          e = p;
        else if (p instanceof TreeNode)
          e = ((TreeNode<K, V>) p).putTreeVal(this, tab, hash, key, value);
        else {
          for (int binCount = 0; ; ++binCount) {
            if ((e = p.next) == null) {
              p.next = newNode(hash, key, value, null);
              if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                treeifyBin(tab, hash);
                break;
            }
            if (e.hash == hash && ((k = e.key) == key || (key != null && key.equals(k))))
               break;
            p = e;
          }
      }
      if (e != null) { // existing mapping for key
        V oldValue = e.value;
        if (!onlyIfAbsent || oldValue == null)
          e.value = value;
          afterNodeAccess(e);
          return oldValue;
        }
      }
      ++modCount;
      if (++size > threshold)
        resize();
        afterNodeInsertion(evict);
        return null;
    }
    ```
#### 5. Set接口实现类-LinkedHashSet
##### LinkedHashSet的全面说明
1. LinkedHashSet 是HashSet的全面说明
2. LinkedHashSet 底层是一个LinkeHashMap,底层维护了一个**数组+双向链表**
3. LinkedHashSet 根据元素的hashCode值来决定元素的存储位置,同时使用链表维护元素的**次序**,由此看起来是以插入顺序保存的
4. LinkedHashSet不允许添加重复元素

### 5. Map 接口和常用方法
#### 1. Map 接口实现类的特点(JDK8)
1. Map与Collection并列存在.用于保存具有映射关系的数据(`Key-Value`)
2. Map中的key和value可以是任何引用类型的数据,会封装到`HashMap$Node`对象中
3. Map中的key不允许重复,Map中的value可以重复
4. Map的key可以为null,value也可以为null,注意key为null只能有一个,value为null可以多个
5. 常用String类作为Map的Key
6. key和value之间存在单向一对一关系(`即通过指定key总能找到对应的value`)

#### 2. Map 接口常用方法
```java
public class mapMethod {
  public static void main(String[] args) {
    Map map = new HashMap();
    map.put("邓超", new Book("", 100));//OK
    map.put("邓超", "孙俪"); //替换-> 一会分析源码
    map.put("王宝强", "马蓉");//OK
    map.put("宋喆", "马蓉"); //OK
    map.put("刘令博", null); //OK
    map.put(null, "刘亦菲"); //OK
    map.put("鹿晗", "关晓彤"); //OK
    map.put("hsp", "hsp 的老婆");
    System.out.println("map=" + map);
    // remove:根据键删除映射关系
    map.remove(null);
    System.out.println("map=" + map);
    // get：根据键获取值
    Object val = map.get("鹿晗");
    System.out.println("val=" + val);
    // size:获取元素个数
    System.out.println("k-v=" + map.size());
    // isEmpty:判断个数是否为 0
    System.out.println(map.isEmpty());//F
    // clear:清除 k-v
    map.clear();
    System.out.println("map=" + map);
    // containsKey:查找键是否存在
    System.out.println("结果=" + map.containsKey("hsp"));//T
  }
}
```
#### 3. Map 接口遍历方法
1. `containsKey`: 查找键是否存在
2. `keySey`: 获取所有的键
3. `entrySet`: 获取所有关系k-v
4. `values`: 获取所有的值

```java
public class mapFor {
  public static void main(String[] args) {
    Map map = new HashMap();
    map.put("邓超", new Book("", 100));//OK
    map.put("邓超", "孙俪"); //替换-> 一会分析源码
    map.put("王宝强", "马蓉");
    map.put("宋喆", "马蓉");
    map.put("鹿晗", "关晓彤");
    System.out.println("map=" + map);
    // containsKey: 查找键是否存在
    // keySey: 获取所有的键
    System.out.println("* * * keySey: (获取所有的键) * * *");
    Set set = map.keySet();
    for (Object o : set) {
      System.out.print(o + " ");
    }
    // values: 获取所有的值
    System.out.println("\n* * * values: (获取所有的值) * * *");
    Collection values = map.values();
    for (Object value : values) {
      System.out.print(value + " ");
    }
    // entrySet: 获取所有关系k-v
    System.out.println("\n* * * entrySet: (获取所有关系k-v) * * *");
    Set set1 = map.entrySet();
    for (Object o : set1) {
      Map.Entry entry = (Map.Entry) o;
      System.out.print(entry.getKey() + "-" + entry.getValue() + " ");
    }
  }
}
```
#### 4. Map 接口课堂练习
```text
使用HashMap添加3个员工对象,要求(键: 员工id  值: 员工对象)
并且遍历显示工资>18000的员工(遍历方式至少两种)
员工类: 姓名、工资、员工id
```
```java
import java.util.Random;

public class Employee {
  private String name;
  private double salary;
  private String id;

  public Employee(String name, double salary) {
    this.name = name;
    this.salary = salary;
    this.id = randomNum();
  }

  // 生成随机数
  // int randNumber = random.nextInt(max - min + 1) + min;
  // int randNumber = a + random.nextInt(b - a + 1);
  public String randomNum() {
    Random random = new Random();
    StringBuilder str = new StringBuilder();
    for (int i = 0; i < 4; i++) {
      str.append(random.nextInt(10));
    }
    return str.toString();
  }

  @Override
  public String toString() {
    return " " + name + "\t\t" + salary + "\t\t " + id;
  }

  public String getName() {
    return name;
  }

  public double getSalary() {
    return salary;
  }

  public String getId() {
    return id;
  }
}
```
```java
public class Test {
  public static void main(String[] args) {
    Map<String, Employee> hashMap = new HashMap<>();
    Employee fishx = new Employee("赵六", 12000);
    Employee zs = new Employee("张三", 21000);
    Employee ls = new Employee("李四", 19000);
    Employee ww = new Employee("王武", 24000);
    hashMap.put(fishx.getId(), fishx);
    hashMap.put(zs.getId(), zs);
    hashMap.put(ls.getId(), ls);
    hashMap.put(ww.getId(), ww);
    System.out.println("* * * * * 增强for循环 * * * * *");
    System.out.println("员工姓名\t\t" + "员工工资\t\t" + "员工ID");
    Collection<Employee> values = hashMap.values();
    for (Employee value : values) {
      if (value.getSalary() > 18000) {
        System.out.println(value);
      }
    }
    System.out.println("\n* * * * * 迭代器输出 * * * * *");
    System.out.println("员工姓名\t\t" + "员工工资\t\t" + "员工ID");
    Set<Map.Entry<String, Employee>> entries = hashMap.entrySet();
    Iterator<Map.Entry<String, Employee>> iterator = entries.iterator();
    while (iterator.hasNext()) {
      Map.Entry<String, Employee> next = iterator.next();
      if (next.getValue().getSalary() > 18000) {
        System.out.println(next.getValue());
      }
    }
  }
}
```
#### 5. Map 接口实现类-HashMap
##### 1. HashMap 小结
1. Map接口的常用实现类: HashMap、Hashtable和Properties
2. HashMap是Map使用频率最高的实现类
3. HashMap是以`key-value`键值对的方式来存储数据(`HashMap$Node类型`)
4. key不能重复,但是值可以重复,允许使用null键和null值
5. 如果添加相同的key,则会覆盖原来的`key-value`,等同于修改(key不会替换,value会替换)
6. 与HashSet一样,不保证映射的顺序,因为底层是以hash表的方式来存储的(`JKD8的HashMap底层: 数组+链表+红黑树`)
7. HashMap没有实现同步,因此是线程不安全的,方法没有做同步互斥的操作,没有synchronized

##### 2. HashMap 底层机制及源码剖析
![](media/16629433472925/16664489509719.jpg)

1. HashMap底层维护了Node类型的数组table,默认为null
2. 当创建对象时,将加载因子(loadfactor)初始化为0.75
3. 当添加key-value时,通过**key**的哈希值得到在table的索引,
    1. 然后判断该索引处是否有元素: 
        - 如果没有元素 => 直接添加
        - 如果该索引处有元素 => 继续判断该元素的key和准备加入的key是否相等: 
            - 如果相等 => 则直接替换value
            - 如果不相等 => 继续判断是树结构还是链表结构,做出相应处理
    2. 如果添加是发现容量不够 => 需要扩容 
		1. 第一次添加,则需要扩容table容量为16,临界值(threshold)为12 (16*0.75)
		2. 以后再扩容,则需要扩容table容量为原来的2倍(32),临界值为原来的2倍(24),依次类推
4. 在Java8中如果一条链表的元素个数到达`TREEIFY_THRESHOLD树_阈值(默认为8)`,并且table的大小 >= `MIN_TREEIFY_CAPACITY最小树化容量(默认为64)` 就会进行树化(红黑树)

#### 6. Map 接口实现类-Hashtable
##### 1. HashTable 的基本介绍
1. 存放的元素是键值对
2. HashTable的键和值都不能为null,否则会抛出`NullPointerException`
3. HashTable使用方法基本和HashMap一致
4. HashTable是线程安全`synchronize`的,HashMap是线程不安全的

##### 2. Hashtable 和 HashMap 对比

|  | 版本 | 线程安全(同步) | 效率 | 运行null键,null值 |
| :-: | :-: | :-: | :-: | :-: |
| HashMap | v1.2 | 不安全 | 高 | 可以 |
| HashTable | v1.0 | 安全 | 较低 | 不可以 |

#### 7. Map 接口实现类-Properties
##### 1. 基本介绍
1. Properties类继承自HashTable类并且实现了Map接口(键值对保存数据)
2. 使用方法和特点与HashTable类型相似
3. Properties**还可以用于**从xxx.properties文件中,加载数据到Properties类对象,并进行读取和修改
4. 说明: 实际开发时xxx.properties文件通常作为配置文件

##### 2. 基本使用
```java
public class Properties_ {
  public static void main(String[] args) {
    // 1.Properties 继承 HashTable
    // 2.可以通过k-v存储数据，其中key和value均不能为null
    Properties properties = new Properties();
    properties.put("Mak", 100);
    properties.put("zs", 200);
    properties.put("Ls", 100);
    properties.put("fishx", 421);
    properties.put("fishx", 4);// 若键相同，值替换
    System.out.println(properties);
    // 通过K => 访问V
    System.out.println(properties.get("fishx"));
    // 删除(键K): 返回删除元素的(值V); 删除(键K,值V): 返回boolean
    System.out.println(properties.remove("Mak"));
    // 修改
    properties.put("Ls","李四");
    System.out.println(properties);
    // 查
    System.out.println(properties.getProperty("Ls"));
  }
}
```

### 6. 总结: 开发中如何选择集合实现类(记住)
- 在开发中,选择什么集合实现类,主要取决于**业务操作的特点**,然后根据集合实现类特性进行选择
1. 先判断存储的类型(一组对象**单列**或一组键值对**双列**)
2. 一组对象**单列**: Collection接口
    1. 允许重复: List
        - 增改多: LinkedList **底层维护了一个双向链表**
        - 改查多: ArrayList **底层维护Object类型的可变数组**
    2. 不允许重复: Set
        - 无序: HashSet **底层是HashMap,维护了一个哈希值(即数组+链表+红黑树)**
        - 排序: **TreeSet**
        - 插入和取出顺序一致: LinkedHashSet,**维护数组+双向链表**
3. 一组键值对**双列**: Map
    - 键无序: HashMap **底层是: 哈希表(在JDK7:数组+链表,在JDK8:数组+链表+红黑树)**
    - 键排序: **TreeSet**
    - 键插入和取出顺序一致: LikedHashMap
    - 读取文件: Properties

### 7. Collections 工具类
#### 1. Collections 工具类介绍
1. Collections是一个操作Set、List和Map等集合的工具类
2. Collections中提供了一系列静态方法对集合元素进行排序、查询和修改等操作

#### 2. 排序操作:(均为 static 方法)
1. reverse(List): 反转List中元素的顺序
2. shuffle(List): 对List集合元素进行随机排序
3. sort(List): 根据元素的自然顺序对指定List集合元素按升序排序
4. sort(List,Comparator): 根据指定的Comparator产生的顺序对List集合元素进行排序
5. swap(List,int,int): 将指定List集合中的i处元素和j元素进行交换

#### 3. 查找、替换
![](media/16629433472925/16665029550692.jpg)

### 8. 本章作业
#### 1. 编程题
![](media/16629433472925/16665234967535.jpg)
```java
public class NewsApp {
  public static void main(String[] args) {
    ArrayList<News> array = new ArrayList<>();
    array.add(new News("新冠确诊病例超千万，数百万印度教信徒赴恒河“圣浴”引民众担忧"));
    array.add(new News("男子突然想起2个月前钓的鱼还在网兜里，捞起一看赶紧放生"));
    for (int i = array.size(); i > 0; i--) {
      News news = array.get(i - 1);
      System.out.println(processTitle(news.getTitle()));
    }
  }

  public static String processTitle(String title) {
    if (title == null) {
      return "";
    }
    if (title.length() > 15) {
      return title.substring(0, 15) + "...";
    } else {
      return title;
    }
  }
}
```
```java
public class News {
  private String title;
  private String content;

  public News(String title) {
    this.title = title;
  }

  @Override
  public String toString() {
    return "标题：" + title;
  }

  public String getTitle() {
    return title;
  }
}
```
#### 2. 简答题
分析HashSet和TreeSet分别如何实现去重的?

1. HashSet的去重机制: hashCode() + equals()
    - 底层先通过存入对象,进行运算得到一个hash值,通过hash值得到对应的索引
    - 如果发现table索引所在的位置
        - 没有数据 => 就直接存放 
        - 有数据 => equals()[遍历]比较 => 相同不加入,不相同加入
2. TreeSet的去重机制: 
    - 如果传入了一个Comparator匿名对象,就使用实现的方法去重
        - 如果方法return: 0,就认为是相同的元素/数据,就不添加
    - 如果没有传入一个Comparator匿名对象,则以你添加对象实现的 Compareable接口的compareTo去重
#### 3. 分析题
![](media/16629433472925/16665245981423.jpg)

## 十四、泛型
* 泛型优点:
    1. 泛型 `=>` 保证了类型安全
    2. 减少了类型转换次数,提高了效率
    3. 提高了代码的健壮性

* 泛(广泛)型(类型) => Integer、String、Dog
    1. 泛型又称参数化类型,是JDK5.0出现的新特性,解决数据类型和的安全性问题
    2. 在类声明或实例化时只要指定好需要的具体类型即可
    3. Java泛型可以保证如果程序在编译时没有发出警告,运行时就不会产生ClassCastException异常.同时,代码更加简洁、健壮
    4. 泛型的作用时: 
        - 可以在类声明时通过一个标识表示类中某个属性的类型,或者是某个方法的返回值类型,或者是参数类型

### 1. 泛型的语法
#### 1. 泛型的声明
```Java
interface 接口<T>{} 和 class 类<K,V>{}
// 比如: List, ArrayList
```
* 说明:
    * 其中,T,K,V不代表值,而是表示类型
    * 任意字母都可以.常用T表示,是Type的缩写

#### 2. 泛型的实例化
要在类名后面指定类型参数的值(类型)
```Java
List<String> strList = new ArrayList<String>();
Iterator<Customer> iterator = customers.iterator();
```
#### 3. 泛型使用举例
```text
练习:
1. 创建3个学生对象
2. 放入到HashSet中学生对象,使用
3. 放入到HashMap中,要求Key是String name,Value就是学生对象
4. 使用两种方式遍历
```
```Java
public class Student {
  private String name;
  private int age;

  @Override
  public String toString() {
    return "Student{" +
        "name='" + name + '\'' +
        ", age=" + age +
        '}';
  }

  public Student(String name, int age) {
    this.name = name;
    this.age = age;
  }
}
```
```java
public class Test {
  public static void main(String[] args) {
    HashSet<Student> students = new HashSet<>();
    students.add(new Student("jack", 19));
    students.add(new Student("nick", 20));
    students.add(new Student("fishx", 21));
    for (Student student : students) {
      System.out.println(student);
    }

    HashMap<String, Student> hashMap = new HashMap<>();
    hashMap.put("tom", new Student("tom", 21));
    hashMap.put("milan", new Student("milan", 34));
    hashMap.put("hsp", new Student("hsp", 18));
    Set<Map.Entry<String, Student>> entries = hashMap.entrySet();
    Iterator<Map.Entry<String, Student>> iterator = entries.iterator();
    while (iterator.hasNext()) {
      Map.Entry<String, Student> next = iterator.next();
      System.out.println(next.getKey() + ": " + next.getValue());
    }
  }
}
```
#### 4. 泛型使用的注意事项和细节
1. T,E只能是引用类型
    ```java
    interface List<T>{};
    public class HashSet<E>{}...
    
    List<Integer> list = new ArrayList<Integer>(); // ok
    List<int> list2 = new ArrayList<int>(); // 错误
    ```
2. 在给泛型指定具体类型后,可以传入该类型或者其子类类型
3. 泛型使用形式
    ```java
    List<Integer> list = new ArrayList<Integer>();
    List<Integer> list1 = new Arraylist<>();
    ```
```java
public class generic {
  public static void main(String[] args) {
    // 1.给泛型指向数据类型是，要求是引用类型，不能是基本数据类型
    List<Integer> integers = new ArrayList<>();

    // 2.在给泛型指定具体类型后，可以传入该类或其子类类型
    Pig<A> aPig = new Pig<>(new A());
    aPig.f();
    Pig<A> aPig1 = new Pig<>(new B());
    aPig1.f();

    // 3. 泛型使用形式(推荐使用，类型推断)
    ArrayList<Integer> integers1 = new ArrayList<>();
    
    // 4. 没有使用泛型,默认E为Object
    Pig<Object> p = new Pig<>();
  }
}

class A {
}

class B extends A {
}

class Pig<E> {
  E e;

  public Pig(E e) {
    this.e = e;
  }

  public void f() {
    System.out.println(e.getClass()); // 运行类型
  }
}
```
4. 泛型课堂练习题
    ```text
    定义Employee类:
    1. private成员变量(name,sal,birthday)并封装,其中birthday为MyDate类的对象
    2. 重写toString方法输出name,sal,birthday
    3. MyDate类包含: private成员变量month,day,year;并为每一个属性定义getter,setter方法
    4. 创建该类的3个对象,并把这些对象放入ArrayList集合中(ArrayList需使用泛型定义),
      对集合中的元素进行排序,并遍历输出
    排序方式:
        调用ArrayList的sort(),传入Comparator对象,
        先按照name排序,如果name相同,则按照生日日期先后排序
    ```
    ```java
    public class Employee {
      private String name;
      private double salary;
      private MyDate birthday;
    
      public Employee(String name, double salary, MyDate birthday) {
        this.name = name;
        this.salary = salary;
        this.birthday = birthday;
      }
    
      @Override
      public String toString() {
        return "\nEmployee{" +
            "name='" + name + '\'' +
            ", salary=" + salary +
            ", birthday=" + birthday +
            "}";
      }
    
      public String getName() {
        return name;
      }
    
      public MyDate getBirthday() {
        return birthday;
      }
    }
    ``` 
    ```java
    public class MyDate implements Comparable<MyDate> {
      private int month;
      private int day;
      private int year;
    
      public MyDate(int day, int month, int year) {
        this.month = month;
        this.day = day;
        this.year = year;
      }
    
      @Override
      public String toString() {
        return "MyDate{" +
            "month=" + month +
            ", day=" + day +
            ", year=" + year +
            '}';
      }
    
      public int getMonth() {
        return month;
      }
    
      public int getDay() {
        return day;
      }
    
      public int getYear() {
        return year;
      }
    
      @Override
      public int compareTo(MyDate o) {
        int yearMinus = year - o.getYear();
        int monthMinus = month - o.getMonth();
        int dayMinus = day - o.getDay();
        return yearMinus != 0 ? yearMinus : monthMinus != 0 ? monthMinus : dayMinus;
      }
    }
    ```
    ```java
    public class Test {
      public static void main(String[] args) {
        ArrayList<Employee> list = new ArrayList<>();
        list.add(new Employee("tom", 12000, new MyDate(17, 11, 2001)));
        list.add(new Employee("alink", 21000, new MyDate(10, 12, 2001)));
        list.add(new Employee("blink", 15000, new MyDate(9, 11, 1999)));
        list.add(new Employee("alink", 21000, new MyDate(10, 12, 1991)));
    
        list.sort(new Comparator<Employee>() {
          @Override
          public int compare(Employee e1, Employee e2) {
            if (!(e1 instanceof Employee && e2 instanceof Employee)) {
              System.out.println("类型不正确～");
              return 0;
            }
            // 比较name
            int i = e1.getName().compareTo(e2.getName());
            if (i != 0) {
              return i;
            }
            // 如果name相同，就比较birthday - year
            return e1.getBirthday().compareTo(e2.getBirthday());
          }
        });
        System.out.println("* * * 排序后的结果 * * *");
        System.out.println(list);
      }
    }
    ```
### 2. 自定义泛型
#### 1. 自定义泛型类
```Java
class 类名<T,R...> { // ...表示可以有多个泛型
 成员
}
```
```text
1. 普通成员可以使用泛型(属性、方法)
2. 使用泛型的数组,不能初始化
3. 静态方法中不能使用类的泛型
4. 泛型类的类型,是在创建对象是确定的(因为创建对象时,需要指定确定类型)
5. 如果在创建对象时,没有指定类型,默认为Object
```
```java
/**
 * 1. Tiger后面泛型，这种格式称为自定义泛型
 * 2. T，R，M 泛型标识符，一般是单个大写字母且允许有多个
 * 3. 普通成员可以使用泛型(属性，方法)
 * 4. 使用泛型的数组，不能初始化
 * 5. 被static修饰的成员不能使用泛型
 */
class Tiger<T, R, M> {
  String name;
  T t;
  R r;
  M m;
  // T[] ts = new T[8]; 类型形参 'T' 不能直接实例化
  T[] ts;

  // 构造器使用泛型
  public Tiger(String name, T t, R r, M m) {
    this.name = name;
    this.t = t;
    this.r = r;
    this.m = m;
  }

  /*
   静态是和类相关的，在类加载时，对象还没有创建
   因此，如果使用static修饰的成员使用泛型，JVM就无法完成初始化
    static R r2;
    public static void f(M m) {}
  */
  
  // 方法使用泛型
  public T getT() {
    return t;
  }

  public R getR() {
    return r;
  }

  public M getM() {
    return m;
  }
}
```
#### 2. 自定义泛型接口
```java
interface 接口名<T,R...> { //...表示可以有多个泛型
    成员
}
```
```text
1. 接口中,静态成员也不能使用泛型(因为需要确定类型才能被static所修饰)
2. 泛型接口的类型,在继承接口or实现接口时确定
3. 没有指定类型,默认为Object
```
```java
interface IUsb<U, R> {
  // 普通方法中，可以使用接口泛型
  R get(U u);

  void hi(R r);

  void run(R r1, R r2, U u1, U u2);

  // 在JDK8中，可以在接口中，使用默认方法
  default R method(U u) {
    return null;
  }
}

// 在继承接口 指定泛型接口的类型
interface IA extends IUsb<String, Double> { }

// 当我们去实现 IA 接口时，因为 IA 在继承 IUsu 接口时，指定了 U 为 String R 为 Double
// 在实现 IUsu 接口的方法时，使用 String 替换 U, 是 Double 替换 R
class A implements IA {
  @Override
  public Double get(String s) { return null; }
  
  @Override
  public void hi(Double aDouble) {}
  
  @Override
  public void run(Double r1, Double r2, String u1, String u2) {}
}
```
#### 3. 自定义泛型方法
```java
修饰符 <T,R...> 返回类型 方法名(参数列表) { }
```

```text
1. 泛型方法,可以定义在普通类中,也可以定义在泛型类中
2. 当泛型方法被调用时,类型会确定
3. public void eat(E e) { },修饰符后无<T,R...>eat()不是泛型方法,而是使用了泛型
```

```java
public class CustomGenericMethod {
  public static void main(String[] args) {
    Bird<String, Double, Float> bird = new Bird<>();
    // 泛型方法被调用时，类型会确定
    bird.fly("我会飞，我是String类型");
    bird.fly(3);
    Fish fish = new Fish();
    fish.show(12);
  }
}

class Bird<T, R, M> {
  public <E> void fly(E t) {
    System.out.println(t);
  }

  // 方法的修饰符后没有泛型定义，不是泛型方法，而是使用了泛型
  public void eat(R e) {
  }
}

class Fish {
  public void run() { // 普通方法
    System.out.println("Running...");
  }

  // 泛型方法，可以定义在普通类中，也可以定义在泛型类中
  public <A> A show(A t) {
    System.out.println("t的值：" + t);
    System.out.println("t的类型:" + t.getClass().getSimpleName());
    return t;
  }
}
```
* 自定义泛型方法练习
    ```Java
    public class Test {
      public static void main(String[] args) {
        // 下面代码输出了什么？
        Apple<String, Integer, Double> apple = new Apple<>();
        apple.fly(10); // int: 10 => Integer(自动装箱)
        apple.fly(new Dog()); // Dog
      }
    }
    
    class Apple<T, R, M> { // 自定义泛型类
      public <E> void fly(E e) {
        System.out.println(e.getClass().getSimpleName());
      }
    
      public void eat(U u) { // error，没有声明U
    
      }
      // 普通方法使用泛型，但并不是泛型方法
      public void run(M m) { }
    }
    
    class Dog { }
    ```
### 3. 泛型的继承和通配符  
1. 泛型不具备继承性
2. `<?>` 支持任意泛型类型
3. `<? extend A>` 支持A类以及A类的子类,规定了泛型的上限
4. `<? super A>` 支持 A 类以及 A 类的父类, 不限于直接父类, 规定了泛型的下限

```java
public class generic_extends {
  public static void main(String[] args) {
    // 1. 泛型不具备继承性
    // ArrayList<Object> list = new ArrayList<String>();error

    List<Object> list1 = new ArrayList<>();
    List<String> list2 = new ArrayList<>();
    List<AA> list3 = new ArrayList<>();
    List<BB> list4 = new ArrayList<>();
    List<CC> list5 = new ArrayList<>();

    // 2. <?>，支持任意泛型类型
    printCollection1(list5);

    // 3. <? extends AA>，表示上限，可以接受AA或者AA子类
    // printCollection2(list1); x
    // printCollection2(list2); x
    printCollection2(list3); // AA
    printCollection2(list4); // BB extends AA
    printCollection2(list5); // CC extends BB(间接继承AA)

    // 支持AA类以及AA类的父类,不限于直接父类,规定了泛型的下限
    printCollection3(list1); // Object
    // printCollection3(list2); x String
    printCollection3(list3); // AA
    // printCollection3(list4); x BB 
    // printCollection3(list5); x CC
  }

  // List<?>表示任意泛型都可以接受
  public static void printCollection1(List<?> c) { 
    int random = (int) Math.floor(Math.random());
    for (Object o : c) { // 通配符，取出时，就是Object
      System.out.println(o);
    }
  }

  // ？extends AA 表示上限，可以接受AA或者AA子类
  public static void printCollection2(List<? extends AA> c) {
    for (Object o : c) {
      System.out.println(o);
    }
  }

  // ？super 子类类名AA：支持AA类以及AA类的父类，不限于直接父类
  // 规定了泛型下限
  public static void printCollection3(List<? super AA> c) {
    for (Object o : c) {
      System.out.println(c);
    }
  }
}

class AA { }

class BB extends AA { }

class CC extends BB { }
```
### 4. JUnit
1. JUnit是一个Java语言的单元测试框架
2. 多数Java的开发环境都已经集成了JUnit作为单元测试的工具

@Test,安装依赖,然后导包.

## 十五、IO 流
* 什么是文件
    - 保存数据的地方
* 文件流
    - 流: 数据在数据源(文件)和程序(内存)之间经历的路径
    - 输入流: 数据从数据源(文件)到程序(内存)的路径
    - 输出流: 数据从程序(内存)到数据源(文件)的路径
    ![](media/16629433472925/16665821625180.jpg)
### 1. 常用的文件操作
#### 1. 创建文件对象相关构造器和方法
```Java
// 相关方法
new File(String pathname) // 根据路径构建一个File对象
new File(File parent,String child) // 根据父目录文件+子路径构建
new File(String parent,String child) // 根据父目录+子路径构建

// createNewFile 创建新文件
```
在windows下面你读文件可以从e://或者d:// 开始，但在mac下你必须从 / 目录开始。

正确的写法应该是 `/Users/fishx/simtext.txt`
```java
// 创建文件 news1.txt、news2.txt、news3.txt,用三种不同方式创建
public class FileCreate {
  // 方式 1 new File(String pathname)
  @Test
  public void createMethod01() {
    String filePath = "/Users/fishx/Documents/Java/txt/news1.txt";
    File file = new File(filePath);
    try {
      if (file.createNewFile()) {
        System.out.println("文件创建成功");
      }
    } catch (IOException e) {
      e.printStackTrace();
    }
  }

  // 方式 2 new File(File parent,String child)
  // 根据父目录文件+子路径构建
  @Test
  public void createMethod02() {
    String fileName = "news2.txt";
    File file = new File("/Users/fishx/Documents/Java/txt", fileName);
    try {
      if (file.createNewFile()) {
        System.out.println("文件创建成功");
      }
    } catch (IOException e) {
      e.printStackTrace();
    }
  }

  // 方式 3 new File(String parent,String child)
  // 根据父目录+子路径构建
  @Test
  public void createMethod03() {
    String parent = "/Users/fishx/Documents/Java/txt/";
    String fileName = "news3.txt";
    File file = new File(parent, fileName);
    try {
      if (file.createNewFile()) {
        System.out.println("文件创建成功");
      }
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```
* 如何获取Mac文件路径
    * 直接拖拽文件到终端，获取相关文件路径
    ![](https://img-blog.csdnimg.cn/80907d520d5d4d9c892921cb97b6e4bb.gif)
#### 2. 获取文件的相关信息
```java
// 如何获取文件的大小,文件名,路径,父File,是文件还是目录(目录本质: 是一种特殊的文件),是否存在
public class FileInformation {
  // 获取文件信息
  @Test
  public void info() {
    // 先创建文件对象
    File file = new File("/Users/fishx/Documents/Java/txt/news1.txt");
    // 调用相应方法，得到对应信息
    System.out.println("文件名称 = " + file.getName());
    System.out.println("文件绝对路径 = " + file.getAbsolutePath());
    System.out.println("文件父级目录 = " + file.getParent());
    System.out.println("文件大小(按字节统计) = " + file.length() + "(一个汉字3byte)");
    System.out.println("文件是否存在 = " + file.exists());
    System.out.println("是不是一个文件 = "+file.isFile());
    System.out.println("是不是一个目录 = "+file.isDirectory());
  }
}
```
#### 3. 目录的操作和文件删除
mkdir创建一级目录、mkdirs创建多级目录、delete删除空目录或文件
```text
1. 判断news1.txt是否存在,如果存在就删除
2. 判断dome02是否存在,存在 => 删除 || 不存在 => 提示
3. 判断dome\\a\\b\\c目录是否存在,存在 => 提示|| 不存在 => 创建
```
```java
import org.junit.jupiter.api.Test;
import java.io.File;

@SuppressWarnings({"all"})
public class Directory_ {
  @Test
  // 1. 判断news1.txt是否存在,如果存在就删除
  public void m1() {
    String filePath = "/Users/fishx/Documents/Java/txt/news1.txt";
    File file = new File(filePath);
    if (file.exists()) {
      if (file.delete()) {
        System.out.println(filePath + "，删除成功");
      } else {
        System.out.println(filePath + "，删除失败");
      }
    } else {
      System.out.println("该文件不存在");
    }
  }

  @Test
  // 2. 判断dome02是否存在,存在 => 删除 || 不存在 => 提示
  public void m2() {
    String filePath = "/Users/fishx/Documents/Java/txt/dome02";
    File file = new File(filePath);
    if (file.exists()) {
      if (file.delete()) {
        System.out.println(filePath + "，删除成功");
      } else {
        System.out.println(filePath + "，删除失败");
      }
    } else {
      System.out.println("目录不存在～");
    }
  }

  @Test
  // 3. 判断dome\\a\\b\\c目录是否存在,存在 => 提示 || 不存在 => 创建
  public void m3() {
    String filePath = "/Users/fishx/Documents/Java/txt/dome/a/b/c";
    File file = new File(filePath);
    if (file.exists()) {
      System.out.println("不存在此文件夹，" + filePath);
    } else {
      if (file.mkdirs()) {
        System.out.println(filePath + ",创建成功～");
      } else {
        System.out.println(filePath + ",创建失败～");
      }
    }
  }
}
```
### 2. IO 流原理及流的分类
#### 1. Java IO 流原理
1. I/O是Input/Output的缩写, I/O技术是非常实用的技术,用于处理数据传输.
2. Java程序中,对于数据的输入、输出操作以“流(stream)”的方式进行
3. Java.io包下提供了各种“流”类和接口,用以获取不同种类的数据,并通过方法输入或输出数据
4. 输入input: 读取外部数据(磁盘、光盘等存储设备的数据)到程序(内存)中
5. 输出output: 将程序(内存)数据输出到磁盘、光盘等存储设备中

#### 2. 流的分类
1. 按操作数据单位不同: 字节流(8bit)二进制文件, 字符流(按字符)文本文件
2. 按数据流的流向不同: 输入流、输出流
3. 按流的角色的不同: 节点流、处理流/包装流


| 抽象基类 | 字节流 | 字符流 |
| :-: | :-: | :-: |
| 输入流 | InputStream | Reader |
| 输出流 | OutputStream | Writer |

1. Java的IO流共涉及40多个类,实际上非常规则,都是从如上4个抽象基类派生的
2. 由这四个类派生出的子类名称都是以其父类名作为子类名后缀

### 3. IO 流体系图-常用的类
#### 1. IO 流体系图
![IO流](media/16629433472925/IO%E6%B5%81.png)

![](media/16629433472925/16665901493965.jpg)

#### 2. InputStream
* InputStream 关系图
    ![FileInputStream](media/16629433472925/FileInputStream.png)

##### 1. FileInputStream
```java
// FileInputStream字节输入流（文件 => 程序）
public class FileInputStream_ {
  public static void main(String[] args) {
  }

  @Test
  // 单字节读取，效率低 => 使用read(byte[] b)
  public void readFile01() {
    String filePath = "/Users/fishx/Documents/Java/txt/news2.txt";
    int readDate = 0;
    FileInputStream fileInputStream = null;
    try {
      // 创建了FileInputStream对象，用于读取文件
      fileInputStream = new FileInputStream(filePath);
      // read(): 从该输入流读取一个字节的数据。
      // 如果没有输入可用，此方法将被阻止，如果返回-1，表示文件读取完毕
      while ((readDate = fileInputStream.read()) != -1) {
        System.out.print((char) readDate);
      }
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      // 关闭资源（流），释放连接
      try {
        fileInputStream.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }

  @Test
  // 使用read(byte[] b) 提高效率
  public void readFile02() {
    String filePath = "/Users/fishx/Documents/Java/txt/news2.txt";
    int readLen = 0;
    FileInputStream fileInputStream = null;
    // 定义字节数组
    byte[] bytes = new byte[8]; // 一次读取8个
    try {
      fileInputStream = new FileInputStream(filePath);
      //从该输入流读取最多 b.length 字节的数据到字节数组。
      // 此方法将阻塞，直到某些输入可用。
      // 如果返回-1 , 表示读取完毕
      // 如果读取正常, 返回实际读取的字节数
      while ((readLen = fileInputStream.read(bytes)) > 0) {
        System.out.print(new String(bytes, 0, readLen));
      }
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      try {
        fileInputStream.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```
##### 2. BufferedInputStream
![](media/16629433472925/16668001979139.jpg)

#### 3. OutputStream
* OutputStream 关系图
    ![OutputStream](media/16629433472925/OutputStream.png)
##### 1. FileOutStream
如果文件不存在，会创建 文件(注意：前提是目录已经存在.)
```java
public void writeFile() {
    String filepath = "/Users/fishx/Documents/Java/txt/news2.txt";
    FileOutputStream fo = null;
    try {
      // (1).new FileOutputStream(filepath) 创建方式，会覆盖旧内容
      // (2).new FileOutputStream(filepath,true) 创建，不会覆盖
      fo = new FileOutputStream(filepath);
      // 1. fo.write('I'); 写入一个字节
      String str = "sbI love you!";
      // 2. fo.write(str.getBytes()); 写入字符串
      // getBytes()可以将String => Byte[]
      fo.write(str.getBytes(), 2, str.length() - 2);
      // 3. write(byte b[], int off, int len) 指定位置开始，长度要相应减去偏移量
      System.out.println("写入成功！");
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      try {
        fo.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
}
```
* 文件拷贝
```java
public void copyFile() {
    String originalFilePath = "/Users/fishx/Documents/Java/txt/dome/axia.png";
    String targetFilePath = "/Users/fishx/Documents/Java/txt/a.png";
    FileInputStream fi = null;
    FileOutputStream fo = null;
    try {
      int readContent = 0;
      byte[] bytes = new byte[1024];
      fi = new FileInputStream(originalFilePath);
      fo = new FileOutputStream(targetFilePath);
      while ((readContent = fi.read(bytes)) > 0) {
        // 读到之后，就输出(即边读边写)
        fo.write(bytes, 0, readContent);
      }
      System.out.println("拷贝成功～");
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      try {
        if (fo != null) {
          fo.close();
        }
        if (fi != null) {
          fi.close();
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
```
##### 2. BufferedOutputStream
```java
public class BufferedInputStream_ {
  public static void main(String[] args) {
    String target = "/Users/fishx/Downloads/我记得.mp3";
    String newFile = "/Users/fishx/Documents/Java/txt/I remember.mp3";

    // 创建对象 BufferedInputStream和
    BufferedInputStream bis = null;
    BufferedOutputStream bos = null;
    try {
      bis = new BufferedInputStream(new FileInputStream(target));
      bos = new BufferedOutputStream(new FileOutputStream(newFile));
      // 循环读取文件
      byte[] bytes = new byte[1024];
      int readInt;
      while ((readInt = bis.read(bytes)) != -1) {
        bos.write(bytes, 0, readInt);
      }
      System.out.println("文件拷贝完成～");
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      try {
        // 关闭外层处理流即可，底层自动关闭节点流
        if (bos != null) {
          bos.close();
        }
        if (bis != null) {
          bis.close();
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```
#### 4. Reader
![FileReader](media/16629433472925/FileReader.png)

##### 1. FileReader
FileReader 相关方法：
1. new FileReader(File/String)
2. read: 每次读取单个字符,返回该字符,如果读到文件末尾返回-1
3. read(char[]): 批量读取多个字符到数组,返回读取的字符数,如果读到文件末尾返回-1
4. 相关API:
    1. new String(char[]): 将char[]转换成String
    2. new String(char[],off,len): 将char[]的指定部分转换成String
```java
public class FileReader_ {
  public static void main(String[] args) {
    readFile01();
    readFile02();
  }

  private static void readFile01() {
    String filePath = "/Users/fishx/Documents/Java/txt/dome/yy.txt";
    FileReader fileReader = null;
    try {
      fileReader = new FileReader(filePath);
      int textContent;
      while ((textContent = fileReader.read()) != -1) {
        System.out.print((char) textContent);
      }
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      try {
        if (fileReader != null) {
          fileReader.close();
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }

  private static void readFile02() {
    String filePath = "/Users/fishx/Documents/Java/txt/dome/yy.txt";
    FileReader fileReader = null;
    try {
      fileReader = new FileReader(filePath);
      int textContent;
      char[] chars = new char[30];
      while ((textContent = fileReader.read(chars)) != -1) {
        System.out.print(new String(chars, 0, textContent));
      }
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      try {
        if (fileReader != null) {
          fileReader.close();
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```
##### 2. BufferedReader
```java
public class BufferedRead_ {
  public static void main(String[] args) {
    String filePath = "/Users/fishx/Documents/Java/code/chapter16/FileWriter_.java";
    BufferedReader br = null;
    try {
      br = new BufferedReader(new FileReader(filePath));
      String line; // 按行读取效率高
      while ((line = br.readLine()) != null) {
        System.out.println(line);
      }
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      try {
        if (br != null) {
          // 关闭流, 这里注意，只需要关闭 BufferedReader ，因为底层会自动的去关闭 节点流 FileReader
          /* 
          public void close () throws IOException {
            synchronized (lock) {
              if (in == null) return;
              try {
                in.close();//in 就是我们传入的 new FileReader(filePath), 关闭了.
              } finally {
                in = null;
                cb = null;
              }
            }
          }
        */
          br.close(); // 只需要关闭操作流
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```
* 多个文件合并
    ```java
    import java.io.*;
    
    public class FileCopy {
      public static void main(String[] args) {
        String[] strings = new String[2];
        strings[1] = "/Users/fishx/Documents/Java/txt/node.txt";
        strings[0] = "/Users/fishx/Documents/Java/txt/bfw.txt";
        String newFile = "/Users/fishx/Documents/Java/txt/new.txt";
        mergeFile(strings, newFile);
      }
    
      public static void mergeFile(String[] file, String newFile) {
        BufferedReader br = null;
        BufferedWriter bw = null;
        try {
          bw = new BufferedWriter(new FileWriter(newFile)); // 写数据，不能开辟两个空间
          for (String s : file) {
            br = new BufferedReader(new FileReader(s));
            String line;
            while ((line = br.readLine()) != null) {
              System.out.println(line);
              bw.write(line);
              bw.newLine();
            }
          }
        } catch (IOException e) {
          e.printStackTrace();
        } finally {
          try {
            if (br != null) {
              br.close();
            }
            if (bw != null) {
              bw.close();
            }
          } catch (IOException e) {
            e.printStackTrace();
          }
        }
      }
    }
    ```
#### 5. Writer
![BufferedWriter](media/16629433472925/BufferedWriter.png)

##### 1. FileWriter
FileWriter 相关方法：
1. new FileWriter(File/String): 覆盖模式(相当于流的指针在首端)
2. new FileWriter(File/String,true): 追加模式(相当于流的指针在尾端)
3. write(int): 写入单个字符
4. write(char[],off,len): 写入指定数组的指定部分
5. write(string): 写入整个字符串
6. write(string,off,len): 写入字符串的指定部分
7. 相关API: String类: toCharArray(将String转换成char[])
8. **注意**: FileWriter使用后,必须要关闭(close)或刷新(flush),否则写入不到指定的文件
```java
public void writerFile() {
    FileWriter fileWriter = null;
    try {
      String fileName = "/Users/fishx/Documents/Java/txt/node.txt";
      fileWriter = new FileWriter(fileName);//,true
      // 1) write(int): 写入单个字符
      fileWriter.write('H');
      // 2) write(char[],off,len): 写入指定数组的指定部分
      char[] chars = {'H', 'e', 'l', 'l', 'o'};
      fileWriter.write(chars, 1, chars.length - 1);
      // 3) write(string): 写入整个字符串
      String s = "风雨之后，定见彩虹";
      fileWriter.write("风雨之后");
      // 4) write(string,off,len): 写入字符串的指定部分
      fileWriter.write(s, 4, s.length() - 4);
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      try {
        if (fileWriter != null) {
          fileWriter.close();// 关闭文件流 等价于flush+关闭
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
}
```
##### 2. BufferedWriter
使用BufferedRead读取文本文件
```java
public class BufferedWriter_ {
  public static void main(String[] args) throws IOException {
    String filePath = "/Users/fishx/Documents/Java/txt/bfw.txt";
    //  创建对象
    BufferedWriter bw = new BufferedWriter(new FileWriter(filePath));
    bw.write("Hello,武家坡！");
    bw.newLine(); // 插入一个和系统相关的换行符
    bw.write("无论是屠洪刚先生的《定军山》里那一段“头通鼓”，")
    bw.write("还是此曲末尾“苏龙魏虎为媒证”，戏文一响，听来都让人感动。");
    bw.newLine();
    bw.write("我以为，这才是将流行歌曲与戏曲的有机结合，让更多的人愿意了解戏曲，爱上戏曲。" );
    bw.newLine();
    bw.write("以此为目的进行的创作，有血有肉，让人动容。我以为，这才是真正的戏腔。");
    // 关闭处理流，底层关闭节点流
    bw.close();
  }
}
```

### 4. 节点流和处理流
![](media/16629433472925/16667901868590.jpg)
#### 节点流和处理流的区别和联系
1. 节点流是底层流/低级流,直接对接数据源
2. 处理流(包装流)包装节点流,既可以清除不同节点流的实现差异,也可以提供更方便的方法来完成输入输出
3. 处理流(包装流)对节点流进行包装,使用了修饰器设计模式,不会直接于数据源相连

#### 处理流的功能主要体现在以下两个方面:
1. 性能的提高: 主要以增强缓冲的方式来提高输入输出的效率
2. 操作的便捷: 处理流(包装流)可能提供了一系列便捷的方法来一次输入输出大批量的数据,使用更加灵活方便

#### 包装流(处理流)分类
| 流的分类 | 输入流 | 输出流 | 作用 |
| :-: | :-: | :-: | :-: |
| 字符流 | BufferedReader | BufferedWriter | 主要操作文本 |
| 字节流 | BufferedInputStream | BufferedOutputStream | 主要操作二进制文件 |

#### 模拟修饰器设计模式
```java
public abstract class Reader_ {
  public void readFile() {
  }

  public void readString() {
  }
}
```
```java
public class StringReader_ extends Reader_ {
  public void readString() {
    System.out.println("对String进行读取...");
  }
}
```
```java
public class FileReader_ extends Reader_ {
  public void readFile() {
    System.out.println("对文件进行读取...");
  }
}
```
```java
// 做成包装流
public class BufferedReader_ extends Reader_ {
  private Reader_ reader_; // 属性是Reader_类型

  public BufferedReader_(Reader_ reader_) {
    this.reader_ = reader_;
  }

  // 让方法更加灵活 => 多次读取文件
  public void readFiles(int nums) {
    for (int i = 0; i < nums; i++) {
      reader_.readFile();
    }
  }

  // 扩展 readString，批量处理字符串数据
  public void readString(int num) {
    for (int i = 0; i < num; i++) {
      reader_.readString();
    }
  }
}
```
```java
public class Test {
  public static void main(String[] args) {
    BufferedReader_ brf = new BufferedReader_(new FileReader_());
    brf.readFiles(10);
    BufferedReader_ brs = new BufferedReader_(new StringReader_());
    brs.readString(10);
  }
}
```
### 5. 对象流介绍
功能：提供了对基本类型或对象类型的序列化和反序列化的方法
#### 1). ObjectInputStream 提供 反序列化功能
![](media/16629433472925/16668013616076.jpg)
```java
public class Object_Input {
  public static void main(String[] args) {
    ObjectInputStream ois = null;
    ArrayList<Dog> list = new ArrayList<>();
    try {
      ois = new ObjectInputStream(new FileInputStream("/Users/Documents/txt/dog.dat"));
      list = (ArrayList<Dog>) ois.readObject();
      for (Dog dog : list) {
        System.out.println(dog);
      }
    } catch (IOException | ClassNotFoundException e) {
      e.printStackTrace();
    } finally {
      try {
        if (ois != null) {
          ois.close();
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```
#### 2). ObjectOutputStream 提供 序列化功能
![](media/16629433472925/16668013952228.jpg)
```java
import java.io.Serializable;

public class Dog implements Serializable {
  private static final long serialVersionUID = 2646341473543292545L;
  private String name;
  private String type;

  public Dog(String name, String type) {
    this.name = name;
    this.type = type;
  }
}
```
```java
import java.io.*;
import java.util.ArrayList;

public class Object_Output {
  public static void main(String[] args) {
    Dog dog = new Dog("旺财", "中华田园犬");
    Dog dog1 = new Dog("大砖头", "哈士奇");
    Dog dog2 = new Dog("来福", "柯基");
    ArrayList<Dog> list = new ArrayList<>();
    list.add(dog);
    list.add(dog1);
    list.add(dog2);
    ObjectOutputStream oos = null;
    try {
      oos = new ObjectOutputStream(new FileOutputStream("/Users/Documents/txt/dog.dat"));
      oos.writeObject(list);
      System.out.println("序列化成功！");
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      try {
        if (oos != null) {
          oos.close();
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```
#### 3). 注意事项和细节说明
1) 读写顺序要一致
2) 要求序列化或反序列化对象,需要实现Serializable
3) 序列化的类中建议添加SerialVersionUID,为了提高版本的兼容性
4) 序列化对象时，默认将里面所有属性都进行序列化，但除了static或transient修饰的成员
5) 序列化对象时，要求里面属性的类型也需要实现序列化接口
6) 序列化具备可继承性，也就是如果某类已经实现了序列化，则它的所有子类也已经默认实现了序列化

### 6. 标准输入输出流

|   | 类型 | 默认设备 |
| :-: | :-: | :-: |
| System.in标准输入 | InputStream | 键盘 |
| System.out标准输出 | PrintStream | 显示器 |

```java
// System 类的in: public final static InputStream in = null;
// System.in 编译类型： InputStream
// System.in 运行类型： BufferedInputStream
// 表示的是标准输入 键盘
System.out.println(System.in.getClass());

// 1. System.out: public final static PrintStream out = null;
// 2. 编译类型 PrintStream
// 3. 运行类型 PrintStream
// 4. 表示标准输出 显示器
System.out.println(System.out.getClass());
```
### 7. 转换流-InputStreamReader 和 OutputStreamWriter
1. InputStreamReader是Reader的子类,可以将InputStream(字节流)包装成(转换)Reader(字符流)
2. OutputStreamWriter是Writer的子类,实现将OutputStream(字节流)包装成Writer(字符流)
3. 当处理纯文本数据时,使用字符流效率更高,并且可以有效解决中文乱码问题,所以建议将字节流转换成字符流
4. 可以在使用时指定编码格式(比如: utf-8,gdk,gb2312...)

* InputStreamReader
    ```java
    /**
     * 将FileInputStream包装成字符流InputStreamReader
     * 对文件进行读取（按照utf-8格式）,进而再包装成BufferedReader
     */
    public class InputStreamReader_ {
      public static void main(String[] args) throws IOException {
        String filePath = "/Users/fishx/Documents/Java/txt/new.txt";
        // 将FileInputStream(节点流) => InputStreamReader(字符流)
        InputStreamReader isr = new InputStreamReader
                (new FileInputStream(filePath), "utf-8");
        // 再把InputStreamReader(字符流) => BufferedReader(操作流)
        BufferedReader br = new BufferedReader(isr);
        // BufferedReader br = new BufferedReader(new InputStreamReader(
          //  new FileInputStream(filePath), "utf-8"));
        // 读取
        String line;
        while ((line = br.readLine()) != null) {
          System.out.println(line);
        }
        br.close();
      }
    }
    ```
* OutStreamWriter
    ```java
    // 1.创建流对象
    OutputStreamWriter osw = new OutputStreamWriter
                (new FileOutputStream("d:\\a.txt"), "gbk");
    // 2.写入 
    osw.write("hello,韩顺平教育~"); 
    // 3.关闭 
    osw.close(); 
    System.out.println("保存成功~");
    ```
### 8. 打印流-PrintStream 和 PrintWriter
* PrintStream
![](media/16629433472925/16668391656047.jpg)
```java
    public class PrintStream_ {
      public static void main(String[] args) throws IOException {
    
        PrintStream out = System.out;
        //在默认情况下，PrintStream 输出数据的位置是 标准输出，即显示器
            /*
               public void print(String s) {
                  if (s == null) {
                      s = "null";
                  }
                  write(s);
               }
             */
        out.print("john, hello");
        //因为print底层使用的是write , 所以我们可以直接调用write进行打印/输出
        out.write("韩顺平,你好".getBytes());
        out.close();
    
        //我们可以去修改打印流输出的位置/设备
        //1. 输出修改成到 "e:\\f1.txt"
        //2. "hello, 韩顺平教育~" 就会输出到 e:\f1.txt
        //3. public static void setOut(PrintStream out) {
        //        checkIO();
        //        setOut0(out); // native 方法，修改了out
        //   }
        System.setOut(new PrintStream("e:\\f1.txt"));
        System.out.println("hello, 韩顺平教育~");
      }
    }
```
* PrintWriter
![](media/16629433472925/16668391947447.jpg)
```java
    public class PrintWriter_ {
      public static void main(String[] args) throws IOException {
        //PrintWriter printWriter = new PrintWriter(System.out);
        PrintWriter printWriter = new PrintWriter(new FileWriter("e:\\f2.txt"));
        printWriter.print("hi, 北京你好~~~~");
        printWriter.close();//flush + 关闭流, 才会将数据写入到文件..
      }
    }
```
### 9. Properties 类
1) 专门用于读写配置文件的集合类
    - 配置文件的格式:
        - 键=值
        - 键=值
2) 注意:键值对不需要有空格，值不需要用引号扩起来。默认类型是String
3) Properties的常见方法
    - load:加载配置文件的键值对到Properties对象
    - list:将数据显示到指定设备
    - getProperty(key):根据键获取值
    - setProperty(key,value):设置键值对到Properties对象
    - store:将Properties中的键值对存储到配置文件，在idea中，保存信息到配置文件，如果含有中文，会存储为unicode码
     [unicode码查询工具](http://tool.chinaz.com/tools/unicode.aspx)
```java
// 使用Properties类读取mysql.properties文件
public class Properties01 {
  public static void main(String[] args) throws IOException {
    // 1. 创建Properties对象
    Properties properties = new Properties();
    // 2. 加载指定配置文件
    properties.load(new FileReader("src//mysql.properties"));
    // 3. 把k-v输出至控制台
    properties.list(System.out);
    // 4. 通过key获取value
    String user = properties.getProperty("user");
    System.out.println(user);
  }
}
```
```java
//使用 Properties 类来创建 配置文件, 修改配置文件内容
public class Properties02 {
  public static void main(String[] args) throws IOException {
    Properties properties = new Properties();
    // 创建key-value
    properties.setProperty("charset", "uft-8");
    properties.setProperty("user", "鱼香");
    properties.setProperty("pwd", "root");

    // 修改 => 底层是HashTable 1.没有 key => 创建 2.有 key => 修改
    /*
      Properties 父类是 Hashtable ， 底层就是 Hashtable 核心方法
      public synchronized V put(K key, V value) {
        // Make sure the value is not null
        if (value == null) {
          throw new NullPointerException();
        }

        // Makes sure the key is not already in the hashtable.
        Hashtable.Entry<?, ?> tab[] = table;
        int hash = key.hashCode();
        int index = (hash & 0x7FFFFFFF) % tab.length;
        @SuppressWarnings("unchecked")
        Hashtable.Entry<K, V> entry = (Hashtable.Entry<K, V>) tab[index];
        for (; entry != null; entry = entry.next) {
          if ((entry.hash == hash) && entry.key.equals(key)) {
            V old = entry.value;
            entry.value = value; //如果 key 存在，就替换
            return old;
          }
        }
        addEntry(hash, key, value, index);// 如果是新 k, 就 addEntry
        return null;
      }
    */
    properties.setProperty("user", "fishx");
    // 将k-v存储 Learning to write files...
    // 字符流 => 文本(设置文件编码), 字节流 => 码值
    properties.store(new FileOutputStream("src//mysql02.properties"), "学习写文件...");
    System.out.println("保存成功...");
  }
}
```
### 10. 章节练习
```text
(1)在判断e盘下是否有文件夹mytemp ,如果没有就创建mytemp
(2)在e:\mytemp目录下，创建文件hello.txt
(3)如果hello.txt已经存在，提示该文件已经存在，就不要再重复创建了
(4)并且在hello.txt文件中，写入hello,world~
```

```java
public static void main(String[] args) throws IOException {
    String directory = "/Users/fishx/Documents/Java/mytemp";
    String fileName = directory + "/hello.txt";
    File file = new File(directory);
    if (!file.exists()) {
      if (file.mkdirs()) {
        System.out.println(directory + ",文件夹创建成功～");
      } else {
        System.out.println(directory + ",文件夹创建失败～");
      }
    }
    file = new File(fileName);
    if (!file.exists()) {
      if (file.createNewFile()) {
        System.out.println(fileName + ",文件夹创建成功～");
      } else {
        System.out.println(fileName + ",文件夹创建失败～");
      }
    } else {
      System.out.println(fileName + ",文件已存在～");
    }
    FileOutputStream fileOutputStream = new FileOutputStream(fileName);
    String s = "hello,world~";
    fileOutputStream.write(s.getBytes());
    System.out.println("文件写入成功！");
    fileOutputStream.close();
}
```
```text
要求:使用BufferedReader读取文本文件，为每行加上行号,
再连同内容一 并输出到屏幕上。
//如果把文件的编码改成了gbk ,出现中文乱码，大家思考如何解决
//1.默认是按照utf-8处理，开始没有乱码
//2.提示:使用我们的转换流，将FileInputStream -> InputStreamReader[可以指定编码]
-> BufferedReader...
```
```java
public static void main(String[] args) throws IOException {
    String old = "/Users/fishx/Documents/Java/txt/old.txt";
    BufferedReader br = new BufferedReader(new FileReader(old));
    String line;
    int linNum = 0;
    while ((line = br.readLine()) != null) {
      System.out.println((++linNum) +" "+ line);
    }
    br.close();
}
```
```test
(1)要编写一个dog.properties
name= tom
age= 5
color= red
(2)编写Dog类(name,age,color)创建一个dog对象，读取dog.properties 用相应的内容完
成属性初始化，并输出
(3)将创建的Dog对象,序列化到文件dog.dat文件
```
```java
public class Dog implements Serializable {
  private static final long serialVersionUID = -6195201653811293172L;
  private String name;
  private int age;
  private String color;

  public Dog(String name, int age, String color) {
    this.name = name;
    this.age = age;
    this.color = color;
  }

  @Override
  public String toString() {
    return "Dog{" +
        "name='" + name + '\'' +
        ", age=" + age +
        ", color='" + color + '\'' +
        '}';
  }
}
```
```java
public class h03method {
  Properties properties = new Properties();
  Dog dog = null;

  @Test
  public void createConfig() throws IOException {
    properties.setProperty("name", "tom");
    properties.setProperty("age", "5");
    properties.setProperty("color", "red");
    properties.store(new FileOutputStream("src/dog.properties"), null);
    System.out.println("保存成功！");
  }

  @Test
  public void readConfig() throws IOException {
    // 2. 加载指定配置文件
    properties.load(new FileReader("src/dog.properties"));
    String name = properties.getProperty("name");
    int age = Integer.parseInt(properties.getProperty("age"));
    String color = properties.getProperty("color");
    dog = new Dog(name, age, color);
    System.out.println(dog);
  }

  @Test
  public void writeSerial() {
    ObjectOutputStream oos = null;
    try {
      oos = new ObjectOutputStream(new FileOutputStream("src/dog.dat"));
      oos.writeObject(dog);
      System.out.println("序列化成功！");
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      try {
        if (oos != null) {
          oos.close();
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }

  @Test
  public void readSerial() {
    ObjectInputStream ois = null;
    try {
      ois = new ObjectInputStream(new FileInputStream("src/dog.dat"));
      Dog o = (Dog) ois.readObject();
      System.out.println("反序列化结果：" + o);
    } catch (IOException | ClassNotFoundException e) {
      e.printStackTrace();
    } finally {
      try {
        if (ois != null) {
          ois.close();
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```
## 十六、线程 (基础)
### 1. 线程基本概念
#### 1. 进程
1. 进程是指运行中的程序(操作系统为进程分配内存空间)
2. 进程是程序的一次执行过程,或是正在运行的一个程序.
    - 是动态过程: 有它自身的产生、存在和消亡的过程

#### 2. 线程
1. 线程由进程创建的,是进程的一个实体
2. 一个进程可以拥有多个线程

#### 3. 单多线程
1. 单线程: 同一时刻,只允许执行一个线程
2. 多线程: 同一时刻,可以执行多个线程

#### 4. 并发并行
![](media/16629433472925/16666649710146.jpg)

### 2. 线程基本使用
#### 1. 创建线程的两种方式
##### 1. 继承Thread类, 重写run方法
```text
1. 编写程序,开启一个线程,该线程每隔1s,就输出“喵喵,我是小猫咪”
2. 改进: 输出80次,结束该线程
3. 使用JConsole监控线程执行情况
```
```java
public class ThreadUse {
  public static void main(String[] args) throws InterruptedException {
    // 创建Cat对象，可以当线程使用
    Cat cat = new Cat();
    /*
      (1).
      public synchronized void start() {
        start0();
      }
      (2).
      start0() 是本地方法，是 JVM 调用, 底层是 c/c++实现
      真正实现多线程的效果， 是 start0(), 而不是 run private native void start0();
     */
    cat.start(); // 启动线程 => 最终会执行cat的run方法
    /*
     为什么不能直接 cat.run();
     因为run 方法就是一个普通的方法（阻塞）, 没有真正的启动一个线程，就会把 run 方法执行完毕，才向下执行
    */
    // 说明：当main线程启动一个子线程Thread-0,主线程不会阻塞,会继续执行。
    for (int i = 1; i <= 60; i++) {
      System.out.println("主线程 i = " + i);
      // 让主线程休眠
      Thread.sleep(1000);
    }
  }
}


class Cat extends Thread {
  int num;

  @Override
  public void run() { // 重写run方法，业务逻辑
    do {
      System.out.println("喵喵,我是小猫咪" + (++num) + "线程名 = "
       + Thread.currentThread().getName());
      try {
        Thread.sleep(1000);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
    } while (num != 100);// 循环out，线程也终结
  }
}
```
##### 2. 实现Runnable接口, 重写run方法
```java
public class RunnableUse {
  public static void main(String[] args) {
    Thread thread = new Thread(new Dog(), "小狗");
    thread.start();
  }
}

class Dog implements Runnable {
  @Override
  public void run() {
    int count = 0;
    do {
      System.out.println("小狗汪汪叫...嗨" + (++count) + Thread.currentThread().getName());
      try {
        Thread.sleep(1000);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
    } while (count != 10);
  }
}
```
```java
public class RunnableUse {
  public static void main(String[] args) {
    Tiger tiger = new Tiger();
    ThreadProxy proxy = new ThreadProxy(tiger);
    proxy.start();
  }
}

class Animal { }

class Tiger extends Animal implements Runnable {
  @Override
  public void run() {
    System.out.println("老虎嗷嗷叫...");
  }
}

//线程代理类 , 模拟了一个极简的 Thread 类
class ThreadProxy implements Runnable {
  private Runnable target; // 属性，类型是Runnable

  public ThreadProxy(Runnable target) {
    this.target = target;
  }

  @Override
  public void run() {
    if (target != null) {
      target.run(); // 动态绑定（运行类型 Tiger）
    }
  }

  public void start() {
    start0();
  }

  public void start0() {
    run();
  }
}
```
#### 2. 多线程的简单使用
![](media/16629433472925/16667540944741.jpg)

```java
public class MoreThread {
  public static void main(String[] args) {
    Thread.currentThread().setName("SayHi");
    Thread t1 = new Thread(new SayHello(), "SayHello");
    t1.start();
    String name = Thread.currentThread().getName();
    for (int i = 1; i <= 5; i++) {
      System.out.println(name+ "第" + i + ": Hi");
      try {
        Thread.sleep(1000);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
      if (i == 5) {
        System.out.println("* * * * * "+name + "完成任务 * * * * *");
        break;
      }
    }
  }
}

class SayHello implements Runnable {

  @Override
  public void run() {
    int count = 0;
    String name = Thread.currentThread().getName();
    do {
      System.out.println(name + "第" + (++count) + ": Hello，world");
      try {
        Thread.sleep(1000);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
    } while (count != 10);
  }
}
```
#### 3. 继承 Thread vs 实现 Runnable 的区别
1. 从Java的设计来看,通过继承Thread或者Runnable接口来创建线程本质上没有区别(Thread类本身就实现了Runnable接口)
2. 实现Runnable接口方式更加适合多个线程共享一个资源的情况,并且避免了单继承的限制,建议使用Runnable

### 3. 线程常用方法

1. setName/getName:  设置/获取名称
2. start:  使线程开始执行(底层调用线程start0方法)
    - start底层会创建新的线程,调用run(不会启动新线程)
3. run: 调用线程对象run方法
4. setPriority/getPriority: 更改/获取线程优先级
    - Max最高优先级(10),5默认优先级,Min最低优先级(1)
5. sleep: 线程休眠
6. interrupt: 中断线程
7. yield: 线程礼让. 让出cpu,让其他线程执行,但礼让不一定成功
8. join: 线程插队,插队的线程一旦成功则先执行,被插队线程就处于就绪状态

```java
public class ThreadMethod01 {
  public static void main(String[] args) throws InterruptedException {
    //测试相关的方法
    T t = new T();
    t.setName("老韩");
    t.setPriority(Thread.MIN_PRIORITY);//1
    t.start();//启动子线程


    //主线程打印5 hi ,然后我就中断 子线程的休眠
    for (int i = 0; i < 5; i++) {
      Thread.sleep(1000);
      System.out.println("hi " + i);
    }

    System.out.println(t.getName() + " 线程的优先级 =" + t.getPriority());//1

    t.interrupt();//当执行到这里，就会中断 t线程的休眠.
    
  }
}

class T extends Thread { //自定义的线程类
  @Override
  public void run() {
    while (true) {
      for (int i = 0; i < 100; i++) {
        //Thread.currentThread().getName() 获取当前线程的名称
        System.out.println(Thread.currentThread().getName() + "  吃包子~~~~" + i);
      }
      try {
        System.out.println(Thread.currentThread().getName() + " 休眠中~~~");
        Thread.sleep(20000);//20秒
      } catch (InterruptedException e) {
        //当该线程执行到一个interrupt 方法时，就会catch 一个 异常, 可以加入自己的业务代码
        //InterruptedException 是捕获到一个中断异常.
        System.out.println(Thread.currentThread().getName() + "被 interrupt了");
      }
    }
  }
}
```
#### 用户线程和守护线程
1. 用户线程: 也叫工作线程,当线程的任务执行完或通知方式结束
2. 守护线程: 一般是为工作线程服务的,当**所有的用户线程结束**,守护线程自动结束
    * 设置守护线程: `Thread.setDaemon(true);`
3. 常见的守护线程: 垃圾回收机制

### 4. 线程的生命周期
* NEW (尚未启动的线程处于此状态)
* RUNNABLE (在Java虚拟机中执行的线程处于此状态)
* BLOCKED (被阻塞等待监视器锁定的线程处于此状态)
* WAITING (正在等待另一个线程执行动作达到指定等待时间的线程处于此状态)
* TERMINGATED (已退出的线程处于此状态)
![](media/16629433472925/16667641999306.jpg)

### 5. 线程同步
* 同步原理
![](media/16629433472925/16667673778737.jpg)

#### 1. 线程同步机制
1. 在多线程编程,一些敏感数据不允许被多个线程同时访问,此时就使用同步访问技术,保证数据在任何同一时刻,最多有一个线程访问,以保证数据的完整性
2. 也可以理解: 线程同步,即当有一个线程在对内存进行操作时,其他线程都不可以对这个内存地址进行操作,直到该线程完成操作,其他线程才能对该内存地址操作
#### 2. 同步具体方法-Synchronized
1. 同步代码块
    ```java
    synchronized (对象) { // 得到对象的锁,才能操作同步代码
      // 需要被同步代码;
    }
    ```
2. synchronized还可以放在声明中,表示整个方法(即同步方法)
    ```java
    (访问修饰符) synchronized void m (String name) {
      // 需要被同步代码;
    }
    ```
#### 3. 互斥锁
1. Java语言中,引入了对象互斥锁的概念,来保证共享数据操作的完整性
2. 每个对象都对应于一个可称为“互斥锁”的标记,这个标记用来保证在任一时刻,只能有一个线程访问该对象
3. 关键字synchronized来与对象的互斥锁联系.
    * 当某对象用synchronized修饰时,表明该对象在任一时刻只能由一个线程访问
4. 同步的局限性: 导致程序的执行效率降低
5. 同步方法(非静态的)锁可以是this,也可以是其他对象(要求是同一对象)
6. 同步方法(静态的)的锁为当前类本身

* 注意事项: 
    1. 同步方法如果没有使用static修饰: 默认锁对象为this
    2. 如果方法使用static修饰,默认锁对象: 当前类.class
    3. 实现的落地步骤:
        1. 需要先分析上锁的代码
        2. 选择**同步代码块**或同步方法
        3. 要求多个线程的锁对象为同一对象即可

#### 4. 线程的死锁
多个线程都占用了对方的锁资源,但不肯相让,导致了死锁,在编程是一定要避免死锁的发生

```java
public class DeadLock {
  public static void main(String[] args) {
    DeadLockDome A = new DeadLockDome(true);
    A.setName("线程A");
    DeadLockDome B = new DeadLockDome(false);
    B.setName("线程B");
    A.start();
    B.start();
  }
}

class DeadLockDome extends Thread {
  static Object o1 = new Object();// 保证多线程，共享一个对象,这里使用 static
  static Object o2 = new Object();
  boolean flag;

  public DeadLockDome(boolean flag) {
    this.flag = flag;
  }

  @Override
  public void run() {
    /*
    下面业务逻辑的分析:
     1. 如果 flag 为 T, 线程 A 就会先得到/持有 o1 对象锁, 然后尝试去获取 o2 对象锁
          => 如果线程 A 得不到 o2 对象锁，就会 Blocked
     2. 如果 flag 为 F, 线程 B 就会先得到/持有 o2 对象锁, 然后尝试去获取 o1 对象锁
          => 如果线程 B 得不到 o1 对象锁，就会 Blocked
    */
    if (flag) {
      synchronized (o1) { // 对象互斥锁, 下面就是同步代码
        System.out.println(Thread.currentThread().getName() + " 进入1");
        synchronized (o2) { // 这里获得 o2 对象的监视权
          System.out.println(Thread.currentThread().getName() + " 进入2");
        }
      }
    } else {
      synchronized (o2) {
        System.out.println(Thread.currentThread().getName() + " 进入 3");
        synchronized (o1) { // 这里获得 o1 对象的监视权
          System.out.println(Thread.currentThread().getName() + " 进入 4");
        }
      }
    }
  }
}
```
#### 5. 释放锁
1. 释放锁
    ![](media/16629433472925/16667705711598.jpg)
2. 不会释放锁
    ![](media/16629433472925/16667705817279.jpg)

### 章节练习
1. 通知关闭线程
    ```text
    1. 在main方法中启动两个线程
    2. 第1个线程循环随机打印100以内的整数
    3. 直到第2个线程从键盘读取了“Q”命令,才关闭线程A
    ```
    ```java
    public class H01 {
      public static void main(String[] args) {
        A a = new A();
        B b = new B(a);
        a.start();
        b.start();
      }
    }
    
    class A extends Thread {
      private boolean loop = true;
    
      @Override
      public void run() {
        // 输出 1 ～ 100 数字
        while (loop) {
          System.out.println((int) (Math.random() * 100 + 1));
          try {
            Thread.sleep(1000);
          } catch (InterruptedException e) {
            e.printStackTrace();
          }
        }
      }
    
      public void setLoop(boolean loop) {
        this.loop = loop;
        System.out.println("A线程收到B线程关闭通知...");
      }
    }
    
    class B extends Thread {
      private A a; // 必须持有A线程，才有通知方法
      private Scanner scanner = new Scanner(System.in);
    
      public B(A a) {
        this.a = a;
      }
    
      @Override
      public void run() {
        while (true) {
          System.out.println("请输入你的指令(Q)表示关闭：");
          char key = scanner.next().toUpperCase().charAt(0);
          if (key == 'Q') {
            // 以通知方式结束a线程
            a.setLoop(false);
            System.out.println("B线程退出...");
            break;
          }
        }
      }
    }
    ```
2. 取款问题
    ```java
    public class H02 {
      public static void main(String[] args) {
        T sal = new T();
        Thread t1 = new Thread(sal, "张三");
        Thread t2 = new Thread(sal, "张三之妻");
        t1.start();
        t2.start();
      }
    }
    
    // 为什么不能使用： extends Thread 
    // 因为银行账户属于共享资源 => 涉及多个线程共享资源需实现Runnable接口
    class T implements Runnable {
      private static int money = 10000;
    
      @Override
      public void run() {
        while (true) {
          synchronized (this) {
            if (money <= 0) {
              System.out.println("余额为0，无法取款");
              break;
            }
            money -= 1000;
            System.out.println(Thread.currentThread().getName()
                + "取款成功，余额剩余：" + money);
            try {
              Thread.sleep(1000);
            } catch (InterruptedException e) {
              e.printStackTrace();
            }
          }
        }
      }
    }
    ```
