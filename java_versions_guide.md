# نسخه‌ها و ورژن‌های جاوا

وقتی درباره نسخه‌های جاوا (Java Versions) صحبت می‌کنیم، منظور نسخه‌های
زمانی JDK/JRE است، نه ویراست‌هایی مانند Java SE یا Java EE.

-   Java SE, EE, ME = خانواده‌ها یا ویراست‌ها
-   Java 8, Java 11, Java 17, Java 21, Java 25 = نسخه‌های زمانی جاوا

------------------------------------------------------------------------

# Java 1.0 (1996)

اولین نسخه رسمی جاوا.

ویژگی‌ها:

-   JVM
-   Applet
-   AWT

امروزه تقریباً منسوخ شده است.

------------------------------------------------------------------------

# Java 1.2 (1998)

معروف به Java 2

ویژگی مهم:

-   Collections Framework

کلاس‌های مشهور:

``` java
ArrayList
HashMap
HashSet
```

------------------------------------------------------------------------

# Java 5 (2004)

یکی از مهم‌ترین نسخه‌های تاریخ جاوا.

ویژگی‌های اصلی:

## Generics

قبل:

``` java
List list = new ArrayList();
```

بعد:

``` java
List<String> list = new ArrayList<>();
```

## Enhanced For Loop

``` java
for(String s : list)
```

## Annotations

``` java
@Override
```

## Enum

``` java
enum Day
```

------------------------------------------------------------------------

# Java 6 (2006)

تمرکز اصلی:

-   افزایش سرعت
-   بهبود JVM
-   پایداری بیشتر

------------------------------------------------------------------------

# Java 7 (2011)

ویژگی‌ها:

## Try-with-Resources

``` java
try(FileInputStream f = ...) {
}
```

## Diamond Operator

``` java
List<String> list = new ArrayList<>();
```

------------------------------------------------------------------------

# Java 8 (2014)

مهم‌ترین نسخه تاریخ جاوا.

ویژگی‌ها:

## Lambda Expressions

``` java
x -> x * 2
```

## Streams API

``` java
list.stream()
```

## Functional Programming

## Optional

``` java
Optional<String>
```

## Date & Time API

``` java
LocalDate
LocalDateTime
```

------------------------------------------------------------------------

# Java 9 (2017)

ویژگی مهم:

## Module System (Project Jigsaw)

``` java
module-info.java
```

------------------------------------------------------------------------

# Java 10

ویژگی مهم:

## var

``` java
var name = "Aida";
```

------------------------------------------------------------------------

# Java 11 (LTS)

اولین نسخه LTS مدرن.

LTS = Long Term Support

ویژگی‌ها:

## HTTP Client جدید

``` java
HttpClient
```

## اجرای مستقیم فایل جاوا

``` bash
java Main.java
```

------------------------------------------------------------------------

# Java 12 تا Java 16

نسخه‌های میانی که بیشتر برای آماده‌سازی قابلیت‌های جدید منتشر شدند.

------------------------------------------------------------------------

# Java 17 (LTS)

یکی از مهم‌ترین نسخه‌های سازمانی.

ویژگی‌ها:

## Sealed Classes

``` java
sealed class Shape
```

## Pattern Matching

## امنیت بهتر

## عملکرد بهتر

------------------------------------------------------------------------

# Java 21 (LTS)

نسخه بسیار محبوب امروزی.

ویژگی‌ها:

## Virtual Threads (Project Loom)

قبل:

``` text
10000 Thread
=
مصرف حافظه زیاد
```

بعد:

``` text
1000000 Virtual Thread
```

## Record Patterns

## Pattern Matching پیشرفته

## Structured Concurrency

مناسب برای:

-   Backend
-   Microservices
-   Cloud Applications

------------------------------------------------------------------------

# Java 25 (LTS)

جدیدترین نسخه LTS.

تمرکز اصلی:

-   Performance
-   Cloud Native
-   GraalVM Integration
-   AI Workloads
-   بهبود Virtual Threads

------------------------------------------------------------------------

# جدول نسخه‌های مهم

  نسخه            سال انتشار   ویژگی شاخص
  --------------- ------------ -----------------------
  Java 5          2004         Generics و Annotation
  Java 7          2011         Try-with-Resources
  Java 8          2014         Lambda و Stream
  Java 11 (LTS)   2018         LTS مدرن
  Java 17 (LTS)   2021         Sealed Classes
  Java 21 (LTS)   2023         Virtual Threads
  Java 25 (LTS)   2025         جدیدترین LTS

------------------------------------------------------------------------

# مسیر پیشنهادی یادگیری در سال 2026

1.  Java 21 یا Java 25
2.  آشنایی با Java 8 برای پروژه‌های قدیمی
3.  یادگیری قابلیت‌های Java 17 و Java 21
4.  Spring Boot
5.  Hibernate
6.  Docker
7.  Microservices

------------------------------------------------------------------------

# خلاصه نهایی

  سؤال                             پاسخ
  -------------------------------- ---------------------------------
  Java SE چیست؟                    خانواده اصلی جاوا
  Java 21 چیست؟                    نسخه زمانی Java SE
  Java EE چیست؟                    پلتفرم سازمانی مبتنی بر Java SE
  LTS چیست؟                        نسخه با پشتیبانی بلندمدت
  بهترین نسخه برای شروع در 2026؟   Java 21 یا Java 25
  مهم‌ترین نسخه تاریخی؟             Java 8
