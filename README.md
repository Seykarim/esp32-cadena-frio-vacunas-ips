# ESP32 Smart Cold-Chain & Vaccine Monitoring System for Rural Health Centers

[![Hardware](https://img.shields.io/badge/Hardware-ESP32%20%7C%20DS18B20%20%7C%20ReedSwitch-blue.svg)](#componentes)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PlatformIO](https://img.shields.io/badge/PlatformIO-Compatible-orange.svg)](https://platformio.org/)

Un sistema IoT de bajo costo, portátil y de código abierto basado en el microcontrolador **ESP32** para el monitoreo continuo de la cadena de frío ($2^\circ\text{C} \text{ a } 8^\circ\text{C}$) en refrigeradores y termos de vacunación de Instituciones Prestadoras de Servicios de Salud (IPS) rurales y urbanas.

---

## Motivación y Relevancia Sanitaria

El mantenimiento de la cadena de frío es indispensable para preservar la inmunogenicidad de las vacunas y la estabilidad de insumos biomédicos:
- **Prevención de Pérdidas:** Notificación inmediata ante fallas del suministro eléctrico o alteración de temperatura en el refrigerador.
- **Detección de Apertura Prolongada:** Alarma por puerta mal cerrada mediante sensor magnético switch reed.
- **Sin Costos Recurrentes:** Operatorio mediante la API gratuita de Telegram Bot sobre la infraestructura WiFi existente de la IPS o punto de acceso móvil.
- **Alineación Normativa:** Herramienta de apoyo a los procesos de habilitación y calidad de la atención en salud.

---

## Lista de Materiales (BOM)

| Componente | Función | Precio Aprox. (COP) |
| :--- | :--- | :---: |
| **ESP32 DevKit V1** | Microcontrolador principal (Dual Core + WiFi + Bluetooth) | ~$30.000 |
| **DS18B20 (Sumergible)** | Sensor de temperatura digital de alta precisión (1-Wire) | ~$16.000 |
| **Sensor Magnético Reed Switch** | Detección de apertura/cierre de la puerta del refrigerador | ~$8.000 |
| **Buzzer Activo 5V** | Alarma sonora local para personal asistencial | ~$4.000 |
| **DHT22 / AM2302** | Sensor de humedad y temperatura ambiente de la sala | ~$20.000 |
| **Módulo TP4056 + Batería LiPo 3.7V** | Sistema de respaldo de energía ante falla eléctrica (1000mAh) | ~$25.000 |
| **Protoboard / Cableado / Resistencia 4.7kΩ** | Interconexión de componentes y pull-up I2C/1-Wire | ~$8.000 |
| **TOTAL ESTIMADO** | | **~$111.000 COP (~$27 USD)** |

---

## Esquema de Conexionado (Pinout)

| Componente | Pin del Componente | Pin ESP32 DevKit V1 | Notas |
| :--- | :--- | :--- | :--- |
| **DS18B20** | VCC (Rojo) | 3.3V | Requiere resistencia de pull-up de 4.7kΩ a VCC |
| | GND (Negro) | GND | Tierra común |
| | DATA (Amarillo) | GPIO 4 | Bus OneWire |
| **Sensor Magnético** | Terminal 1 | GPIO 13 | Configurado con resistencia interna `INPUT_PULLUP` |
| | Terminal 2 | GND | Se conecta a tierra al cerrar |
| **Buzzer Activo** | VCC (+) | GPIO 25 | Control digital |
| | GND (-) | GND | Tierra común |
| **DHT22** | VCC | 3.3V | Alimentación lógica |
| | GND | GND | Tierra común |
| | DATA | GPIO 16 | Lectura de ambiente |

---

## Modelo Térmico y Algoritmo de Control

El sistema evalúa de forma continua la variación de temperatura mediante la tasa de cambio instantánea:

$$\frac{dT}{dt} = \frac{T_{actual} - T_{anterior}}{\Delta t}$$

Rumbos de alerta preconfigurados:
1. **Rango Crítico de Vacunas:** $T < 2.0^\circ\text{C}$ (Riesgo de congelación) o $T > 8.0^\circ\text{C}$ (Riesgo de pérdida de potencia biológica).
2. **Alerta de Puerta Abierta:** Si el sensor magnético detecta estado `HIGH` (abierto) de manera continua por un tiempo $t > 45 \text{ segundos}$.
3. **Tendencia de Pérdida de Frío:** Si $\frac{dT}{dt} > +0.5^\circ\text{C}/\text{minuto}$ con puerta cerrada (posible falla del compresor).

---

## Configuración del Bot de Telegram

1. Inicie una conversación con `@BotFather` en Telegram y envíe `/newbot`.
2. Obtenga el **Token** generado.
3. Consulte su **Chat ID** de grupo o personal mediante `@userinfobot`.
4. Defina estos parámetros en el archivo `firmware/src/config.h`.

---

## Descargo de Responsabilidad

Este proyecto es un prototipo desarrollado con fines **académicos, de investigación y desarrollo tecnológico libre**. No constituye un dispositivo médico calibrado con trazabilidad metrológica oficial ni sustituye los sistemas validados bajo normativa nacional de habilitación IPS sin previa calibración de laboratorio acreditado ONAC.

---

## Autor

**Seykarim R. Mestre Zalabata**  
*Ingeniero Electrónico | Innovación en Salud & IoT Territorial*  
Valledupar, Cesar, Colombia  
[![GitHub](https://img.shields.io/badge/GitHub-Seykarim-181717?style=flat&logo=github)](https://github.com/Seykarim)
