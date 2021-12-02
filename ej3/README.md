a)
Para compilación condicional se utilizan macros de la siguiente forma:
```
#ifdef X_MACRO
...
#endif
```
También se admiten estructuras de este tipo:

```
#if X_MACRO == X_VALOR
...
#endif
```

De este modo se va a compilar solo el código encerrado por ese #ifdef si se define anteriormente X_MACRO.


Ejemplo:

![This is an image](./pre.png)

b) A continuación se presenta una tabla de las funciones usadas en tickHook.c.
|Función|Descripción de lo que hace|
|:------------------------------------------------------------|:---------------------------------------------|
|boardConfig()|Macro definida en sapi_board.h como boardInit() (la cual está en sapi_board.c), es decir, hace lo mismo que esta función. Prácticamente es la incialización del system core, cycles counter y puertos dependiendo de la placa utilizada.|
|bool_t tickConfig(tick_t tickRateMSvalue )|Macro definida en sapi_tick.h como tickInit() (en sapi_tick.c). Realiza la inicialización del tick (timer dentro del core) y lo configura con un tick rate de tickRateMSvalue. Devuelve false en el caso de que se quiera usar tickInit() con FreeRTOS.|
|bool_t tickCallbackSet( callBackFuncPtr_t tickCallback, void* tickCallbackParams)|Función definida en sapi_tick.c, usada para setear la función que se ejecuta cada vez que el tick realiza una interrupción (tickHookFunction) como tickCallback con parámetros tickCallbackParams. Devuelve false si se quiere ejecutar con FreeRTOS o si tickCalback o tickCallBackParams son NULL|
|void myTickHook( void *ptr )|Función ejecutada cada vez que se produce un tick, enciende el puerto indicado por *ptr. La función está definida dentro de tickHook.c. En este caso se pasa esta función a tickCallBackSet para que la ponga como función a ejecutar en cada interrupción.|
