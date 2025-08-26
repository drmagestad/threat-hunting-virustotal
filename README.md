# Threat Hunting - Suspicious Link Analysis

Este repositorio documenta mi **tercer ejercicio de threat hunting**, basado en el análisis de un enlace sospechoso.  
El objetivo principal es aprender y practicar técnicas de investigación con **VirusTotal**, documentando cada paso y compartiendo hallazgos.

---

## 📌 Contexto
El enlace recibido simulaba ser parte de una plataforma de inversión.  
El proceso descrito era:
1. Ingresar datos personales (nombre, apellido, correo y teléfono).
2. Recibir una llamada de un supuesto "asesor" solicitando una inversión inicial de **10.000 pesos**.
3. (Hipótesis) Posible entrega de un **APK malicioso** en una etapa posterior.  
   
Aclaro que **no avancé en el proceso de la llamada**. Solo investigué el enlace.

---

## 🔍 Análisis realizado

### 1. Análisis inicial en VirusTotal
- El **enlace completo** no arrojaba detecciones sospechosas.  
- Separé el enlace en sus componentes:
  - **Dominio + ruta principal** → quitando los datos ilegibles.
  - Datos ilegibles 
  - s2=eda4b0a0-37c3-4bff-a82b-4220dbf623f8
  - s3=PAZXh0bgNhZW0BMABhZGlkAasifuT2m1QBp7H-ZvsrLXgXdceM4xWIxRyjg7rITIjUquHxdqJMg_nF__8g_nIg7bifVJ-3_aem_zHjuhljbnTAQQaLDECB7IQ
  - s4=2e30efcd-0ee3-43a7-a3c5-b0563004e2e5
  - s5=xm2ee3126dbbyfmjeb6lq

Los mismos estaban concatenados con un &.

### 2. Resultados en VirusTotal
- El **dominio y la ruta** fueron detectados como:
  - **Phishing**
  - **Sospechoso**

 ![Figura 1](/images/2.png)

- En la pestaña **Details** identifiqué:
  - Dirección **IP** asociada.
  - **Body SHA-256 hash** del contenido.
 
 ![Figura 2](/images/3.png)

- Este hash arrojó detección como:  
  - `HTML/hoax.Agent.P` → también conocido como:  
    - `HTML/Phishing.Agent`  
    - `HTML/Phishing.Agent.BDI` 
    - `JS[Phish]`

👉 Distribución habitual: correos con archivos HTML adjuntos o enlaces que redirigen a formularios falsos.

 ![Figura 3](/images/4.png)

### 3. Relaciones y comportamiento
- En **Relations** encontré:
  - Varias **URLs contactadas**.  
  - Más de **55 direcciones IP** a las que intentaba conectarse.  
  - Una de esas IP estaba marcada como **maliciosa** → también fue analizada en VirusTotal.  

---

## 📂 Archivos en este repositorio
- `/reports` → capturas y reportes exportados de VirusTotal.  
- `/images` → imágenes de apoyo (con datos sensibles difuminados).  
- `README.md` → este documento.  

---

## 🎯 Conclusión
Este análisis me permitió:
- Practicar el uso de **VirusTotal** en un caso real.  
- Entender mejor cómo identificar **indicadores de compromiso (IOCs)**.  
- Reconocer patrones típicos de campañas de **phishing**.  

Es mi **primer acercamiento al threat hunting**, por lo que el enfoque es básico, pero me ayudó a adquirir experiencia práctica.

---

## ⚠️ Nota de seguridad
- No se incluyen enlaces activos ni archivos maliciosos en este repositorio.  
- Todo el contenido compartido (hashes, capturas y reportes) es únicamente con fines de **aprendizaje**.
