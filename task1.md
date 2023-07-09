# Задача "Понимание JVM"

## Описание


```java
public class JvmComprehension {

// Система загрузки ищет класс в кэш Application ClassLoader->Platform ClassLoader->Bootstrap ClassLoader.
// Если ранее класс JvmComprehension не был загружен в кэш, он подгружается в Application ClassLoader,
// если на выходе класс не будет найден - программа не запутится (ошибка java.lang.ClassNotFoundException).

// Далее происходит связывание, подготовка классов к выполнению:
// проверка вадидности кода, подготовка примитивов в статических полях, связывание ссылок на другие классы.

// Далее мета-данные о JvmComprehension.class (имя, методы, поля и прочее) попадают в область памяти Metaspace.

    public static void main(String[] args) {
        int i = 1;                      // 1 создается фрейм main() в Stack, там хранится значение переменной 'i = 1'.
        Object o = new Object();        // 2 выделяеся память в Heap для new Object(), ссылка присваивается переменной 'o' во фрейме main(). 
        Integer ii = 2;                 // 3 аналогично п.2, Integer val=2 в Heap, 'ii' во фрейм main().
        printAll(o, i, ii);             // 4 в стеке новый фрейм printAll(), выделяется память под переменные о, ii, int i = 1.
        System.out.println("finished"); // 7 новый фрейм, "finished" в куче.
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // в фрейме printAll переменная uselessVar, в куче Integer = 700.
        System.out.println(o.toString() + i + ii);  // 6 в стеке новый фрейм println(), toString новый фрейм, ссылки на объекты во фреймах, объекты в куче.
    }
}

```
