### 1. Preprocesador
b. Comando ejecutado: "gcc -E hello2.c -o hello2.i". 
Al preprocesar el archivo hello2.c, observamos que el preprocesadro incluye todos los archivos de cabecera "stdio.h", los comentarios del archivo se eliminan y, además, los errores de escritura no son relevantes en esta etapa. Es decir, los errores sintácticos y semánticos no son importantes al momento de preprocesar un archivo.

d. La linea "int printf(const char * restrict s, ...)" es un prototipo de funcion. Indica al compilador que la función "printf" espera recibir al menos un argumento, que debe ser un puntero a una cadena de caracteres. Esta cadena de caracteres define el formato de salida para la función printf. Es decir, que con esta linea de código se le está indicando al compilador la información necesaria para llamar a printf correctamente desde cualquier parte del programa.

e. Comando ejecutado: "gcc -E hello3.c -o hello3.i".
La única diferencia entre hello3.i y hello3.c es que en hello3.i  se define en las primeras lineas las directivas del preprocesador, que indica la relacion entre el archivo original y el preprocesado. Son definidas con #. Vemos que los errores de semantica y sintaxis no son corregidos en esta etapa.  

### 2. Compilador
a. Comando ejecutado: "gcc -S hello3.c -o hello3.s". 
Observamos que no compila, dado que hay un error de sintaxis, le falta la llave final.

b. Comando ejecutado: "gcc -S hello4.c -o hello4.s". 
Al agregar la llave faltante, se nos genera el archivo hello4.s, a pesar de los warnings.

c. La función del archivo hello4.s es traducir el código del archivo original hello4.c a instrucciones en lenguaje ensamblador que la CPU puede entender y ejecutar. Es el paso intermedio antes de generar el código binario que se va a ejecutar.

d. Comando ejecutado: "gcc -c hello4.s -o hello4.o".
En este paso, ensamblamos el archivo hello4.s en hello4.o para luego vincular y generar el ejecutable. 

### 3. Vinculación
a. Comando ejecutado: "gcc hello4.o -o hello4".
Observamos que nos indica que hay un error, ya que "prontf" no está definido y no se puede generar el ejecutable. Se omite el otro error, de la falta de argumento para el "%d".

b. Comando ejecutado: "gcc hello5.c -o  hello5".
Al correjir el error, nos permite generar el ejecutable.

c.  Comando ejecutado: "./hello5".   
    Resultado: "La respuesta es 875242464".
Al correjir en hello5.c el error de escritura que existia en hello4.c de printf, vemos que podemos ejecutar el archivo sin ningun warning o error, pero nos estaría faltando indicarle el argumento correspondiente al "%d" en la llamada a printf y es por ello que el resultado que nos indica es un número basura.

### 4. Correccion de bugs
a. Comandos ejecutados: "gcc -S hello6.c -o hello6.s", "gcc -c hello6.s -o hello6.o", "gcc hello6.o -o hello6", "./hello6". 
Al corregir el error mencionado en el punto anterior, vemos que la salida es la esperada: "La respuesta es 42".

### 5. Remoción de prototipo
Comandos ejecutados: "gcc hello7.c -o  hello7", "./hello7".
Warning al generar el ejecutable: declaración implicita de la función 'printf'
Resultado luego de ejecutar: "La respuesta es 42".

i. Al compilar el archivo  hello7.c el código funciona pero el compilador nos tira un warning por la falta de un protitipo de función para "printf".

ii. Un prototipo de función es una declaracion de la función que especifica el tipo de retorno y de los argumentos. Permite al compilador realizar las verificaciones de tipo y saber como utlizar dicha función.

iii. Una declaración implicita de una función es cuando la función es utilizada sin haberse declarado previamente. 

iv. La especificación del lenguaje C nos indica que todas las funciones deben ser declaradas antes de utilizarse. El uso de funciones sin declaración explicita no es aconsejable y no es compatible con algunas versiones de C.

v. El "gcc" sigue las especificaciones del estándar del lenguaje C, compilando pero emitiendo warnings cuando se utilizan funciones sin antes haberlas declarado, como en este caso. En algunos casos, se tratan estos warnings como erroes.

vi. Una función built-in es una función que está directamente soportada por el compilador. Sin embargo, printf no es una función built-in ya que pertenece a la biblioteca estándar de C.

vii. "gcc" nos emite un warning ya que es peligroso el uso de una función sin declaracion previa: el compilador no puede verificar los argumentos ni el tipo de retorno de la función. Además, "gcc" no va en contra de la especificación por cumplir con las normativas emitiendo el warning, esto indica que se está realizando algo incorrecto.

En resumen, como observamos, el código compilará emitiendo un warning sobre la declaracion implicita de "printf", cumpliendo así con las normativas de las especifiaciones de C. Lo correcto sería incluir el archivo de cabecera "<stdio.h>" para proporcionar el prototipo de "printf" evitando los warnings.

### 6. Compilación separada: contratos y módulos
b.  Comandos ejecutados: "gcc -c studio1.c -o studio1.o", "gcc -c hello8.c -o hello8.o", "gcc studio1.o hello8.o -o hello8", "./hello8".
    Warnings: Al compilar studio1.c nos arroja un warning de declaración implícita de 'printf' dado que falta el header "<stdio.h>", y al compilar hello8.c nos arroja un warning de declaración implicita de la función 'prontf' dado que no estamos llamando al archivo de studio1.c donde está definida dicha función.
    Resultado: "La respuesta es 42".
Observamos que igualmente nos arroja el resultado esperado dado que al compilar estamos enlazando ambos archivos en el ejecutable final.

c. Si eliminamos o agregamos argumentos a la invocacion de 'prontf' el compilador generá un error o un warning por la diferencia que existe entre el número y el tipo de argumento que está esperando la función recibir proporcionados en la llamada. Esto se debe a que el compilador es quien realiza la verificacion de los argumentos dados a una funcion contra el prototipo que se declaró.

d. Al incluir el contrato en los clientes y en el proveedor, nos aseguramos que la declaración de 'prontf' sea la misma en la implementación y en los lugares donde se utiliza, previniendo erroes de tipo y número de argumentos. Además si 'prontf' cambiara, solo necesitamos actualizar el archivo header 'studio.h'. También de esta forma podemos detectar los problemas en tiempo de compilación, en lugar de tiempo de ejecución ya que el compilador verifica que coincida la declaracion de 'prontf' en los archivos cliente con la de 'studio.h'. Por último, mejora la modularidad y la reutilización de código.

### Crédito extra

- Las bibliotecas son archivos de códigos que se pueden utilizar por muchos programas. Permiten la reutilización de código y funcionalidades sin necesidad de reescribirlas desde cero, facilitando el proceso de codificación. Pueden ser bibliotecas estáticas, donde los archivos se enlazan al ejecutable en tiempo de compilación y las bibliotecas dinámicas que se enlazan en tiempo de ejecución.

- Se pueden distribuir, es decir, se puede compartir la biblioteca con otros desarrolladores para que puedan utilizarlas en sus programas. Por ejemplo, las bibliotecas estáticas se distribuyen como archivos '.lib' o '.a' mientras que las bibliotecas dinámicas como archivos '.dll' en windows.

- No todas las bibliotecas son portables, la portabilidad depende de varios factores. La portabilidad de una biblioteca refiere a la capacidad que tiene la misma de ser utilizada en diferentes sistemas operativos sin realizar grandes modificaciones. Las bibliotecas estándar, por ejemplo, son más portables ya que solo utilizan características estándar del lenguaje de programación. Mientras que las bibliotecas que dependen de características específicas del sistema operativo suelen tener problemas de portabilidad.

- Algunas de las ventajas de las bibliotecas son que permiten la reutilización de código en muchos programas, la          simplificación del desarrollo, facilitan el mantenimiento del código, favorecen a la modularidad y pueden reducir el tamaño de los ejecutables permitiendo actualizaciones sin recompilar todo el programa.
En desventajas podemos encontrar problemas de compatibilidad de versiones y conflictos de dependencias, la sobrecarga de rendimiento por la abstracción agregada, dificultades con la portabilidad en algunas ocasiones y restricciones legales que deben cumplirse.