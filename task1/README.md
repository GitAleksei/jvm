# Задача "Понимание JVM"


## Код для исследования
```java
public class JvmComprehension {
    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }
    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}
```

0. В начале данные о классе JvmComprehension записываются в Metaspace. Создается фрейм main() в Stack Memory.
1. Переменая int i = 1 сохраняется во фрейме main() в Stack Memory
2. В heap выделяется пямять под объект Object, во фрейме main() создается указатель o на экземпляр Object.
3. В heap выделяется пямять под объект '2' класса Integer , во фрейме main() создается указатель ii на экземпляр '2'.
4. ClassLoader ищет метод printAll(,,), и находит его на уровне Application ClassLoader. В Stack Memory создается фрейм printAll(,,). Во фрейме printAll(,,) создаются указатели o, ii соответственно указывающие на те же объекты что и o, ii во фрейме main(). Так же во фрейме printAll(,,) создается примитив вида int i = 1.
5. В heap выделяется пямять под объект '700' класса Integer , во фрейме printAll(,,) создается указатель uselessVar на экземпляр '700'. 
6. ClassLoader ищет метод System.out.println() и находит его на уровне Bootstrap ClassLoader. В Stack Memory создается фрейм System.out.println(). В heap выделяется память под объект "o.toString() + i + ii" класса String. Во фрейме System.out.println() создается указатель x на объект "o.toString() + i + ii".
7. ClassLoader ищет метод System.out.println() и находит его на уровне Bootstrap ClassLoader. В Stack Memory создается фрейм System.out.println(). В heap выделяется память под объект "finished" класса String. Во фрейме System.out.println() создается указатель x на объект "finished".

Во время выполнения данной программы, сборщик мусора проверяет, стоит ли удалять объект из heap. Если он не достижим, то его удаляют в Stop The World паузе работы программы.