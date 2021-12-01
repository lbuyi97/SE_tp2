a)
Lo que se pide es que según las definiciones y directivas que se le dan al preprocesador.
Todo lo que esté dentro de una estructura como esta:

#ifdef X_MACRO
...
#endif

será compilado unicamente si X_MACRO ha sido definida.

También se admiten estructuras de este tipo:

#if X_MACRO == X_VALOR
...
#endif

en las cuales será compilado el bloque si X_MACRO está definido y vale X_VALOR.

De esta manera se pueden tener varios bloques de códigos (o programas enteros) dentro de un mismo archivo y "elegir" con un #define cuales de los bloques se quieren compilar.

Ejemplo:

![This is an image](/pre.png)
