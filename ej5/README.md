a) La forma de compilar de forma condicional se explicó en los ejercicios anteriores.

c) Para poder enviar mensajes mientras se realiza un debug a una terminal, se pueden hacer uso de las funciones dentro del archivo sapi_debugPrint.h. Este archivo prácticamente consiste en un conjunto de macros que se definen como funciones del archivo sapi_print.h (funciones correspondientes al uso de las UARTs para enviar mensajes) con ciertos parámetros en particular. Para habilitar el debugPrint, se llama a la macro "DEBUG_PRINT_ENABLE" que lo que realiza es la declaración de una varible global de tipo print_t llamada debugPrint que luego van a hacer uso las demás funciones del archivo. A continuación se muestra una tabla describiendo las funciones más importantes.

|Función|Descripción de lo que hace|
|:------------------------------------------------------------|:---------------------------------------------|
|debugPrintSetUart(uart)|Define cual va ser la UART a usar. Para ello, le asigna el valor de la uart elegida a la variable global debugPrint.|
|debugPrintConfigUart(uart,baudRate) | Realiza lo mismo que debugPrintSetUart solo que además iniciliza la uart (con la función uartInit(uart, baudRate) del archivo sapi_uart.c) con un baudrate de valor baudRate.|
|debugPrintString(string)|Escribe un string a la uart. Hay otro par de funciones muy parecidas a esta pero que además imprimen un '\n' al final o 'ln'|
|debugPrintIntFormat(number,format)|Escribe un int "number" en base "format" a la UART. Para la conversión a string hace uso de la función "int64ToString" del archivo sapi_convert.c. Hay otras varias funciones parecidas a esta pero para imprimir con '\n' o '\ln', uint o en formato hexadecimal. Y algunas otras en la que no se especifica la base (simplemente se hace en base 2).|

Anteriromente se dijo que debugPrint era de tipo print_t. Este tipo se define en el archivo sapi_print.h como uartMap_t, la cual es una estructrura de la siguiente forma:
```
typedef enum {
	#if (BOARD == ciaa_nxp)
	   UART_485  = 1, // Hardware UART0 via RS_485 A, B and GND Borns
                          // Hardware UART1 not routed
	   UART_USB  = 3, // Hardware UART2 via USB DEBUG port
	   UART_232  = 5, // Hardware UART3 via DB9 connector
	#elif (BOARD == edu_ciaa_nxp)
	   UART_GPIO = 0, // Hardware UART0 via GPIO1(TX), GPIO2(RX) pins on header P0
	   UART_485  = 1, // Hardware UART0 via RS_485 A, B and GND Borns
                          // Hardware UART1 not routed
	   UART_USB  = 3, // Hardware UART2 via USB DEBUG port
	   UART_ENET = 4, // Hardware UART2 via ENET_RXD0(TX), ENET_CRS_DV(RX) pins on header P0
	   UART_232  = 5, // Hardware UART3 via 232_RX and 232_tx pins on header P1
	#else
	   #error BOARD not supported yet!
	#endif
   UART_MAXNUM,
} uartMap_t;
```
