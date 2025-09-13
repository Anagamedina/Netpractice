# 🖧 NetPractice – 42 Barcelona


> Proyecto de la **Academia 42** para aprender y practicar conceptos de **redes informáticas**.  
> El objetivo es configurar correctamente distintas topologías de red entendiendo cómo funcionan las **IP, máscaras de subred, routing y comunicación entre hosts**.

---

## 🧩 Conceptos de redes

### 🔀 Switch
- Dispositivo de **capa 2** (enlace de datos).  
- Conecta varios dispositivos dentro de una misma **red local (LAN)**.  
- Funciona con **direcciones MAC**, no con IP.  
- Su tarea principal es **reenviar tramas al puerto correcto**.  
- **Ejemplo:** conectar varios PCs dentro de una oficina.  

---

### 📡 Router
- Dispositivo de **capa 3** (red).  
- Conecta **redes distintas** entre sí (ejemplo: tu red local con Internet).  
- Funciona con **direcciones IP**.  
- Permite el **routing** (elige la mejor ruta para enviar paquetes).  
- **Ejemplo:** el router de casa conecta tu red WiFi con Internet.  

---

### 🌍 Direcciones IP privadas y públicas
- **Privadas (no se enrutan en Internet) → rangos reservados:**  
  - Clase A → `10.0.0.0 – 10.255.255.255`  
  - Clase B → `172.16.0.0 – 172.31.255.255`  
  - Clase C → `192.168.0.0 – 192.168.255.255`  

- **Públicas (enrutables en Internet)** → todas las demás direcciones IP.  
- Para salir a Internet, los hosts privados usan **NAT** en el router.  

---

### 🚪 Gateway (puerta de enlace)
- Es la **IP del router** en la red local.  
- Todos los hosts de la red la usan para salir hacia otras redes.  

---

### 🔄 NAT (Network Address Translation)
- Traduce **IPs privadas → IP pública** en el router.  
- Permite que muchos dispositivos internos compartan **una sola IP pública** en Internet.  

---

### 🎫 DHCP (Dynamic Host Configuration Protocol)
- Asigna **IPs automáticamente** a los dispositivos.  
- Evita tener que configurarlas manualmente.  
- **Ejemplo:** tu móvil recibe una IP cada vez que se conecta al WiFi.  

---

### 🌐 DNS (Domain Name System)
- Convierte **nombres de dominio → IPs**.  
- **Ejemplo:** `www.google.com → 142.250.184.206`.  

---

### 🔍 ARP (Address Resolution Protocol)
- Traduce **IP ↔ MAC** dentro de una red local.  
- **Ejemplo:** antes de enviar un paquete a `192.168.1.15`, el PC pregunta:  
  “¿Quién tiene esta IP? Dame tu MAC”.  

---

## 🚀 Pasos a seguir en el proyecto

1. **Leer el subject** de NetPractice para entender los ejercicios.  
2. **Repasar conceptos básicos** de IP y subredes (explicados aquí abajo).  
3. **Resolver cada nivel** en el simulador, comprobando siempre:  
   - Que las IP pertenecen a la misma red.  
   - Que la máscara es correcta.  
   - Si no están en la misma red → necesitan un **router**.  
4. **Calcular subredes y hosts disponibles** con las fórmulas.  
5. **Testear y validar** que todos los dispositivos se comunican correctamente.  

---

## 📖 Conceptos clave

### Representación de una IP
```sjsx
Dirección IP: 192.168.1.15
┌───────┬───────┬───────┬───────┐
│ 192   │ 168   │ 1     │ 15    │   → Cada bloque es 1 byte (8 bits)
└───────┴───────┴───────┴───────┘
```

### Conceptos básicos de IP
- Cada **byte** va de `0 – 255`  
- Se escribe en **decimal con puntos**  
- Internamente: **binario**  
  - Ejemplo: `192 = 11000000`
```
---

## 🖧 Máscara de subred
```sjsx
IP: 192.168.1.15
Máscara: 255.255.255.0 → /24

┌────────────── Red ──────────────┐┌── Host ──┐
11000000 10101000 00000001 | 00001111
```


- La parte **izquierda** (24 bits) es **Red**  
- La parte **derecha** (8 bits) es **Host**  

---

### Notación CIDR
```sjsx
- Ejemplo: 192.168.1.15/24 → 24 bits para red, 8 bits para hosts  

Ejemplo /28:
┌────────────── Red ────────────┐┌─ Host ─┐
11000000 10101000 00000001 0000 | 1111

Red → 28 bits
Host → 4 bits
Hosts disponibles: 2^4 - 2 = 14

```

---

### Rangos privados

| Clase | Rango privado                          |
|-------|----------------------------------------|
| A     | 10.0.0.0 – 10.255.255.255              |
| B     | 172.16.0.0 – 172.31.255.255            |
| C     | 192.168.0.0 – 192.168.255.255          |

➡️ El resto de direcciones son **públicas**.  

---

### Comunicación entre dispositivos
- ✅ Misma red → **pueden comunicarse directamente**  
- ❌ Red distinta → **necesitan Router**  

---

### Fórmulas útiles

- **De un CIDR → máscara**:  
  CIDR = nº de bits en `1` en la máscara, luego se rellenan con ceros hasta completar 32 bits  

- **Número de hosts**:  
  Hosts = 2^(bits host) − 2  

- **Incremento de subredes**:  
  Incremento = 256 − último octeto de la máscara  

---

### Ejemplo /28
```sjsx
Subredes en un /24 con /28 → incremento 16

192.168.1.0/28 → Hosts: 1 - 14 | Broadcast: 15
192.168.1.16/28 → Hosts: 17 - 30 | Broadcast: 31
192.168.1.32/28 → Hosts: 33 - 46 | Broadcast: 47
```


---

### Tabla de referencia CIDR

| CIDR | Máscara decimal     | Bits host | Nº de hosts* | Ejemplo red       | Rango de hosts               | Broadcast       |
|------|--------------------|-----------|--------------|------------------|------------------------------|----------------|
| /32  | 255.255.255.255    | 0         | 1            | 192.168.1.15/32  | —                            | —              |
| /31  | 255.255.255.254    | 1         | 2            | 192.168.1.0/31   | —                            | —              |
| /30  | 255.255.255.252    | 2         | 2            | 192.168.1.0/30   | 192.168.1.1 – 192.168.1.2   | 192.168.1.3    |
| /29  | 255.255.255.248    | 3         | 6            | 192.168.1.0/29   | 192.168.1.1 – 192.168.1.6   | 192.168.1.7    |
| /28  | 255.255.255.240    | 4         | 14           | 192.168.1.0/28   | 192.168.1.1 – 192.168.1.14  | 192.168.1.15   |
| /27  | 255.255.255.224    | 5         | 30           | 192.168.1.0/27   | 192.168.1.1 – 192.168.1.30  | 192.168.1.31   |
| /26  | 255.255.255.192    | 6         | 62           | 192.168.1.0/26   | 192.168.1.1 – 192.168.1.62  | 192.168.1.63   |
| /25  | 255.255.255.128    | 7         | 126          | 192.168.1.0/25   | 192.168.1.1 – 192.168.1.126 | 192.168.1.127  |
| /24  | 255.255.255.0      | 8         | 254          | 192.168.1.0/24   | 192.168.1.1 – 192.168.1.254 | 192.168.1.255  |
| /23  | 255.255.254.0      | 9         | 510          | 192.168.0.0/23   | 192.168.0.1 – 192.168.1.254 | 192.168.1.255  |
| /22  | 255.255.252.0      | 10        | 1022         | 192.168.0.0/22   | 192.168.0.1 – 192.168.3.254 | 192.168.3.255  |

---

## 🎯 Objetivo final

Con estos apuntes podrás:

✅ Configurar IPs y máscaras correctas.  
✅ Identificar qué dispositivos se comunican.  
✅ Calcular hosts, subredes e incrementos.  
✅ Comprender cómo funcionan **switches, routers, NAT, DHCP, DNS y ARP**.  
✅ Completar todos los niveles de **NetPractice** 🚀

