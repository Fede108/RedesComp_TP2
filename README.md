# Trabajo Practico nro 2
**Facultad de Ciencias Exactas, Fisicas y Naturales de la U.N.C**

**Redes de Computadoras**

**Profesores**
- Santiago M. Henn
- Facundo N. Oliva C.
  
**Nombre del grupo : Kiritoro** 

**Integrantes**
- Federico Cechich
- Juan Manuel Ferrero
- Luciano Trachta


**16 de Abril de 2025**


**Información de contacto**:  _juan.manuel.ferrero@mi.unc.edu.ar_, _federico.cechich@mi.unc.edu.ar_, _ltrachta@mi.unc.edu.ar_ 

## Resumen

## ¿Qué es `iperf3`?

Es una herramienta de línea de comandos que sirve para medir la velocidad de paquetes, el ancho de banda la pérdida de paquetes, y otros aspectos importantes del tráfico de red.

Se utiliza entre dos computadoras 

- Una actúa como servidor
- Otra como cliente

## 2. Principales comandos de `iperf3` para pruebas de red con **TCP** y **UDP**

- Protocolo (TCP vs UDP)
- Número y tamaño de paquetes
- Frecuencia / Tiempo de prueba
- Ancho de banda

---

## 🅰️ Protocolos TCP y UDP

| Protocolo | Comando base | Explicación |
|----------|----------------|-------------|
| **TCP** (por defecto) | `iperf3 -c <IP>` | TCP establece una **conexión confiable**, verifica errores y orden de paquetes. Ideal para pruebas de conexión realista como descargas. |
| **UDP** | `iperf3 -c <IP> -u` | UDP es **no confiable**, sin verificación de errores ni retransmisión. Ideal para pruebas de streaming, VoIP, etc. |

> 🔹 **Diferencia**: TCP ajusta automáticamente la velocidad según la red (control de congestión), mientras que UDP necesita que se defina el ancho de banda manualmente.

---

## 🅱️ Número y Tamaño de Paquetes

| Protocolo | Comando | Explicación |
|----------|---------|-------------|
| **TCP** | `iperf3 -c <IP> -l <TAM>` | Usa `-l` para establecer el tamaño del buffer de envío (por ejemplo: `-l 8K`). TCP puede fragmentar y reensamblar automáticamente. |
| **UDP** | `iperf3 -c <IP> -u -l <TAM>` | Define el tamaño del datagrama UDP (por ejemplo: `-l 512`). Es fundamental ya que UDP no fragmenta automáticamente. |

> 🔹 **Diferencia**: TCP gestiona la fragmentación y reensamblado. En UDP, el tamaño del datagrama es clave para evitar pérdidas o fragmentación IP.

---

## 🅲️ Frecuencia / Tiempo de Prueba

| Protocolo | Comando | Explicación |
|----------|---------|-------------|
| **TCP / UDP** | `iperf3 -c <IP> -t <TIEMPO>` | El parámetro `-t` define la duración de la prueba (en segundos), por ejemplo `-t 10`. Es igual para ambos protocolos. |

> 🔹 **Diferencia**: TCP adapta su velocidad a lo largo del tiempo. Con UDP conviene realizar pruebas más largas para evaluar pérdida, jitter y estabilidad.

---

## 🅳️ Ancho de Banda

| Protocolo | Comando | Explicación |
|----------|---------|-------------|
| **TCP** | *(automático)* | TCP ajusta el ancho de banda dinámicamente usando control de congestión y ventana. No requiere `-b`. |
| **UDP** | `iperf3 -c <IP> -u -b <BW>` | Con UDP, es obligatorio definir el ancho de banda (por ejemplo: `-b 10M`) ya que el protocolo no se regula por sí solo. |

> 🔹 **Diferencia**: TCP es "autoajustable" mediante algoritmos internos. UDP depende del valor definido manualmente por el usuario.

---

## 3 Consolas corriendo iperf

Este comando inicia a la PC como servidor, hace que la computadora espere conexiones desde otras, abre un puerto de escucha donde el cliente se puede conectar

![image](https://github.com/user-attachments/assets/614564b7-50a5-4ab0-9133-4dc977268d54)


---

## 4. Conclusiones del trafico usando iperf  
  - **Ancho de banda promedio (Bitrate):** Aproximadamente 4.8 Mbits/s.
  - **Tiempo de duracion de la prueba:** 10 segundos.
  - **Tamaño promedio de paquetes:** Por defecto, iperf3 en TCP utiliza un tamaño de ventana y va segmentando en paquetes (con un tamaño de segmento TCP cercano a 1460 bytes para la parte de datos).
  - **Relacion entre los parámetros y la perdida de paquetes:** 
---

## 5. Pruebas como cliente hacia el servidor propuesto en clase

### Envio de paquetes TCP mediante *iPerf3*. 

![image](https://github.com/user-attachments/assets/0965ead1-a29f-4ab5-9f8a-0d0cddf11dba)

El tamaño de segmento TCP se ajusta según las opciones del sistema operativo y la configuración interna de iPerf.  
Si no se especifica la duración de la prueba (por ejemplo, con `-t 5` para 5 segundos), **iPerf3** utiliza un valor por defecto de **10 segundos**.

### Parámetros del reporte

- **Interval**: Indica el rango de tiempo (en segundos) durante el cual se toman las mediciones parciales.  
  iPerf3 realiza mediciones en intervalos de **1 segundo** por defecto.

- **Transfer**: Muestra la cantidad total de datos transferidos en cada intervalo (expresado en MBytes, KBytes, etc.).

- **Bitrate**: Representa la tasa media de transferencia de datos en ese intervalo.  
  Se expresa comúnmente en **Mbits/sec** y también se conoce como *Throughput*.

- **Retr**: Indica cuántas veces se retransmitieron segmentos TCP en ese intervalo.  
  Un valor alto de **Retr** puede indicar problemas en la red, como **pérdida de paquetes** o **congestión**.

- **Cwnd**: Corresponde a la *congestion window* (ventana de congestión) de TCP.  
  Es un valor interno que limita la cantidad máxima de datos "en vuelo", es decir, enviados pero aún no reconocidos mediante ACK.

### Trafico UDP usando *iPerf3* capturado mediante *WireShark*

![image](https://github.com/user-attachments/assets/7c38ee46-6e57-4963-8094-ccb6ec92460d)

---

### Envio de paquetes TCP mediante *iPerf3*. 

![image](https://github.com/user-attachments/assets/12aed5a4-ff46-43ee-ab64-663fbeb7f25e)

En este caso, se debe utilizar `-u` para indicar que el protoclo de envio es mediante **UDP**. Con `-t` seleccionamos un tiempo de transmicion de 5 segundos y usando la directiva `-b` el bitrate por cada intervalo.

### Parámetros del reporte

 - **Total Datagrams**: Esta columna es específica de iPerf en modo UDP. Muestra la cantidad total de datagramas (paquetes UDP) enviados o recibidos en ese intervalo.
 - **Metricas Finales**: Al final de la prueba (en la penúltima y última línea), iPerf3 muestra un resumen con métricas de UDP, que incluyen:
   - Transfer: Cantidad total de datos transferidos durante toda la prueba.
   - Bandwidth: El bitrate promedio total.
   - Jitter: Variación en el tiempo de llegada de los paquetes (latencia variable).
   - Lost/Total Datagrams: Cantidad de datagramas perdidos respecto al total enviados, y el porcentaje de pérdida.  

### Trafico UDP usando *iPerf3* capturado mediante *WireShark*

![image](https://github.com/user-attachments/assets/e51d32b0-a80f-45e0-acf1-0409dffcf3a1)
