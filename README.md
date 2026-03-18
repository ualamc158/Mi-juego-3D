# Paintball Shooter 3D - Proyecto FPS

Prototipo funcional de un videojuego de disparos en primera persona (FPS) en 3D, desarrollado en Unity. Este proyecto demuestra la integración de sistemas avanzados como inteligencia artificial mediante NavMesh, máquinas de estados, gestión de inventario, interfaz de usuario (HUD) y efectos multimedia.

---

## 1. Concepto y Escenario

* **Título:** Paintball Shooter 3D
* **Diseño (RA1.4):** El mapa está diseñado en torno a una aldea con una clara **verticalidad** y flujo lógico. Las montañas circundantes delimitan el espacio de juego, mientras que las casas, muros y árboles actúan como zonas de cobertura estratégicas para evitar los disparos de los enemigos.
* **Assets (RA1.5):** Se ha utilizado el paquete *Low Poly Fantasy Village* de OccaSoftware para el entorno, junto con modelos gratuitos para personajes humanos animados (mixamo/estándar). Los materiales han sido configurados para integrarse correctamente con la iluminación de la escena.

## 2. Mecánicas del Jugador (RA3)

* **Controlador (RA3.2):** El movimiento del jugador se realiza mediante el sistema clásico **WASD** (script `ControlJugador`), permitiendo un movimiento fluido por el escenario. La cámara está vinculada al movimiento del ratón, utilizando `CursorLockMode.Locked` para mantener el foco en la acción y apuntar con precisión.
* **Inventario (RA3.3):** El jugador dispone de un sistema de armas administrado por el script `InventarioArmas`, permitiendo alternar entre diferentes tipos de armamento (Pistola y Fusil). Por el mapa se pueden recolectar *Gemas* y otros items para gestionar la munición. Para optimizar el rendimiento, los proyectiles utilizan un sistema de **Object Pooling**.
* **Salud (RA3.4):** El jugador tiene un sistema de vidas gestionado internamente. Al recibir impactos de las balas enemigas (que distinguen bando gracias al script `ControlBala`), el jugador pierde vida. Este cambio se refleja inmediatamente en pantalla gracias al **HUD dinámico** (`ControlHUD`), que también muestra la munición restante en tiempo real. Los recolectables de tipo *Corazón* permiten recuperar salud.

## 3. Inteligencia Artificial (RA4)

* **Navegación (RA4.5):** Todo el escenario estático (casas, puentes, rocas y montañas) ha sido "bakeado" usando el sistema moderno **NavMesh Surface**, permitiendo a los agentes esquivar obstáculos complejos sin quedarse atascados.

* **Comportamiento (RA4.1/4.3):** Los enemigos utilizan una **Máquina de Estados Finita** programada a medida (`ControlEnemigoMejorado`). 
  * **Patrullaje:** Caminan de forma cíclica entre una serie de *Waypoints* predefinidos.
  * **Persecución:** Si detectan al jugador a cierta distancia, interrumpen la patrulla, activan su animación de correr mediante el parámetro de velocidad del *Animator* (Blend Trees) e inician la caza. Se detienen a 3 metros de distancia para evitar colisiones físicas erráticas y proceden a disparar.
  * **Reacción al daño y Muerte:** Al recibir un impacto, reproducen una animación de daño y retoman la persecución. Al perder toda la vida, se eliminan del contador global (`ControlJuego`), activando la victoria cuando no queda ninguno vivo.

## 4. Multimedia (RA3.5)

* **Efectos Visuales (VFX):** Se han implementado *Particle Systems* (sistemas de partículas) que se instancian dinámicamente (`ParticulasExplosionFusil` y `ParticulasExplosionPistola`) en el punto exacto donde la bala colisiona, proporcionando feedback visual inmediato de los impactos.
* **Audio:** El juego cuenta con efectos de sonido (SFX) para los disparos (`Disparo.wav`, `PistolaSonido.wav`) usando componentes **Audio Source** configurados en modo **Audio 3D espacial (Spatial Blend 1.0)**, lo que permite al jugador localizar auditivamente la posición de los enemigos por el origen del sonido.

---
