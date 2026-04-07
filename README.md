# Visor 3D Interactivo — Museo Guanacaste

https://museo3d.netlify.app/

Este proyecto consiste en una aplicación web interactiva desarrollada para visualizar de manera óptima y fluida un modelo tridimensional pesado generado mediante herramientas de fotogrametría (OpenDroneMap).

## 🚀 Objetivo del Desarrollo

El objetivo principal era construir una alternativa en formato de visor local robusto al Sandbox genérico de Babylon.js, sin recurrir a software de pago (suscripciones a plataformas GIS) ni requerir instalar componentes adicionales pesados para el usuario final. 

El entorno utiliza las librerías CDN nativas de **Babylon.js** (100% de código abierto) a través de un simple index `.html`.

## 🛠️ Resumen del Proceso y Decisiones Técnicas

### 1. Cambio de Formato Base (GLB vs OBJ)
*   **Intento Inicial:** Se construyó originalmente utilizando el archivo `.glb` de 305 MB.
*   **Problema detectado:** El exporte automático introducía traslaciones negativas que causaban un comportamiento donde las piezas secundarias (como los techos) se "hundían", además de una desorientación por conversión del eje de proyección (de Z-UP de fotogrametría a Y-UP web).
*   **Solución Óptima:** Se descartó y eliminó el archivo `.glb` del disco duro. El visor se reconfiguró para consumir directamente la raíz limpia exportada por ODM: el archivo `odm_textured_model_geo.obj` asociado directamente a su carpeta de dependencias `odm_texturing`. Gracias a este giro, todos los problemas estructurales desaparecieron matemáticamente.

### 2. Orientación, Cámara y Encuadre Matemático (Alineación)
*   Las coordenadas de la cámara se ajustaron específicamente y *hardcodearon* a un ángulo isométrico superior particular con los valores precisos logrados en campo para representar adecuadamente la manzana del museo, anulando al auto-encuadre inicial:
    *   **Alpha:** 4.5648
    *   **Beta:** 0.3314
    *   **Radius:** 350.72
    *   **Center Target (x, y, z):** Compensación estricta para ubicar el centro topográfico.

### 3. Interfaz de Usuario e Interacción
*   **Efectos visuales UI:** Se creó un diseño premium oscuro "glassmorphism", en lugar de ventanas crudas.
*   **Panel de Luces:** Viene precargado por defecto abierto y centrado en la opción de iluminación de entorno de **"Estudio"** (Intensity: 2.2 / Exposure: 1.6), realzando el contraste y color topológico.
*   **Herramientas de Soporte:** Modo `Wireframe` para debugeo técnico, Información paramétrica del Modelo, Autogiro natural del modelo y Herramienta exportadora de coordenadas de plano de cámara a través de teclado (`KeyC`).

## 💻 Ejecución General

Para hacer funcionar o probar el proyecto sin restricciones de seguridad (*CORS*) respecto a archivos grandes `.obj`:
1. Asegurarse de tener `Node.js` inicializado.
2. Levantar el puerto con el comando ligero oficial de entorno en PowerShell o CMD:
   ```cmd
   npx -y http-server . -p 8081 -c-1 --cors
   ```
3. Visitar `http://127.0.0.1:8081/` en el navegador web local.
