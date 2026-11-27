# OpenWrt para LTC Velo7 Max

Documentación práctica para compilar, instalar y configurar OpenWrt en el LTC Velo7 Max.

## Estado del soporte

OpenWrt `v25.12.6` no incluye el perfil del Velo7 Max. El soporte oficial se incorporó después en el commit [`25f6620614acf98a6afe3a85ad6a37ea035766d8`](https://github.com/openwrt/openwrt/commit/25f6620614acf98a6afe3a85ad6a37ea035766d8). Para construir una imagen basada en 25.12.6 hay que aplicar ese cambio y comprobar que compila y arranca en el equipo.

Esto es documentación y una receta de backport; aquí todavía no se ha compilado ni probado una imagen 25.12.6.

## Documentos

- [Hardware y puertos](docs/HARDWARE-PORTS.md)
- [Perfil Wi-Fi](docs/WIFI.md)
- [Red y SQM](docs/NETWORK-SQM.md)
- [Compilar 25.12.6](docs/BUILD-25.12.6.md)
- [Actualización y recuperación](docs/UPGRADE-RECOVERY.md)
- [Inventario y límites de lo verificado](docs/INVENTORY.md)

## Referencias principales

- [OpenWrt 25.12.6](https://github.com/openwrt/openwrt/releases/tag/v25.12.6)
- [Commit oficial de soporte LTC Velo7 Max](https://github.com/openwrt/openwrt/commit/25f6620614acf98a6afe3a85ad6a37ea035766d8)
- [Tabla de hardware de OpenWrt](https://toh.openwrt.org/?features=wifi_be&view=all)

## Seguridad y cumplimiento

Usar el código de país real del lugar de instalación (España: `ES`) y los límites de potencia que anuncian el driver y el dominio regulatorio. No usar un país distinto para eludir límites. Antes de actualizar, guardar una copia privada de la configuración y confirmar que la imagen corresponde exactamente al Velo7 Max.
