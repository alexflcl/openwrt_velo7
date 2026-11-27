# Hardware y puertos

## Plataforma

El soporte upstream identifica el equipo como **LTC Velo7 Max**, perfil `ltc_vl7m19k`, plataforma MediaTek Filogic/MT7988A. Incluye CPU de cuatro núcleos Cortex-A73, NAND SPI de 256 MiB, radios MediaTek MT7996 Wi-Fi 7 y variantes de memoria RAM de 1, 4 u 8 GiB. La placa integra cuatro puertos Ethernet de 1 Gb/s y dos interfaces de 10 Gb/s.

Las variantes de RAM no cambian el nombre del perfil. La memoria disponible para paquetes depende de las particiones y del espacio overlay de la imagen; no equivale a la RAM física ni a toda la NAND.

## Correspondencia lógica observada

El mapeo de red que instala el soporte upstream es:

| Interfaz Linux | Uso configurado |
| --- | --- |
| `eth2` | WAN |
| `eth1` | LAN, interfaz de 10 Gb/s |
| `lan0`–`lan3` | LAN, puertos de 1 Gb/s del switch |
| `br-lan` | Bridge de LAN que agrupa `eth1` y `lan0`–`lan3` |

`eth0` aparece como interfaz Linux, pero no forma parte del bridge LAN de referencia. No se debe deducir la posición física del puerto solamente por el nombre `ethN`: confirmar link y velocidad con `ip -br link`, `ethtool <interfaz>` y observando qué puerto cambia al conectar/desconectar cada cable.

La orientación física de los dos conectores de 10 Gb/s depende del etiquetado del chasis. Verificar WAN/LAN con la configuración activa antes de mover cables o cambiar el bridge.

## Datos de fábrica y arranque

La descripción upstream contempla NAND con NMBM, datos de calibración y MAC de fábrica, además de dos conjuntos de volúmenes kernel/rootfs para arranque dual. Esos datos son específicos del dispositivo. No borrar particiones de fábrica ni ejecutar comandos `mtd`, `ubiformat` o `firstboot` fuera de los casos descritos en la guía de [actualización y recuperación](UPGRADE-RECOVERY.md).

Referencia: [DTS y soporte upstream](https://github.com/openwrt/openwrt/commit/25f6620614acf98a6afe3a85ad6a37ea035766d8).
