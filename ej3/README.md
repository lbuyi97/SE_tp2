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
