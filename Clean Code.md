# Tips para un Código Limpio ⌨️

## Tip 1 : No devolver Null (pg. 155)
### Definición: 
Este concepto insta a evitar devolver valores nulos (null) desde funciones o métodos. En su lugar, es preferible utilizar valores por defecto o excepciones para manejar casos en los que no se pueda producir un resultado válido.

Se debe evitar pasar null como parámetro de una llamada a un método o devolverlo como valor de retorno. Esto introduce complejidad adicional debido a las comprobaciones necesarias.

### Resultado: 
Evita errores de referencia nula (NullPointerException) y promueve un flujo de control más claro en el código, ya que los consumidores de la función no necesitan preocuparse por manejar valores nulos.

### Ejemplo Práctico: 
Imagina una función en Java que busca un elemento en una lista y devuelve nulo si no se encuentra:

```java
// Malo: Devolver nulo si el elemento no se encuentra
public String findElement(List<String> list, String target) {
    for (String item : list) {
        if (item.equals(target)) {
            return item;
        }
    }
    return null;
}

// Bueno: Lanzar una excepción si el elemento no se encuentra
public String findElement(List<String> list, String target) {
    for (String item : list) {
        if (item.equals(target)) {
            return item;
        }
    }
    throw new ElementNotFoundException("Elemento no encontrado: " + target);
}

```

### Explicación: 
En el ejemplo "Malo", la función devuelve nulo cuando no encuentra el elemento, lo que puede conducir a errores inesperados en el código que lo consume. En el ejemplo "Bueno", en su lugar se lanza una excepción específica que indica claramente el problema. Esto obliga al código que llama a manejar el caso de excepción de manera explícita, en lugar de tratar con valores nulos.

## Tip 2 : F.I.R.S.T. (Fast, Isolated/Independent, Repeatable, Self-validating, Timely) (pg. 182)
### Definición: 
El acrónimo F.I.R.S.T. representa un conjunto de características deseables para las pruebas unitarias. Estas características ayudan a crear pruebas efectivas y mantenibles que forman parte integral de un código limpio y de calidad.

#### F - Fast (Rápido):
El código limpio debe ser eficiente y rápido en su ejecución. Esto implica utilizar algoritmos eficientes y estructuras de datos adecuadas para evitar cuellos de botella y retrasos innecesarios.

#### I - Independent (Independiente):
Los componentes de tu código deben ser independientes unos de otros tanto como sea posible. Esto significa que un cambio en una parte del código no debería afectar a otras partes si no es necesario. La independencia entre módulos y componentes facilita la modificación y el mantenimiento.

#### R - Readable (Legible):
La legibilidad es crucial en el código limpio. Debe ser fácil de entender para cualquier persona que lo lea, incluido tu futuro yo. Utiliza nombres descriptivos para variables, funciones y clases, sigue una estructura de indentación coherente y agrega comentarios explicativos cuando sea necesario.

#### S - Small (Pequeño):
Las funciones y métodos deben ser pequeños y hacer una sola cosa. Siguiendo el principio de responsabilidad única, cada función o método debe tener una responsabilidad clara y específica. Esto hace que el código sea más fácil de entender, probar y mantener.

#### T - Testable (Probable):
El código limpio debe ser fácil de probar mediante pruebas unitarias y de integración. El diseño del código debe facilitar la creación de pruebas para verificar su funcionalidad. Las dependencias deben ser manejables y reemplazables, lo que permite realizar pruebas de manera efectiva.

Siguiendo estos principios "FIRST", puedes escribir código que sea más fácil de mantener, extender y depurar, lo que en última instancia mejora la calidad del software y reduce los problemas a lo largo del tiempo.

### Resultado: 
Las pruebas unitarias bien diseñadas mejoran la confiabilidad del código, permiten la detección temprana de errores y facilitan el proceso de desarrollo y mantenimiento.

### Prueba practica 

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class StringUtilsTest {

    @Test
    public void testConcatenateStrings() {
        String result = StringUtils.concatenate("Hello", "World");
        assertEquals("HelloWorld", result);
    }

    @Test
    public void testConcatenateEmptyStrings() {
        String result = StringUtils.concatenate("", "");
        assertEquals("", result);
    }

    @Test
    public void testReverseString() {
        String result = StringUtils.reverse("abc");
        assertEquals("cba", result);
    }
}

```

### Explicación 
En este ejemplo, estamos probando una clase ficticia StringUtils que contiene métodos para concatenar y revertir cadenas. Las pruebas unitarias están escritas utilizando el framework de pruebas JUnit 5.

Fast: Las pruebas se ejecutan rápidamente, lo que permite una rápida retroalimentación durante el desarrollo.
Isolated/Independent: Cada prueba es independiente y no depende del resultado de otras pruebas. Cada método de prueba se ejecuta en su propio contexto.
Repeatable: Las pruebas se pueden ejecutar en diferentes entornos y siempre darán los mismos resultados si el código no cambia.
Self-validating: Las pruebas son automáticas; simplemente se ejecutan y reportan si algo falla sin necesidad de inspección manual.
Timely: Las pruebas se escriben al mismo tiempo que el código que están probando, lo que garantiza que cualquier cambio se pruebe inmediatamente.

## Tip 3:  Reglas 2 a 4 del diseño sencillo: Refactorizar (pg. 228)
### Definición

 La refactorización es el proceso de reestructurar el código existente sin cambiar su comportamiento externo. Implica mejorar la estructura interna del código para hacerlo más legible, mantenible y eficiente, sin alterar su funcionalidad observable.

 Importancia: La refactorización permite eliminar duplicación, mejorar la claridad del código y adaptarlo a cambios futuros. Al realizar refactorizaciones constantes, el código mantiene su calidad a lo largo del tiempo.

### Resultado:
 Código más limpio, menos redundante y más fácil de entender y mantener. También ayuda a prevenir la acumulación de deuda técnica.

### Ejemplo Práctico: 

```java

import java.util.ArrayList;
import java.util.List;

public class RefactoringExample {

    // Regla 2: Refactorizar
    // Código antes de la refactorización
    public double calculateAverage(List<Integer> numbers) {
        double sum = 0;
        for (int num : numbers) {
            sum = sum + num;
        }
        double average = sum / numbers.size();
        return average;
    }

    // Código después de la refactorización
    public double calculateAverageRefactored(List<Integer> numbers) {
        double sum = sum(numbers);
        double average = sum / numbers.size();
        return average;
    }

    // Regla 3: Pequeñas Refactorizaciones
    // Dividir la función en partes más pequeñas
    private double sum(List<Integer> numbers) {
        double sum = 0;
        for (int num : numbers) {
            sum = sum + num;
        }
        return sum;
    }

    // Regla 4: Pruebas
    public static void main(String[] args) {
        RefactoringExample example = new RefactoringExample();
        
        List<Integer> numbers = new ArrayList<>();
        numbers.add(10);
        numbers.add(20);
        numbers.add(30);
        
        double average = example.calculateAverage(numbers);
        System.out.println("Average: " + average);

        double averageRefactored = example.calculateAverageRefactored(numbers);
        System.out.println("Average Refactored: " + averageRefactored);
    }
}
```
## Cita Relevante ✨


>"Si deseas ir rápido, si quieres terminar rápidamente, si quieres que tu código sea fácil de escribir, hazlo fácil de leer."
 — Robert C. Martin, "Clean Code"

Esta cita resalta la importancia de escribir código que sea fácil de leer y comprender, incluso si tu objetivo principal es escribirlo rápidamente. Es decir, aunque tu enfoque sea en la velocidad y eficiencia al programar, es crucial asegurarse de que el código resultante sea claro y comprensible. Aquí hay una explicación más detallada:

#### Reducción de la Complejidad:
 El código claro tiende a ser menos complejo, lo que facilita comprender la lógica y el flujo del programa. Esto puede resultar en menos errores y menos casos de comportamiento no deseado.

#### Ahorro a Largo Plazo: 
Aunque escribir un código limpio y claro puede llevar un poco más de tiempo inicialmente, esto se compensa a largo plazo. A medida que el proyecto evoluciona, el tiempo ahorrado en depuración, mantenimiento y comprensión del código supera el tiempo dedicado a escribirlo.

#### Documentación:
 El código bien escrito a menudo actúa como su propia documentación. Cuando el código es claro y legible, se convierte en un recurso valioso para comprender cómo funciona el programa.

 #### Refactorización Más Sencilla:
  La refactorización, es decir, mejorar la estructura interna del código sin cambiar su comportamiento externo, se vuelve más simple cuando el código es fácil de leer. Los desarrolladores pueden hacer cambios con confianza, sabiendo que pueden entender el impacto.

