# Java Bytecode Execution: Interpreter vs JIT Compiler

## مقدمه

یکی از سوالات رایج هنگام یادگیری JVM این است:

> آیا Java Bytecode به صورت مفسری (Interpreted) اجرا می‌شود یا کامپایلری (Compiled)؟

پاسخ کوتاه: JVMهای مدرن از هر دو روش استفاده می‌کنند.

## فرآیند کامل اجرای برنامه

```text
Java Source Code
      ↓
     javac
      ↓
   Bytecode
      ↓
 Interpreter
      ↓
 JIT Compiler
      ↓
 Native Machine Code
```

## مرحله 1: کامپایل سورس کد

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```

```bash
javac Main.java
```

خروجی:

```text
Main.class
```

## Bytecode چیست؟

Bytecode یک نمایش میانی (Intermediate Representation) است که:

- مستقل از سیستم عامل است.
- مستقل از معماری CPU است.
- روی هر JVM قابل اجراست.

شعار معروف جاوا:

> Write Once, Run Anywhere

## مرحله 2: بارگذاری توسط JVM

هنگام اجرای:

```bash
java Main
```

JVM مراحل زیر را انجام می‌دهد:

1. Class Loading
2. Verification
3. Linking
4. Initialization

## اجرای تفسیری (Interpreter)

Interpreter دستورات Bytecode را یکی‌یکی می‌خواند و اجرا می‌کند.

نمونه Bytecode:

```text
iload_1
iload_2
iadd
istore_3
```

مزیت:

- شروع سریع برنامه

عیب:

- اجرای تکراری کدها کندتر است.

## JIT Compiler

JIT مخفف Just-In-Time Compiler است.

ایده اصلی:

بخش‌هایی از برنامه که زیاد اجرا می‌شوند (Hot Methods) به کد ماشین تبدیل شوند.

مثال:

```java
for(int i=0;i<1000000;i++){
    calculate();
}
```

متد `calculate()` به عنوان Hot Method شناسایی می‌شود.

### فرآیند

```text
Bytecode
   ↓
Interpreter
   ↓
JIT Compiler
   ↓
Native Code
```

## مزایای JIT

- سرعت بیشتر
- استفاده از اطلاعات زمان اجرا
- بهینه‌سازی پویا

## Tiered Compilation

در HotSpot JVM چند سطح اجرا وجود دارد:

```text
Level 0 → Interpreter
Level 1 → C1 Compiler
Level 4 → C2 Compiler
```

هدف:

- Startup سریع
- Performance بالا

## C1 و C2

### C1 Compiler

- کامپایل سریع
- بهینه‌سازی کمتر

### C2 Compiler

- کامپایل کندتر
- بهینه‌سازی پیشرفته‌تر

مسیر معمول:

```text
Interpreter
    ↓
C1
    ↓
C2
```

## Code Cache

کدهای تولیدشده توسط JIT در حافظه‌ای به نام Code Cache ذخیره می‌شوند.

## AOT Compilation

Ahead Of Time Compilation:

```bash
native-image MyApp
```

در GraalVM می‌تواند قبل از اجرا فایل Native تولید کند.

### مقایسه JIT و AOT

| ویژگی | JIT | AOT |
|---------|---------|---------|
| زمان کامپایل | هنگام اجرا | قبل از اجرا |
| Startup | کندتر | سریع‌تر |
| Runtime Optimization | دارد | ندارد یا محدود |
| انعطاف‌پذیری | بیشتر | کمتر |

## نتیجه‌گیری

Java Bytecode نه صرفاً مفسری اجرا می‌شود و نه صرفاً کامپایلری.

JVMهای مدرن از مدل Hybrid استفاده می‌کنند:

1. اجرای اولیه توسط Interpreter
2. شناسایی Hot Methods
3. کامپایل توسط JIT
4. اجرای Native Code

بنابراین عملکرد جاوا ترکیبی از سرعت شروع مناسب و کارایی بالای زمان اجراست.
