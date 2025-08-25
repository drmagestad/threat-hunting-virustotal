# Threat Hunting - Suspicious Link Analysis

Este repositorio documenta mi **primer ejercicio de threat hunting**, basado en el análisis de un enlace sospechoso recibido de forma externa.  
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
  - **Dominio + ruta principal**
  - Datos ilegibles (`s2`, `s3`, `s4`, `s5`) → descartados por falta de valor en el análisis.  

### 2. Resultados en VirusTotal
- El **dominio y la ruta** fueron detectados como:
  - **Phishing**
  - **Sospechoso**
- En la pestaña **Details** identifiqué:
  - Dirección **IP** asociada.
  - **Body SHA-256 hash** del contenido.
- Este hash arrojó detección como:  
  - `HTML/hoax.Agent.P` → también conocido como:  
    - `HTML/Phishing.Agent`  
    - `HTML/Phishing.Agent.BDI` (ESET)  
    - `JS[Phish]` (Avast)  

👉 Distribución habitual: correos con archivos HTML adjuntos o enlaces que redirigen a formularios falsos.

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
