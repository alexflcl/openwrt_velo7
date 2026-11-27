# Compilar una imagen Velo7 Max basada en OpenWrt 25.12.6

## Por qué hace falta un backport

El tag oficial [`v25.12.6`](https://github.com/openwrt/openwrt/releases/tag/v25.12.6) precede al soporte del LTC Velo7 Max. El commit upstream [`25f6620614acf98a6afe3a85ad6a37ea035766d8`](https://github.com/openwrt/openwrt/commit/25f6620614acf98a6afe3a85ad6a37ea035766d8) añade el DTS, perfil de imagen, red/LEDs, entorno U-Boot y lógica de actualización dual. Por eso, una compilación pura de v25.12.6 no ofrece el perfil del dispositivo.

## Obtener la fuente y aplicar el cambio

En una máquina de compilación Linux con las dependencias de compilación indicadas por OpenWrt:

```sh
git clone https://git.openwrt.org/openwrt/openwrt.git
cd openwrt
git checkout v25.12.6
git fetch origin main
git cherry-pick 25f6620614acf98a6afe3a85ad6a37ea035766d8
```

El cherry-pick conserva el commit upstream completo y deja constancia de su procedencia. Si Git informa conflictos, revisarlos contra el diff oficial; no resolverlos a ciegas, especialmente en `platform.sh` y el DTS.

Comprobar antes de compilar que el cambio está aplicado:

```sh
git log -1 --oneline
grep -R "ltc_vl7m19k" target/linux/mediatek
```

## Seleccionar y compilar

Actualizar e instalar feeds, seleccionar `MediaTek / Filogic / LTC Velo7 Max` en `make menuconfig`, guardar y compilar:

```sh
./scripts/feeds update -a
./scripts/feeds install -a
make menuconfig
make defconfig
make download -j"$(nproc)"
make -j"$(nproc)" V=s
```

El identificador de perfil del dispositivo es `ltc_vl7m19k`. Confirmar en la configuración final que el target sea `mediatek/filogic` y el perfil sea el Velo7 Max. El soporte upstream define una imagen `squashfs-sysupgrade`; revisar el nombre real y el manifiesto generados bajo `bin/targets/mediatek/filogic/` antes de instalar.

Para añadir LuCI, SQM u otros paquetes, seleccionarlos en `menuconfig` o en la configuración de imagen. Mantener una configuración mínima en el primer build facilita separar fallos de soporte de fallos de paquetes. 25.12 utiliza `apk` como gestor de paquetes; no reutilizar scripts que asuman `opkg` sin adaptarlos.

## Validación mínima antes de uso diario

- La compilación termina y genera imagen y manifiesto del perfil correcto.
- La imagen arranca y reconoce NAND, radios, Ethernet y dos slots.
- `br-lan` incluye los puertos LAN esperados y `eth2` negocia como WAN.
- Se conservan los datos calibrados de radio y las MAC de fábrica.
- Se prueba sysupgrade y se conserva una vía de recuperación documentada.
- Se prueban las radios en dominio `ES`, empezando con anchos moderados y sin MLO.

No se ha compilado ni flasheado esta receta en el momento de escribirla. Una compilación satisfactoria no sustituye las pruebas de arranque/recuperación en la unidad concreta.
