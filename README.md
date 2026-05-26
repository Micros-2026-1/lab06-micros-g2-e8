[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/MCJunYEq)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23741103&assignment_repo_type=AssignmentRepo)
# Lab06: Comunicación UART en Microcontrolador PIC

Basado en la guía del repositorio [Microcontroladores_ECCI_2026-I - Lab06 UART]

---

# 1. Objetivos de aprendizaje

- Configurar el módulo UART del microcontrolador PIC para permitir la comunicación serial asíncrona.
- Comprender el funcionamiento del módulo EUSART del PIC18F45K22.
- Implementar funciones de transmisión y recepción de datos mediante UART.
- Transmitir datos desde el microcontrolador hacia un terminal serial conectado mediante un conversor USB-UART.
- Visualizar y analizar los datos recibidos desde un monitor serial o mediante un script en Python.
- Comprender parámetros importantes de la comunicación serial como:
  - Baud Rate
  - Bits de datos
  - Bits de parada
  - Paridad
- Aplicar configuración de registros internos relacionados con UART como:
  - `TXSTA`
  - `RCSTA`
  - `SPBRG`
  - `TRISC`

# 2. Herramientas
## Hardware
- Microcontrolador PIC18F45K22 o compatible.
- Programador/debugger PICkit 3 o PICkit 4.
- Conversor USB a UART serial.
- Fuente de alimentación.
- Cables de conexión.

## Software
- MPLAB X IDE.
- Compilador XC8.
- Terminal serial:
  - PuTTY
  - CuteCom
  - Monitor Serial de Arduino IDE
- Python 3.
- Librerías de Python:
  - `pyserial`
  - `matplotlib`

## Archivos utilizados
- `main.c`
- `uart.c`
- `serial_pic.py`

# 3. Procedimiento

## 3.1 Configuración del UART
Se configura el módulo EUSART del PIC en modo asíncrono:

- `TRISC6 = 0` → Pin TX como salida.
- `TRISC7 = 1` → Pin RX como entrada.
- `SPBRG = 25` → Configuración para 9600 baudios con oscilador de 16 MHz.
- `BRGH = 0` → Modo de baja velocidad.
- `SYNC = 0` → Comunicación asíncrona.
- `SPEN = 1` → Habilita puerto serial.
- `TXEN = 1` → Habilita transmisión UART.
- `CREN = 1` → Habilita recepción continua.

La fórmula usada para calcular el baud rate es:

```math
SPBRG = \frac{fosc}{64 \times Baudrate} - 1
```
Para:
- `fosc = 16 MHz`
- `Baudrate = 9600`
El valor obtenido es:
```c
SPBRG = 25;
```
---

## 3.2 Implementación de transmisión UART
Se implementan dos funciones principales:

### `UART_WriteChar()`
- Espera que el buffer de transmisión esté vacío.
- Envía un carácter usando `TXREG1`.

### `UART_WriteString()`
- Envía cadenas completas carácter por carácter.
- Utiliza punteros en C para recorrer el string.

Ejemplo:
```c
UART_WriteString("Hola, UART funcionando!");
```

---

## 3.3 Configuración del microcontrolador
En `main.c` se configuran los fuses:
```c
#pragma config FOSC = INTIO67
#pragma config WDTEN = OFF
#pragma config LVP = OFF
```

### Función de cada configuración
- `FOSC = INTIO67`
  - Usa oscilador interno.
- `WDTEN = OFF`
  - Deshabilita Watchdog Timer.
- `LVP = OFF`
  - Libera el pin RB3 como entrada/salida normal.

---

## 3.4 Verificación con monitor serial
### Opción 1: PuTTY

Configurar:

- Tipo de conexión: Serial
- Baud Rate: 9600
- Puerto COM correspondiente

### Opción 2: CuteCom
Configurar:

- 9600 baudios
- 8 bits de datos
- Sin paridad
- 1 bit de parada

### Opción 3: Arduino IDE
Usar el monitor serial integrado.

---

## 3.5 Visualización usando Python
El script `serial_pic.py` permite:

- Leer datos enviados desde el PIC.
- Mostrar información en tiempo real usando `matplotlib`.

Configuración del puerto serial:
### Windows
```python
SERIAL_PORT = 'COM3'
```

### Linux
```python
SERIAL_PORT = '/dev/ttyUSB0'
```
---

# 4. Entregables
1. Implementación funcional de la comunicación UART en el microcontrolador PIC.
2. Demostración práctica ante el docente.
3. Documentación técnica del laboratorio que incluya:
   - Configuración del UART.
   - <img width="452" height="439" alt="PUTYY" src="https://github.com/user-attachments/assets/86e649f3-20d2-41e1-aed2-f0d8df81790c" />
   - Explicación de registros utilizados.
   - Código implementado.
   - Evidencias de funcionamiento.
   - <img width="1280" height="960" alt="6a5a6502-30ac-45c1-a16f-3bd43ccad233" src="https://github.com/user-attachments/assets/7216c35c-f680-4fd5-9b78-969830635fc2" />
   - Capturas del monitor serial.
   - <img width="960" height="1280" alt="1- PRUEB DE COMUNICACION UART" src="https://github.com/user-attachments/assets/3de83f6c-ee7b-494c-89ae-ff8e1272293d" />

4. Explicación del funcionamiento del script en Python para recepción de datos UART.
5. <img width="807" height="689" alt="GRAFICA DEL SCRIPT" src="https://github.com/user-attachments/assets/05937aae-2c1b-4785-bfd7-35b0d2d1c79a" />
7. Diagrama de conexiones utilizadas entre:

   <img width="743" height="806" alt="56302896-fdea-4c8b-a48c-12df84b8e5c3" src="https://github.com/user-attachments/assets/24993bf6-1229-4c96-9db7-87e63afc22fd" />
   <img width="960" height="1280" alt="233aa073-6403-4961-9e25-cf7fe1a11026" src="https://github.com/user-attachments/assets/398c0a79-97c9-415d-b4f9-df1150a00082" />




