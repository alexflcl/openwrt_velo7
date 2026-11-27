# Actualización y recuperación

## Antes de actualizar

1. Guardar de forma privada un backup de configuración y conservar fuera del router las claves necesarias.
2. Confirmar que la imagen es para LTC Velo7 Max (`ltc_vl7m19k`), plataforma `mediatek/filogic`, y que el tipo de imagen corresponde al método de actualización.
3. Registrar la versión actual y mantener un método de acceso local. Una actualización de red puede interrumpir la sesión.
4. No restaurar a ciegas una configuración de una snapshot sobre otra versión: revisar cambios de UCI, firewall, wireless y gestor de paquetes.

## Desde firmware del fabricante

El soporte upstream describe el firmware del fabricante como basado en OpenWrt y añade una ruta para que su `sysupgrade` escriba la imagen OpenWrt en el slot inactivo. La opción `-R` solicita reiniciar tras la instalación. El procedimiento de referencia es:

```sh
sysupgrade -R openwrt-mediatek-filogic-ltc_vl7m19k-squashfs-sysupgrade.bin
```

Usar el nombre real de la imagen compilada. En el primer arranque de OpenWrt desde el firmware de fábrica, upstream indica ejecutar `firstboot` una sola vez para eliminar el overlay del fabricante. Esto borra la configuración del overlay; no hacerlo sin haber guardado lo que se necesite.

## Desde OpenWrt

Usar la imagen `sysupgrade` específica para el perfil y el mecanismo normal de actualización de OpenWrt, con la conservación de configuración elegida conscientemente. La lógica de plataforma upstream gestiona los dos slots; no escribir manualmente volúmenes NAND ni cambiar variables de entorno U-Boot sin una recuperación probada.

## Recuperación

La placa tiene mecanismo de doble slot y fallback descrito en el soporte upstream, pero no se debe confiar en ello como sustituto de un backup. Conservar firmware de recuperación del fabricante, instrucciones de recuperación y, si se va a desarrollar el port, acceso serie/consola documentado. No publicar dumps de NAND, EEPROM, factory data, backups completos, claves, contraseñas ni tokens.

Referencia: [lógica de actualización oficial](https://github.com/openwrt/openwrt/commit/25f6620614acf98a6afe3a85ad6a37ea035766d8).
