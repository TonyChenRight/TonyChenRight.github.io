---
layout: post
title: 'Apache-Commons库详解（2）--Commons Lang'
subtitle: 'Apache-Commons库详解（2）--Commons Lang'
date: 2018-07-14
categories: java
cover: 'https://commons.apache.org/images/commons-logo.png'
tags: java apache
---

*Commons Lang* 跟java.lang这个包的作用类似，Commons Lang这一组API也是提供一些基础的、通用的操作和处理，如自动生成toString()的结果、自动实现hashCode()和equals()方法、数组操作、枚举、日期和时间的处理等等。目前这组API的版本是2.1，下载地址如下：

[http://apache.justdn.org/jakarta/commons/lang/binaries/commons-lang-2.1.zip](http://apache.justdn.org/jakarta/commons/lang/binaries/commons-lang-2.1.zip)

[http://apache.justdn.org/jakarta/commons/lang/source/commons-lang-2.1-src.zip](http://apache.justdn.org/jakarta/commons/lang/source/commons-lang-2.1-src.zip)

其中后一个是源代码。

这一组API的所有包名都以org.apache.commons.lang开头，共有如下8个包：

>* org.apache.commons.lang

>* org.apache.commons.lang.builder

>* org.apache.commons.lang.enum

>* org.apache.commons.lang.enums

>* org.apache.commons.lang.exception

>* org.apache.commons.lang.math

>* org.apache.commons.lang.mutable

>* org.apache.commons.lang.time

其中的lang.enum已不建议使用，替代它的是紧随其后的lang.enums包。 lang包主要是一些可以高度重用的Util类；lang.builder包包含了一组用于产生每个Java类中都常使用到的toString()、hashCode()、equals()、compareTo()等等方法的构造器；lang.enums包顾名思义用于处理枚举；lang.exception包用于处理Java标准API中的exception，为1.4之前版本提供Nested Exception功能；lang.math包用于处理数字；lang.mutable用于包装值型变量；lang.time包提供处理日期和时间的功能。

由于Commons的包和类实在很多，不可能一个一个讲了，在接下来的专题文章中我就只分别过一下lang、lang.builder、lang.math和lang.time这几个包和常见的用法，其他的我们可以在用到时临时参考一下Javadoc。位置就在安装路径的

…\commons-lang-2.1\docs\api\index.html

我们首先来看org.apache.commons.lang包，这个包提供了一些有用的包含static方法的Util类。除了6个Exception类和2个已经deprecated的数字类之外，commons.lang包共包含了17个实用的类：

>* ArrayUtils – 用于对数组的操作，如添加、查找、删除、子数组、倒序、元素类型转换等；

>* BitField – 用于操作位元，提供了一些方便而安全的方法；

>* BooleanUtils – 用于操作和转换 boolean 或者 Boolean 及相应的数组；

>* CharEncoding – 包含了 Java 环境支持的字符编码，提供是否支持某种编码的判断；

>* CharRange – 用于设定字符范围并做相应检查；

>* CharSet – 用于设定一组字符作为范围并做相应检查；

>* CharSetUtils – 用于操作 CharSet ；

>* CharUtils – 用于操作 char 值和 Character 对象；

>* ClassUtils – 用于对 Java 类的操作，不使用反射；

>* ObjectUtils – 用于操作 Java 对象，提供 null 安全的访问和其他一些功能；

>* RandomStringUtils – 用于生成随机的字符串；

>* SerializationUtils – 用于处理对象序列化，提供比一般 Java 序列化更高级的处理能力；

>* StringEscapeUtils – 用于正确处理转义字符，产生正确的 Java 、 JavaScript 、 HTML 、 XML 和 SQL 代码；

>* StringUtils – 处理 String 的核心类，提供了相当多的功能；

>* SystemUtils – 在 java.lang.System 基础上提供更方便的访问，如用户路径、 Java 版本、时区、操作系统等判断；

>* Validate – 提供验证的操作，有点类似 assert 断言；

>* WordUtils – 用于处理单词大小写、换行等。

>* 接下来我准备用两个例子来分别说明ArrayUtils和StringUtils的常见用法

#### org.apache.commons.lang.ArrayUtils

数组是我们经常需要使用到的一种数据结构，但是由于Java本身并没有提供很好的API支持，使得很多操作实际上做起来相当繁琐，以至于我们实际编码中甚至会不惜牺牲性能去使用Collections API，用Collection当然能够很方便的解决我们的问题，但是我们一定要以性能为代价吗？ArrayUtils帮我们解决了处理类似情况的大部分问题。来看一个例子：

```java
package sean.study.jakarta.commons.lang;

import java.util.Map;

import org.apache.commons.lang.ArrayUtils;

public class ArrayUtilsUsage {

    public static void main(String[] args) {

        // data setup

        int [] intArray1 = { 2, 4, 8, 16 };

        int [][] intArray2 = { { 1, 2 }, { 2, 4 }, { 3, 8}, { 4, 16 } };

        Object[][] notAMap = {

                { "A" , new Double(100) },

                { "B" , new Double(80) },

                { "C" , new Double(60) },

                { "D" , new Double(40) },

                { "E" , new Double(20) }

        };

        // printing arrays

        System.out.println( "intArray1: " +ArrayUtils.toString(intArray1));

        System.out.println( "intArray2: " +ArrayUtils.toString(intArray2));

        System.out.println( "notAMap: " +ArrayUtils.toString(notAMap));

        // finding items

        System.out.println( "intArray1 contains '8'? "

                + ArrayUtils.contains(intArray1,8));

        System.out.println( "intArray1 index of '8'? "

                + ArrayUtils.indexOf(intArray1,8));

        System.out.println( "intArray1 last index of '8'? "

                +ArrayUtils.lastIndexOf(intArray1, 8));

        // cloning and resversing

        int[] intArray3 =ArrayUtils.clone(intArray1);

        System.out.println( "intArray3: " +ArrayUtils.toString(intArray3));

        ArrayUtils.reverse(intArray3);

        System.out.println( "intArray3 reversed: "

                +ArrayUtils.toString(intArray3));

        // primitive to Object array

        Integer[] integerArray1 =ArrayUtils.toObject(intArray1);

        System.out.println( "integerArray1: "

                +ArrayUtils.toString(integerArray1));

        // build Map from two dimensional array

        Map map = ArrayUtils.toMap(notAMap);

        Double res = (Double) map.get( "C" );

        System.out.println( "get 'C' from map: " + res);

    }

}
```

以下是运行结果：
```
intArray1:{2,4,8,16}

intArray2:{ {1,2},{2,4},{3,8},{4,16}}

notAMap:{ {A,100.0},{B,80.0},{C,60.0},{D,40.0},{E,20.0}}

intArray1 contains'8'? true

intArray1 index of'8'? 2

intArray1 last indexof '8'? 2

intArray3:{2,4,8,16}

intArray3 reversed:{16,8,4,2}

integerArray1:{2,4,8,16}

get 'C' from map:60.0
```

这段代码说明了我们可以如何方便的利用ArrayUtils类帮我们完成数组的打印、查找、克隆、倒序、以及值型/对象数组之间的转换等操作。如果想了解更多，请参考Javadoc。

### org.apache.commons.lang.StringUtils

处理文本对Java应用来说应该算是家常便饭了，在1.4出现之前，Java自身提供的API非常有限，如String、StringTokenizer、StringBuffer，操作也比较单一。无非就是查找substring、分解、合并等等。到1.4的出现可以说Java的文字处理上了一个台阶，因为它支持regular expression了。这可是个重量级而方便的东东啊，缺点是太复杂，学习起来有一定难度。相较而言，Jakarta Commons提供的StringUtils和WordUtils至今还维持着那种简洁而强大的美，使用起来也很顺手。来看一个例子：
```java
package sean.study.jakarta.commons.lang;

import org.apache.commons.lang.StringUtils;

public class StringUtilsAndWordUtilsUsage {

    public static void main(String[] args) {

        // data setup

        String str1 = "" ;

        String str2 = "" ;

        String str3 = "\t" ;

        String str4 = null ;

        String str5 = "123" ;

        String str6 = "ABCDEFG" ;

        String str7 = "Itfeels good to use JakartaCommons.\r\n" ;

        // check for empty strings

        System.out.println( "==============================" );

        System.out.println( "Is str1 blank? " +StringUtils.isBlank(str1));

        System.out.println( "Is str2 blank? " +StringUtils.isBlank(str2));

        System.out.println( "Is str3 blank? " +StringUtils.isBlank(str3));

        System.out.println( "Is str4 blank? " +StringUtils.isBlank(str4));

        // check for numerics

        System.out.println( "==============================" );

        System.out.println( "Is str5 numeric? " +StringUtils.isNumeric(str5));

        System.out.println( "Is str6 numeric? " +StringUtils.isNumeric(str6));

        // reverse strings / whole words

        System.out.println( "==============================" );

        System.out.println( "str6: " + str6);

        System.out.println( "str6reversed: " + StringUtils.reverse(str6));

        System.out.println( "str7: " + str7);

        String str8 = StringUtils.chomp(str7);

        str8 =StringUtils.reverseDelimited(str8, ' ' );

        System.out.println( "str7 reversed whole words : \r\n" + str8);

        // build header (useful to print logmessages that are easy to locate)

        System.out.println( "==============================" );

        System.out.println( "print header:" );

        String padding = StringUtils.repeat( "=" , 50);

        String msg = StringUtils.center( " Customised Header " , 50, "%" );

        Object[] raw = new Object[]{padding, msg,padding};

        String header = StringUtils.join(raw, "\r\n" );

        System.out.println(header);

    }

}
```
输出的结果如下：
```
==============================

Is str1 blank? true

Is str2 blank? true

Is str3 blank? true

Is str4 blank? true

==============================

Is str5 numeric?true

Is str6 numeric?false

==============================

str6: ABCDEFG

str6 reversed:GFEDCBA

str7: It feels goodto use Jakarta Commons.

str7 reversed wholewords :

Commons. Jakarta use to good feelsIt

==============================

print header:

==================================================

%%%%%%%%%%%%%%%Customised Header %%%%%%%%%%%%%%%%

==================================================
```
从代码中我们可以大致了解到这个StringUtils类简单而强大的处理能力，从检查空串（对null的情况处理很得体），到分割子串，到生成格式化的字符串，使用都很简洁，也很直截了当。

### org.apache.commons.lang.builder
在前面的专题文章中，我们一起过了一遍org.apache.commons.lang包，接下来我们继续看org.apache.commons.lang.builder这个包。在这里面我们可以找到7个类，用于帮助我们实现Java对象的一些基础的共有方法。这7个类分别是：
>* CompareToBuilder – 用于辅助实现 Comparable.compareTo(Object) 方法；

>* EqualsBuilder – 用于辅助实现 Object.equals() 方法；

>* HashCodeBuilder – 用于辅助实现 Object.hashCode() 方法；

>* ToStringBuilder – 用于辅助实现 Object.toString() 方法；

>* ReflectionToStringBuilder – 使用反射机制辅助实现 Object.toString() 方法；

>* ToStringStyle – 辅助 ToStringBuilder 控制输出格式；

>* StandardToStringStyle – 辅助 ToStringBuilder 控制标准格式。

我们知道，在实际应用中，其实经常需要在运行过程中判定对象的知否相等、比较、取hash、和获取对象基本信息（一般是产生log日志）。然而实现这些compareTo、equals、hashCode、toString其实并非那么直截了当，甚至稍有不注意就可能造成难以追踪的bug，而且这些方法手工维护的话，比较繁琐，也容易出错。于是Commons Lang在builder这个包中提供了上述辅助类，为我们简化这些方法的实现和维护。

来看一个例子：
```java
package sean.study.jakarta.commons.lang;

import java.util.Date;

import org.apache.commons.lang.builder.CompareToBuilder;

import org.apache.commons.lang.builder.EqualsBuilder;

import org.apache.commons.lang.builder.HashCodeBuilder;

import org.apache.commons.lang.builder.ToStringBuilder;

import org.apache.commons.lang.builder.ToStringStyle;

public class BuilderUsage {

    public static void main(String[] args) {

        Staff staff1 = new Staff(123, "John Smith" , new Date());

        Staff staff2 = new Staff(456, "Jane Smith" , new Date());

        System.out.println( "staff1's info: " + staff1);

        System.out.println( "staff2's info: " + staff2);

        System.out.println( "staff1's hash code: " +staff1.hashCode());

        System.out.println( "staff2's hash code: " +staff2.hashCode());

        System.out.println( "staff1 equals staff2? " +staff1.equals(staff2));

    }

}

class Staff implements Comparable {

    private long staffId;

    private String staffName;

    private Date dateJoined;

    public Staff() {

    }

    public Staff( long staffId, String staffName, Date dateJoined){

        this.staffId = staffId;

        this.staffName = staffName;

        this.dateJoined = dateJoined;

    }

    public int compareTo(Object o) {

        int res = -1;

        if (o != null &&Staff.class.isAssignableFrom(o.getClass())) {

            Staff s = (Staff) o;

            res = new CompareToBuilder()

                    .append(dateJoined,s.getDateJoined())

                    .append(staffName,s.getStaffName()).toComparison();

        }

        return res;

    }

    public boolean equals(Object o) {

        boolean res = false;

        if (o != null &&Staff.class.isAssignableFrom(o.getClass())) {

            Staff s = (Staff) o;

            res = new EqualsBuilder()

                    .append(staffId,s.getStaffId())

                    .isEquals();

        }

        return res;

    }

    public int hashCode() {

        return new HashCodeBuilder(11,23).append(staffId).toHashCode();

    }

    public String toString() {

        return new ToStringBuilder( this ,ToStringStyle.MULTI_LINE_STYLE)

                .append( "staffId" ,staffId)

                .append( "staffName" ,staffName)

                .append( "dateJoined" ,dateJoined)

                .toString();

    }

    public Date getDateJoined() {

        return dateJoined;

    }

    public void setDateJoined(Date dateJoined) {

        this .dateJoined = dateJoined;

    }

    public long getStaffId() {

        return staffId;

    }

    public void setStaffId(long staffId) {

        this .staffId = staffId;

    }

    public String getStaffName() {

        return staffName;

    }

    public void setStaffName(String staffName) {

        this .staffName = staffName;

    }

}
```
以下是运行结果：
```
staff1's info:sean.study.jakarta.commons.lang.Staff@190d11[

  staffId=123

  staffName=John Smith

  dateJoined=Sat Jul 30 13:18:45 CST 2005

]

staff2's info:sean.study.jakarta.commons.lang.Staff@1fb8ee3[

  staffId=456

  staffName=Jane Smith

  dateJoined=Sat Jul 30 13:18:45 CST 2005

]

staff1's hash code:376

staff2's hash code:709

staff1 equals staff2? false
```
这些builder使用起来都很简单，new一个实例，append需要参与的信息，最后加上toComparison、isEquals、toHashCode、toString这样的结尾即可。相应的，如果你不需要这样级别的控制，也可以使用利用反射机制的版本自动化实现需要的功能，如：
```java
 public int compareTo(Object o) {

        return CompareToBuilder.reflectionCompare( this , o);

    }

    public boolean equals(Object o) {

        return EqualsBuilder.reflectionEquals( this , o);

    }

    public int hashCode() {

        return HashCodeBuilder.reflectionHashCode( this );

    }

    public String toString() {

        return ReflectionToStringBuilder.toString( this );

    }
```
尤其当我们在项目中不希望过多的参与到对这些对象方法的维护时，采用Commons提供的利用反射的这些API就成了方便而相对安全的一个方案。

### org.apache.commons.lang.math
在Jakarta Commons中，专门处理数学计算的类分别可以在两个地方找到：一是Commons Lang的org.apache.commons.lang.math包中，二是在Commons Math这个单独的子项目中。由于后者主要是处理复数、矩阵等，相对使用比较少，在我的笔记中就只简单讲讲Commons Lang中的math包。对后者感兴趣的可以看看

[http://jakarta.apache.org/commons/math/](http://jakarta.apache.org/commons/math/)

org.apache.commons.lang.math包中共有10个类，这些类可以归纳成四组：
>* 1- 处理分数的 Fraction 类；

>* 2- 处理数值的 NumberUtils 类；

>* 3- 处理数值范围的 Range 、 NumberRange 、 IntRange 、 LongRange 、 FloatRange 、 DoubleRange 类；

>* 4- 处理随机数的 JVMRandom 和 RandomUtils 类。
下面我举个例子分别说明上述四组的使用方法：

```java
package sean.study.jakarta.commons.lang;

import org.apache.commons.lang.ArrayUtils;

import org.apache.commons.lang.BooleanUtils;

import org.apache.commons.lang.StringUtils;

import org.apache.commons.lang.math.DoubleRange;

import org.apache.commons.lang.math.Fraction;

import org.apache.commons.lang.math.NumberUtils;

import org.apache.commons.lang.math.RandomUtils;

import org.apache.commons.lang.math.Range;

public class LangMathUsage {

    public static void main(String[] args) {

        demoFraction();

        demoNumberUtils();

        demoNumberRange();

        demoRandomUtils();

    }

    public static void demoFraction() {

        System.out.println(StringUtils.center( " demoFraction " , 30, "=" ));

        Fraction myFraction = Fraction.getFraction(144,90);

        // FractionmyFraction = Fraction.getFraction("1 54/90");

        System.out.println( "144/90 as fraction: " + myFraction);

        System.out.println( "144/90 to proper: " +myFraction.toProperString());

        System.out.println( "144/90 as double: " +myFraction.doubleValue());

        System.out.println( "144/90 reduced: " + myFraction.reduce());

        System.out.println( "144/90 reduced proper: "

                +myFraction.reduce().toProperString());

        System.out.println();

    }

    public static void demoNumberUtils() {

        System.out.println(StringUtils.center( " demoNumberUtils " , 30, "=" ));

        System.out.println( "Is 0x3Fa number? "

                +StringUtils.capitalize(BooleanUtils.toStringYesNo(NumberUtils

                        .isNumber( "0x3F" )))+ "." );

        double [] array = { 1.0, 3.4, 0.8, 7.1, 4.6 };

        double max = NumberUtils.max(array);

        double min = NumberUtils.min(array);

        String arrayStr =ArrayUtils.toString(array);

        System.out.println( "Max of " + arrayStr + " is: " + max);

        System.out.println( "Min of " + arrayStr + " is: " + min);

        System.out.println();

    }

    public static void demoNumberRange() {

        System.out.println(StringUtils.center( " demoNumberRange " , 30, "=" ));

        Range normalScoreRange = newDoubleRange(90, 120);

        double score1 = 102.5;

        double score2 = 79.9;

        System.out.println( "Normal score rangeis: " + normalScoreRange);

        System.out.println( "Is "

                + score1

                + "a normal score? "

                + StringUtils

                        .capitalize(BooleanUtils.toStringYesNo(normalScoreRange

                                .containsDouble(score1)))+ "." );

        System.out.println( "Is "

                + score2

                + "a normal score? "

                + StringUtils

                        .capitalize(BooleanUtils.toStringYesNo(normalScoreRange

                                .containsDouble(score2)))+ "." );

        System.out.println();

    }

    public static void demoRandomUtils() {

        System.out.println(StringUtils.center( " demoRandomUtils " , 30, "=" ));

        for (int i = 0; i < 5; i++) {

            System.out.println(RandomUtils.nextInt(100));

        }

        System.out.println();

    }

}
```
以下是运行结果：
```
========demoFraction ========

144/90 as fraction:144/90

144/90 to proper: 154/90

144/90 as double:1.6

144/90 reduced: 8/5

144/90 reducedproper: 1 3/5

======demoNumberUtils =======

Is 0x3F a number? Yes.

Max of{1.0,3.4,0.8,7.1,4.6} is: 7.1

Min of{1.0,3.4,0.8,7.1,4.6} is: 0.8

======demoNumberRange =======

Normal score rangeis: Range[90.0,120.0]

Is 102.5 a normal score? Yes.

Is 79.9 a normal score? No.

======demoRandomUtils =======

75

63

40

21

92
```
以上就是Commons Lang的math包通常的用法。提一下NumberUtils.inNumber(String)方法，它会正确判断出给定的字符串是否是合法的数值，而我在前面的笔记中提到的StringUtils.isNumeric(String)只能判断一个字符串是否是由纯数字字符组成。Commons Lang的math包的各个类都还有很多实用的方法，远不止我在例子中用到的这些，如果你感兴趣，参照一下Commons Lang的Javadoc吧。

### org.apache.commons.lang.time
好了，来看我在Common Lang中最后要讲的一个包：org.apache.commons.lang.time。这个包里面包含了如下5个类：

>* DateFormatUtils – 提供格式化日期和时间的功能及相关常量；

>* DateUtils – 在 Calendar 和 Date 的基础上提供更方便的访问；

>* DurationFormatUtils – 提供格式化时间跨度的功能及相关常量；

>* FastDateFormat – 为 java.text.SimpleDateFormat 提供一个的线程安全的替代类；

>* StopWatch – 是一个方便的计时器。

我们还是在一个例子中来看上述各个类的用法吧：
```java
package sean.study.jakarta.commons.lang;

import java.util.Calendar;

import java.util.Date;

import org.apache.commons.lang.StringUtils;

import org.apache.commons.lang.time.DateFormatUtils;

import org.apache.commons.lang.time.DateUtils;

import org.apache.commons.lang.time.FastDateFormat;

import org.apache.commons.lang.time.StopWatch;

public class DateTimeUsage {

    public static void main(String[] args) {

        demoDateUtils();

        demoStopWatch();

    }

    public static void demoDateUtils() {

        System.out.println(StringUtils.center( " demoDateUtils " , 30, "=" ));

        Date date = new Date();

        String isoDateTime =DateFormatUtils.ISO_DATETIME_FORMAT.format(date);

        String isoTime =DateFormatUtils.ISO_TIME_NO_T_FORMAT.format(date);

        FastDateFormat fdf = FastDateFormat.getInstance( "yyyy-MM" );

        String customDateTime =fdf.format(date);

        System.out.println( "ISO_DATETIME_FORMAT: " + isoDateTime);

        System.out.println( "ISO_TIME_NO_T_FORMAT: " + isoTime);

        System.out.println( "Custom FastDateFormat: " +customDateTime);

        System.out.println( "Default format: " + date);

        System.out.println( "Round HOUR: " + DateUtils.round(date,Calendar.HOUR));

        System.out.println( "Truncate HOUR: " +DateUtils.truncate(date, Calendar.HOUR));

        System.out.println();

    }

    public static void demoStopWatch() {

        System.out.println(StringUtils.center( " demoStopWatch " , 30, "=" ));

        StopWatch sw = new StopWatch();

        sw.start();

        operationA();

        sw.stop();

        System.out.println( "operationA used " + sw.getTime() + " milliseconds." );

        System.out.println();

    }

    public static void operationA() {

        try {

            Thread.sleep(999);

        }

        catch (InterruptedException e) {

            // do nothing

        }

    }

}
```
以下是运行结果：
```
=======demoDateUtils ========

ISO_DATETIME_FORMAT:2005-08-01T12:41:51

ISO_TIME_NO_T_FORMAT:12:41:51

CustomFastDateFormat: 2005-08

Default format: MonAug 01 12:41:51 CST 2005

Round HOUR: Mon Aug01 13:00:00 CST 2005

Truncate HOUR: MonAug 01 12:00:00 CST 2005

=======demoStopWatch ========

operationA used 1000milliseconds.
```
具体的调用细节和完整的API请参阅Commons Lang的Javadoc。