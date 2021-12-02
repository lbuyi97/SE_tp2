a)
Lo que se pide es que según las definiciones y directivas que se le dan al preprocesador.
Todo lo que esté dentro de una estructura como esta:
```
#ifdef X_MACRO
...
#endif
```


será compilado unicamente si X_MACRO ha sido definida.

También se admiten estructuras de este tipo:

```
#if X_MACRO == X_VALOR
...
#endif
```

en las cuales será compilado el bloque si X_MACRO está definido y vale X_VALOR.

De esta manera se pueden tener varios bloques de códigos (o programas enteros) dentro de un mismo archivo y "elegir" con un #define cuales de los bloques se quieren compilar.

Ejemplo:

![This is an image](./pre.png)


b) y c)
Se ve a continuación una captura del código del archivo 'tickHook.c'
```
#define TICKRATE_MS 10
#define LED_TOGGLE_MS 100
[...]
   tickConfig( TICKRATE_MS );

   /* Se agrega ademas un "tick hook" nombrado myTickHook. El tick hook es
      simplemente una funcion que se ejecutara peri­odicamente con cada
      interrupcion de Tick, este nombre se refiere a una funcion "enganchada"
      a una interrupcion.
      El segundo parametro es el parametro que recibe la funcion myTickHook
      al ejecutarse. En este ejemplo se utiliza para pasarle el led a titilar.
   */
   tickCallbackSet( myTickHook, (void*)LEDR );
   delay(LED_TOGGLE_MS);

   /* ------------- REPETIR POR SIEMPRE ------------- */
   while(1) {
      tickCallbackSet( myTickHook, (void*)LEDG );
      delay(LED_TOGGLE_MS);
[...]
```

Con las definiciones de 'TICKRATE_MS' y 'LED_TOGGLE_MS', se varían los tiempos del tick y del delay de los leds, cambiando los valores de los tiempos.
