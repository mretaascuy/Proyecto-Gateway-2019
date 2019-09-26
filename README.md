# Proyecto-Gateway-2019

## 1.	Propósito del documento

Describir la arquitectura y configuración del servidor KepWareEX. 

## 2. Alcance

## 3. Definiciones, acrónimos y abreviaturas

## 4. Introducción

KepServerEx es servidor OPC-UA.
¿Cuál fue la necesidad que llevo a cambiar el Gateway y reemplazarlo por KepWare? ¿Cuál es el objetivo del KepWare? ¿Cuáles son los beneficios?

## 5. Arquitectura

Describir cada componente de la arquitectura.
Servidor KepWare: SRVKEPW, 192.168.161.5.
Base de datos Oracle: SGIPDB_T, 192.168.161.9

## 6. Configuración KepWare

### 6.1.	Lineamientos generales para channels, devices, alias y TAGs

El nombre de cada channel tendrá una referencia al tipo de instalaciones que encuesta más un número correlativo de tres cifras, por ejemplo: ESP_001 para el primer channel de pozos BES.

Se tendrá un device por pozo y el Scan Mode será Respect Tag-Specified Scan Rate.

Se deberán crear los TAGs correspondientes al equipo en cuestión, creando agrupamientos de TAGs si fuera necesario y se deberá configurar el Scan Rate en cada TAG según corresponda (1 min por defecto).

Se tendrá como máximo 5 pozos por channel, esta cantidad fue elegida por el tiempo de transacción (10 segundos aproximados por pozo por transacción) y un tiempo de pooleo de 1 minuto a cada pozo.

La decisión de tener varios devices en un mismo channel es por limitación de hasta 2048 channels disponibles para MODBUS RTU (Protocolo usado para las ESP) y de 1024 para ELAM (Protocolo usado para POFF).

Ante un cambio de tecnología de extracción, la reutilización de devices deberá ser por tipo de controlador (o sea si usa el mismo protocolo). Se deberá guardar, en el campo “Descripción” del device, el tipo de controlador que contiene para el caso de pozos.

Se agruaprán los mismos tipos de instalación (protocolo?) por channel.

Se utilizarán los alias apuntados al conjunto channel + device (+ grupo de tags) para facilitar el borrado de device en desuso. Por ejemplo el alias POZO_PVH-1082_GENERAL apunta a ESP_001.PVH-1082.

Siempre se utilizarán alias para direccionamiento. Siendo el lineamiento para su creación: TipoInstalacion_IDInstalacion_Agrupamiento, siendo el alias para los datos generales del pozo PVH-1082 : POZO_PVH-1082_GRAL

En caso que una instalación (pozo , batería, PIAS, etc) tenga varios tipos de TAGs de diferentes agrupaciones o zonas se crearán grupos de TAGs para mejorar el ordenamiento. En este caso se crearán alias apuntando a dicho agrupamiento de TAGs, por ejemplo:


