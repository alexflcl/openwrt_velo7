# Perfil Wi-Fi recomendado

Esta es una base de configuración para España y uso doméstico. La potencia efectiva, los canales y el ancho disponibles los decide el conjunto país/regulación, firmware, driver y hardware; no hay un ajuste que garantice el máximo teórico en todos los clientes.

## Perfil principal

| Radio | Configuración inicial recomendada | Notas |
| --- | --- | --- |
| 2,4 GHz | `ES`; canal fijo 1, 6 u 11; 20 MHz en entorno congestionado (40 MHz solo si el espectro está limpio) | Mejor alcance y compatibilidad IoT. |
| 5 GHz | `ES`; canal no DFS para empezar; 80 MHz como base estable; probar 160 MHz si cliente y entorno lo permiten | DFS puede causar cambio de canal y pausas. 160 MHz no garantiza más caudal útil. |
| 6 GHz | `ES`; modo 802.11be/EHT si está disponible; canal permitido por el driver; probar 320 MHz cerca del AP y luego comparar con 160 MHz | Muy sensible a distancia y obstáculos. El cliente debe admitir 6 GHz en su región. |

Usar WPA3-SAE y PMF requerido en 6 GHz. En 2,4/5 GHz, WPA2/WPA3 mixto puede ayudar a clientes antiguos; elegir AES/CCMP, evitar TKIP y mantener WMM. No fijar potencia manual por encima del valor permitido por el dominio regulatorio.

## SSID común y roaming

Poner el mismo SSID y contraseña en varios radios permite que muchos clientes elijan y cambien de banda, pero **no activa MLO**. MLO requiere soporte compatible tanto en el AP como en el cliente, además de soporte efectivo en la versión de hostapd/driver. Si hay problemas de asociación o caudal, probar primero con MLO desactivado y radios separados durante el diagnóstico.

Wi-Fi 7 (EHT) tampoco implica que cada conexión vaya a usar 4096-QAM o 320 MHz. La modulación y el ancho se negocian según capacidades mutuas y calidad de señal; un cliente puede conectar por Wi-Fi 6 aunque anuncie compatibilidad con Wi-Fi 7.

La señal de 6 GHz cae más con paredes y distancia que 5 GHz. Que el móvil elija 5 GHz al alejarse puede ser un comportamiento normal del cliente, aunque el SSID sea el mismo.

## Red IoT compatible

Para dispositivos domóticos antiguos, crear un SSID adicional en **2,4 GHz solamente**:

- Canal 1, 6 u 11 y ancho 20 MHz.
- WPA2-PSK con AES/CCMP; PMF desactivado u opcional si algún equipo antiguo lo necesita.
- SSID visible, WMM activado y sin 802.11r/roaming rápido inicialmente.
- Aislamiento de clientes según necesidades: activarlo impide que ciertos dispositivos se descubran entre sí o que un controlador local los alcance.
- Contraseña nueva y única, introducida localmente; no guardar secretos en este repositorio.

Si algún equipo solo funciona con tasas 802.11b, habilitarlas únicamente si es necesario: pueden reducir el tiempo de aire disponible para todos los clientes de esa radio.

## Pruebas comparables

Anotar para cada prueba: banda, canal, ancho, seguridad, modo negociado del cliente, RSSI, distancia/obstáculos, servidor de prueba y velocidad por cable de referencia. Ejecutar varias pruebas en cada ajuste y comparar medianas. La prueba de velocidad a Internet también depende del servidor, del proveedor y del SQM; no mide únicamente el Wi-Fi.

En pruebas previas del usuario se observaron aproximadamente 800–850 Mb/s cerca del AP y una bajada de rendimiento al interponer una pared. Es una observación de esa instalación, no una garantía ni una medición controlada del límite del hardware.
