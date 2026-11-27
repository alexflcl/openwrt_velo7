# Inventario y estado verificado

Este documento separa los datos del soporte upstream de observaciones de una instalación concreta. No es un backup ni una declaración de que todos los ajustes sigan activos.

## Confirmado en soporte upstream

- Modelo: LTC Velo7 Max; perfil `ltc_vl7m19k`.
- Target: `mediatek/filogic`; SoC MT7988A; radio MT7996 Wi-Fi 7.
- NAND SPI de 256 MiB con NMBM; dos conjuntos de volúmenes kernel/rootfs para arranque dual.
- Cuatro puertos LAN de 1 Gb/s, una interfaz LAN de 10 Gb/s y una interfaz WAN de 10 Gb/s según el mapeo de placa.
- Mapeo de red por defecto: `eth2` WAN; `eth1` y `lan0`–`lan3` en `br-lan`.
- La memoria RAM existe en variantes de 1, 4 u 8 GiB; comprobar la variante física antes de fijar expectativas.

## Observaciones previas de la unidad del usuario

- Se había instalado una snapshot de OpenWrt; una nota de inventario anterior registraba `r36402-820fbf4a9f`. Puede haber cambiado al actualizar.
- La unidad se había configurado con WAN DHCP sobre `eth2` y CAKE/SQM en `eth2`, con valores iniciales de 900000 kbit/s por sentido. Revisar el estado actual antes de darlo por activo.
- Se probaron radios 2,4, 5 y 6 GHz; se observaron cerca de 800–850 Mb/s en una prueba Wi-Fi 6 GHz cerca del AP. Al alejarse o atravesar una pared el móvil tendía a elegir 5 GHz/Wi-Fi 6.
- Se exploró MLO, pero no hay evidencia fiable aquí de que MLO quedara estable y activo de forma continua. Tratarlo como experimental hasta validarlo.
- No se registran aquí SSID, contraseñas, MAC, IPs domésticas, identificadores Tailscale ni credenciales.

## Qué comprobar en cada imagen nueva

```sh
ubus call system board
cat /etc/openwrt_release
ip -br link
iw list
iw dev
uci show network
uci show wireless
uci show sqm
```

Revisar la salida localmente y eliminar secretos antes de compartirla. La configuración del router es la fuente de verdad: las observaciones de este archivo son históricas y pueden quedar obsoletas.
