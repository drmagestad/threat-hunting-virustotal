# Threat Hunting - Suspicious Link Analysis

Este repositorio documenta un **ejercicio de threat hunting**, basado en el análisis de un enlace sospechoso.
El objetivo principal es **aprender y practicar técnicas de investigación con VirusTotal**, documentando cada paso y compartiendo hallazgos.

---

## 📌 Contexto
El enlace recibido simulaba ser parte de una **plataforma de inversión**.
El proceso descrito era:

  - Ingresar datos personales (**nombre, apellido, correo y teléfono**).

  - Recibir una llamada de un supuesto **"asesor"** solicitando una **inversión inicial de 10.000 pesos.**

**(Hipótesis)** Posible entrega de un APK malicioso en una etapa posterior.

Aclaro que **no avancé en el proceso de la llamada**. Solo investigué el enlace.  

---

## 🔍 Análisis realizado

### 1. Análisis inicial en VirusTotal
El enlace completo no arrojaba detecciones sospechosas.
Separé el enlace en sus componentes:
Dominio + ruta principal → quitando los datos ilegibles.
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

- **Este hash arrojó detección como:**  
  - `HTML/hoax.Agent.P` → también conocido como:  
    - `HTML/Phishing.Agent`  
    - `HTML/Phishing.Agent.BDI` (ESET)  
    - `JS[Phish]` (Avast)

**Distribución habitual:** correos con archivos HTML adjuntos o enlaces que redirigen a formularios falsos.

 ![Figura 3](/images/4.png)

**Al revisar los resultados**, noté que **el análisis disponible en VirusTotal** había sido realizado **hacía un tiempo.**
Por eso, decidí **repetir el análisis** para obtener **información más actualizada.**

 ![Figura 4](/images/5.png)

- **Resultados adicionales**:  
  - Aparecieron **más detecciones y etiquetas** que antes no estaban presentes.

- **El nuevo análisis del hash arrojó detección como:**  
  - `Trojan:Win/Hoax.Akgpp` (AliCloud)  
  - `Other:SNH-gen [Phish]` (AVG, Avast)  
  - `HTML/Hoax.Agent.P` (ESET-NOD32)  
  - `HTML.Trojan.Agent.10JMG5` (GData)

### 3. Relaciones
  - En la sección **Relations**, dentro de **Contacted URLs**, no encontré nada que me llamara la atención.
  - Si bien las conexiones aparecen como **HTTP**, corresponden a servicios legítimos (**Google y Let's Encrypt**) y no representan riesgo, ya que los datos transmitidos no son sensibles ni maliciosos.

 ![Figura 5](/images/6.png)

  - Dentro de **Contacted IP addresses** observé mas de **55 direcciones IP**.
  - Algunas de las direcciones IP fueron **identificadas como maliciosas** y analizadas posteriormente en VirusTotal. 

 <div align="center">
  
  ![Figura 6](/images/7.png)

</div>

  - Cabe destacar que la detección **provino de un único motor antivirus**, pero se observó en tres direcciones IP distintas.  
  - Esto **no garantiza que las direcciones IP sean maliciosas**, ya que podría tratarse de un **falso positivo**.  
  - En algunos casos, la alerta se debe a la **reputación histórica** de la IP, y no a actividad maliciosa activa.  

  ![Figura 7](/images/8.png)

  ![Figura 7](/images/8.1.png)

  ![Figura 7](/images/8.2.png)


VirusTotal reporta **varios archivos** que se han comunicado con la dirección IP analizada, con **distintos niveles de detección**.

  ![Figura 9](/images/10.png)

- Algunos archivos presentan **alta detección**, indicando que la dirección IP ha estado relacionada con **malware o software potencialmente peligroso**.  
- Otros archivos no muestran detecciones, lo que podría deberse a **archivos legítimos o falsos positivos**.

Además, en la sección **Contained in Graphs**, la dirección IP aparece vinculada a **múltiples actividades maliciosas documentadas por la comunidad**, como campañas relacionadas con **KoiLoader, Quakbot, HijackLoader y DarkCloud**.

  ![Figura 10](/images/11.png)

- En conjunto, estos hallazgos refuerzan la hipótesis de que la dirección IP **no es confiable** y puede ser utilizada como **vector de phishing o distribución de malware**.

Volviendo al **hash** analizado anteriormente, lo último que revisaremos es la sección **Behavior**, que muestra las acciones observadas durante la ejecución o interacción de los archivos asociados con el dominio analizado.

### 4. Comportamiento

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
