

## Kotlin中常量和静态方法


>在kotlin中，常用companion object {}中用来修饰 静态常量，或者静态方法，单例等等


### 常量

Java中：

```java
   class StaticDemoActivity {
         public static final String LOAN_TYPE = "loanType";
         public static final String LOAN_TITLE = "loanTitle";
    }
```

Kotlin中：

```
  class StaticDemoActivity {
      companion object {
           val  LOAN_TYPE = "loanType"
           val  LOAN_TITLE = "loanTitle"
    }
}

或者

  class StaticDemoActivity {
      companion object StaticParams{
            val  LOAN_TYPE = "loanType"
            val  LOAN_TITLE = "loanTitle"
    }
}

 或者
  class StaticDemoActivity {
      companion object {
         const val LOAN_TYPE = "loanType"
         const val LOAN_TITLE = "loanTitle"
    }
}
```
注：const 关键字用来修饰常量，且只能修饰 val，不能修饰var, companion object 的名字可以省略，可以使用 Companion来指代

引用常量（这里的引用只针对于java引用kotlin代码）

TestEntity类引用StaticDemoActivity中的常量
```
   class TestEntity {
        public TestEntity () {
            String title = StaticDemoActivity.Companion.getLOAN_TITLE();
    }
  }

  或者

  class TestEntity {
        public TestEntity () {
            String title = StaticDemoActivity.StaticParams.getLOAN_TITLE();
        }
  }

  或者

  class TestEntity {
        public TestEntity () {
            String title = StaticDemoActivity.LOAN_TITLE;
            String type= StaticDemoActivity.LOAN_TYPE;
        }
  }
```

### 静态方法

Java代码：
```
      class StaticDemoActivity {
          public static void test(){
                、、、
          } 
      }
```
Kotlin中：
```
      class StaticDemoActivity {
          companion object {
               fun test(){
                    、、、
                }
          }
      }

  或者

       class StaticDemoActivity {
          companion object StaticParams{
              fun test() {
                  、、、
              }
          }
      }
```

引用静态方法（这里的引用只针对于java引用kotlin代码）

TestEntity类引用StaticDemoActivity中的静态方法

```
    class TestEntity {
          public TestEntity () {
                StaticDemoActivity.Companion.test();
          }
    }

或者

    class TestEntity {
          public TestEntity () {
             StaticDemoActivity.StaticParams.test();
          }
    }
```

