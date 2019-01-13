## 字符编码
### 目的
计算机字符编码，要让人类字符能用计算机理解的形式(01串)无歧义地表达。

### 问题
主要分为两个问题：   
1. **所有字符都有一个唯一标识的数字**，1对1的map映射，即可以通过一个数字找到唯一对应的字符；   
2. **该数字能由流式二进制1/0表示**，即必须能在流式的二级制串中**识别**出该数字，*这由计算机组成确定的*   

### 解决方案
主要是第二个问题（*需要读取多少位bit来作为该数据的值*），大致分为两类解决方案：   
1. 固定字节长度，explicit，即明确规定多少位bit表示一个数字，采用此方法的编码比如ascii 8 bit，UTF-16，GBK 32bit等   
2. 非固定长度，implict，需要暗示接下来多少位bit是一个数字   
> 比如 UTF-8，类似*TLV(Type Length Value)* ，字节流分为首字节和非首字节，非首字节以10开头，首字节首部1的个数代表一共需要的字节数，0开头兼容ascii   
   110xxxxx 10xxxxxx   
   1110xxxx 10xxxxxx 10xxxxxx   

*注：计算机上字节流还需要考虑字节序的问题，也即大小端问题，with(out) BOM(Byte Order Mark)，FEFF 大端，FFEF 小端*    

   

### 实际编码
通用字符集**Unicode**(Universal Character Set)为每一个字符定义唯一的代码(U+000000到 U+10FFFF)。在实际传输、存储过程中，由于不同系统平台的设计不一定一致，以及出于节省空间的目的，对Unicode编码的实现方式有所不同。Unicode的实现方式称为Unicode转换格式（Unicode Transformation Format，简称为**UTF**）。

**Unicode字符** <--> java**代码点**   
**java char** <--> java**代码单元**，**str.length()** 返回char个数。代码单元是指一个已编码的文本中具有最短的比特组合的单元。   

#### 代码相关编码
要注意**源代码文件**、**操作系统默认**、**语言平台默认**(内存中)编码。
1. **Java**   
    1. ***源文件*** 编码方式(一般系统默认，window GBK，Linux UTF-8)。
    2. ***javac*** 按**系统默认**编码读取源文件，以**UTF-8**写到.class文件，可以javac -encoding修改读取的编码格式，*IDE一般会默默做这个事情*。
    3. ***JVM*** 中运行时字符串按**UTF-16**，因为char只有16位，所以辅平面的需要两个char表示。
2. **Python**   
    1. ***源文件*** 可在.py文件中表明，#-\*- coding:utf-8 -\*-
    2. ***内存*** 中运行时字符串按**UTF-8**，2和3有点区别。

#### UTF-16
将Unicode中字符分为**基平面**(2字节，U+0000到U+FFFF)和**辅平面**(4字节，码点范围U+010000到U+10FFFF，20bit)，基平D800-DFFF留空，超16bit表示范围的(c - 0x1 0000 0000)分到辅平中去。高(D800-DBFF)低(DC00-DFFF)   

**猜想设计**   
因为基平面D800-DFFF留空，二进制：   
1000-1111都以1开头，作为辅助平面的标志，剩下3+8=11bit，能表示2048个字符，肯定不行    
于是再拿出一个代码单元，用两个基平面标识(Java中的代码单元)来组合成辅平面标识，两个代码单元本身存在先后顺序，于是规定10表示辅平面前两个字节（高），11表示辅平面后两个字节（低）。 
于是剩下10+10=20bit，辅平面共可以表示2^20个字符。   
因此UTF-16一共可表示2^20+2^16个字符。   

```java
public class CharTest {
        private static final char hexDigits[] = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F'};
        private static String bytes2HexString(byte[] bytes) {
                if (bytes == null) return null;
                int len = bytes.length;
                if (len <= 0) return null;
                char[] ret = new char[len << 1]; 
                for (int i = 0, j = 0; i < len; i++) {
                        ret[j++] = hexDigits[bytes[i] >>> 4 & 0x0f];
                        ret[j++] = hexDigits[bytes[i] & 0x0f];
                }   
                return new String(ret);
        }   
        public static void main(String[] args) throws Exception {
                // String str = "中";
                // System.out.println("len="+str.length());
                // System.out.println("Bytes len="+str.getBytes().length);
                String str = "哈";
                System.out.println("str="+str);
                System.out.println("str len=" + str.length());
                System.out.println("str getBytes=" + str.getBytes().length+"  HEX="+bytes2HexString(str.getBytes()));
                System.out.println("str getBytes UTF-8=" + str.getBytes("UTF-8").length+"  HEX="+bytes2HexString(str.getBytes("UTF-8")));
                System.out.println("str getBytes UTF-16=" + str.getBytes("UTF-16").length+"  HEX="+bytes2HexString(str.getBytes("UTF-16")));
                System.out.println("str getBytes UTF-16BE=" + str.getBytes("UTF-16BE").length+"  HEX="+bytes2HexString(str.getBytes("UTF-16BE")));
                System.out.println("str getBytes UTF-16LE=" + str.getBytes("UTF-16LE").length+"  HEX="+bytes2HexString(str.getBytes("UTF-16LE")));
                System.out.println("str getBytes Unicode=" + str.getBytes("Unicode").length+"  HEX="+bytes2HexString(str.getBytes("Unicode")));
                char[] c = {'哇'};
                System.out.println(c.length);
                String s = "\u03C0\uD835\uDD6B";
                // Unicode编码， 3个代码单元，2个代码点
                int lenU = s.length();
                System.out.println(lenU);
                System.out.println(s.codePointCount(0, lenU));
        }   
}


```
