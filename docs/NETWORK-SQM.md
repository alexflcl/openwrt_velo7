# Red cableada y SQM

## WAN/LAN de referencia

La configuración de placa upstream asigna `eth2` a WAN y agrupa `eth1` junto con `lan0`–`lan3` en `br-lan`. En la instalación documentada, el router aguas arriba entrega conectividad por DHCP a la WAN. Ajustar el protocolo WAN al proveedor real (DHCP, PPPoE, VLAN u otro); no copiar direcciones de una red doméstica a una imagen pública.

Para diagnosticar ausencia de Internet, comprobar por capas:

1. `ip -br link` y `ethtool eth2`: enlace y velocidad del puerto WAN.
2. `ifstatus wan`: dirección, ruta por defecto y DNS.
3. `ip route` y `ping` a la puerta de enlace del proveedor.
4. Resolver un nombre DNS y probar conectividad desde un cliente LAN.
5. Revisar que el firewall tenga zona WAN con masquerading y forwarding LAN→WAN.

El nombre `eth2` es una interfaz lógica; no asumir qué conector físico es sin comprobar el enlace.

## SQM/CAKE

SQM reduce latencia bajo carga al limitar la cola al cuello de botella. Puede reducir la velocidad máxima de un test si se configura por debajo de la capacidad real, y consume CPU. En la configuración observada anteriormente se usó CAKE (`piece_of_cake.qos`) en `eth2`, con 900000 kbit/s de subida y bajada y encapsulación `none`. Esos números son solo un punto de partida histórico, no una recomendación universal.

Procedimiento de ajuste:

1. Medir varias veces la velocidad WAN sin SQM, por cable y en condiciones estables.
2. Configurar SQM sobre la interfaz real de salida WAN.
3. Empezar alrededor del 90–95 % de la velocidad sostenida medida en cada sentido; en PPPoE/VLAN o enlaces variables, ajustar con pruebas de latencia bajo carga.
4. Revisar CPU durante pruebas simultáneas de subida y bajada. Si la CPU llega al límite, el caudal agregado puede caer.
5. Mantener o retirar SQM según el objetivo: menor latencia bajo carga frente al máximo pico de throughput.

Con fibra cercana a 1 Gb/s, un router puede no alcanzar 1 Gb/s en ambas direcciones con CAKE activo, según versión, carga y configuración. La latencia de 12 ms de un test puede estar dominada por la ruta o servidor; SQM se aprecia sobre todo comparando latencia mientras la conexión está saturada.

No incluir aquí la IP, credenciales PPPoE, claves Wi-Fi ni exportaciones completas de UCI.
