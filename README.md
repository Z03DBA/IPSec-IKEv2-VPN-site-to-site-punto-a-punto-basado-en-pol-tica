# 🔒 Práctica 1B: VPN Site-to-Site Basada en Políticas (IPsec IKEv2)

Este repositorio contiene los scripts de configuración, el diagrama de topología y la documentación técnica formal para la implementación de una Red Privada Virtual (VPN) Punto a Punto basada en políticas utilizando el protocolo moderno **IKEv2** en routers Cisco IOS mediante el simulador GNS3.

## 👥 Información del Estudiante
* **Nombre Completo:** Zoe Daniela Bobonagua Acevedo
* **Matrícula:** 2024-0927
* **Carrera:** Tecnología en Seguridad Informática
* **Institución:** Instituto Tecnológico de Las Américas (ITLA)
* **Asignación:** Práctica 1 - Variante IKEv2 (P1)

---

## 🗺️ Diseño de la Topología y Direccionamiento
La red utiliza una arquitectura lineal donde el núcleo del proveedor de servicios de Internet (ISP) interconecta dos sucursales remotas (Peer A y Peer B). El direccionamiento IP se ha estructurado de forma estricta sobre el bloque base **192.8.39.0** derivado de la matrícula institucional:

* **Router 1 (ISP - Nodo Central):**
  * Interfaz `Gi1/0` (Hacia R2): `192.8.39.2/30`
  * Interfaz `Gi2/0` (Hacia R3): `192.8.39.6/30`
* **Router 2 (Peer A - Sucursal Izquierda):**
  * Interfaz `Gi1/0` (WAN): `192.8.39.1/30` -> Gateway: `192.8.39.2`
  * Interfaz `Gi2/0` (LAN A): `192.8.39.65/26`
  * **PC1 (Host Virtual VPCS):** IP `192.8.39.66/26` | Gateway: `192.8.39.65`
* **Router 3 (Peer B - Sucursal Derecha):**
  * Interfaz `Gi1/0` (WAN): `192.8.39.5/30` -> Gateway: `192.8.39.6`
  * Interfaz `Gi2/0` (LAN B): `192.8.39.129/26`
  * **PC2 (Host Virtual VPCS):** IP `192.8.39.130/26` | Gateway: `192.8.39.129`

---

## 🛡️ Matriz de Parámetros de Seguridad (IKEv2)
Para el despliegue del túnel criptográfico se implementó un esquema modular corporativo de alta seguridad bajo el estándar IKEv2:

| Componente / Fase | Algoritmo / Especificación | Rol Técnico |
| :--- | :--- | :--- |
| **IKEv2 Proposal** | AES-CBC-256 / SHA-256 / DH Group 14 | Cifrado y validación de integridad en el intercambio inicial de llaves. |
| **IKEv2 Keyring** | Pre-Shared Key (`cisco123`) | Almacenamiento seguro de llaves simétricas emparejadas por IPs públicas. |
| **IKEv2 Profile** | Autenticación Local y Remota Local/Remote | Vinculación del llavero y verificación estricta de identidades de los Peers. |
| **Transform Set** | `esp-aes 256 esp-sha256-hmac` | Encapsulamiento de Seguridad de Carga de Datos (ESP) en Modo Túnel. |
| **Crypto Map** | Intercepción por ACL | Asociación de las políticas de interés (Tráfico LAN-to-LAN) al perfil IKEv2. |

---

## 🚀 Comandos Clave para la Demostración en Video
De acuerdo con la rúbrica de evaluación, durante la grabación del video explicativo se ejecutan y validan en consola los siguientes comandos de diagnóstico:

```bash
# 1. Validar el estado operacional del túnel IKEv2 (Debe mostrar el estado READY)
show crypto ikev2 sa

# 2. Examinar las propuestas de seguridad configuradas
show crypto ikev2 proposal

# 3. Verificar las Asociaciones de Seguridad (SAs) de IPsec y el incremento de paquetes protegidos
show crypto ipsec sa

# 4. Comprobar la tabla rápida de direccionamiento e interfaces físicas
show ip interface brief
