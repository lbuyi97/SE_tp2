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


b)
Se analizan algunas definiciones y funciones de 'sapi_button.c' y 'sapi_button.h'

Primero tenemos a la estructura 'buttonEvent_t' que tiene la siguiente definición en el archivo 'sapi_button.h', están los distintos tipos de eventos, y se tiene también la estructura 'button_t'
```
/*=====[Definitions of public data types]====================================*/
   
// Button events names
typedef enum{
   BUTTON_NO_EVENT,            
   BUTTON_PRESSED,       // BUTTON_UP --> BUTTON_FALLING --> BUTTON_DOWN (falling edge)
   BUTTON_RELEASED,      // BUTTON_DOWN --> BUTTON_RISING --> BUTTON_UP (rising edge)
   BUTTON_HOLD_PRESED,   // BUTTON_PRESSED and elapsed certain time
   BUTTON_EVENT_HANDLED, // A previous event was handled
} buttonEvent_t;
[...]
// Button object structure
typedef struct{

   // Pin and electrical connection

   int32_t gpio;
   bool_t logic;

   // Button scan time

   tick_t refreshTime; // Time [ms] that refresh buttons (update fsm)
                       //Must be more than bouncing time! i.e. 50ms

   // Button FSM

   buttonFsmState_t state;
   bool_t flagUp;
   bool_t flagDown;
   bool_t flagFalling;
   bool_t flagRising;
   tick_t timeInSate; // In [ms]

   // Button event

   tick_t event;

   bool_t checkPressedEvent;
   bool_t checkReleasedEvent;
   bool_t checkHoldPressedEvent;

   tick_t holdPressedTime; // In [ms]

   callBackFuncPtr_t pressedCallback;
   callBackFuncPtr_t releasedCallback;
   callBackFuncPtr_t holdPressedCallback;
} button_t;
```

Por otro lado, se tiene las funciones públicas para trabajar con los botones:
```
/*=====[Prototypes (declarations) of public functions]=======================*/

// Button initialization
void buttonInit( button_t* button,           // Button structure (object)                
                 int32_t gpio, bool_t logic, // Pin and electrical connection               
                 tick_t refreshTime,         // Button scan time
                 // Button event
                 bool_t checkPressedEvent,
                 bool_t checkReleasedEvent,
                 bool_t checkHoldPressedEvent,
                 tick_t holdPressedTime,
                 callBackFuncPtr_t pressedCallback,
                 callBackFuncPtr_t releasedCallback,
                 callBackFuncPtr_t holdPressedCallback );

// EVENT FUNCTIOS --------------------------------------------

// Get Button last event
buttonFsmState_t buttonEventGet( button_t* button );

// Event was handled
void buttonEventHandled( button_t* button );
```
