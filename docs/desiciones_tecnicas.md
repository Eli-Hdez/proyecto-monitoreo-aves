# Decisiones Técnicas - Proyecto Monitoreo de Agua para Aves

## Arquitectura General
**Decisión:** Sistema cliente-servidor con microcontrolador ESP32 y backend en Python Flask.

**Justificación:** 
- Separación de responsabilidades entre hardware y software
- Escalabilidad para agregar más sensores en el futuro
- Mantenimiento independiente de cada componente

## Hardware

### Microcontrolador: ESP32 DevKit
**Alternativas consideradas:** Arduino Uno + módulo WiFi, Raspberry

**Decisión final:** ESP32 DevKit
- **Razones:**
  - WiFi integrado sin componentes adicionales
  - Suficiente potencia de procesamiento para lectura de pulsos
  - Compatibilidad con Arduino IDE (fácil programación)
  - Bajo costo
  - Amplia comunidad y documentación

### Medidor de Agua: DN20 con salida de pulsos
**Decisión:** Watflow DN20 con reed switch
- **Razones:**
  - Precisión adecuada para medición de consumo
  - Salida digital fácil de interpretar por microcontrolador
  - Compatibilidad con tubería estándar de 3/4"
  - Costo accesible

### Optoacoplador: 4N35
**Decisión:** Aislamiento eléctrico entre medidor y ESP32
- **Razones:**
  - Protección del ESP32 contra picos de voltaje
  - Prevención de ground loops
  - Bajo costo
  - Fácil implementación

## Software Backend

### Framework Web: Flask (Python)
**Alternativas:** Django, FastAPI, Node.js

**Decisión:** Flask
- **Razones:**
  - Curva de aprendizaje suave
  - Ideal para APIs REST simples
  - Flexibilidad en la estructura del proyecto
  - Suficiente para los requisitos del sistema
  - Amplia compatibilidad con librerías de IA

### Base de Datos: SQLite
**Alternativas:** PostgreSQL, MySQL, InfluxDB

**Decisión:** SQLite
- **Razones:**
  - Cero-configuration, no requiere servidor separado
  - Adecuada para volumen de datos del proyecto (1 lectura/5min)
  - Backup simple (copia de archivo)
  - Ideal para aplicación single-user
  - Compatibilidad nativa con Python

## Frontend

### Tecnologías: HTML/CSS/JavaScript vanilla + Chart.js
**Alternativas:** React, Vue.js, Angular

**Decisión:** Tecnologías web estándar
- **Razones:**
  - Baja complejidad para interfaz de monitoreo
  - Sin dependencias de build tools
  - Fácil despliegue integrado con Flask

## Inteligencia Artificial

### Modelo: LSTM (Long Short-Term Memory)
**Alternativas:** ARIMA, Prophet, Redes Neuronales Simples

**Decisión:** LSTM
- **Razones:**
  - Especializado en series temporales
  - Capacidad de aprender patrones estacionales
  - Buen desempeño con datos de consumo de agua
  - Implementación disponible en TensorFlow/Keras

### Framework: TensorFlow/Keras
**Alternativas:** PyTorch, Scikit-learn

**Decisión:** TensorFlow/Keras
- **Razones:**
  - API de alto nivel (Keras) fácil de usar
  - Buen soporte para modelos secuenciales
  - Compatibilidad con despliegue en diversos entornos
  - Amplia documentación y comunidad

## Comunicación

### Protocolo: HTTP REST
**Alternativas:** MQTT, WebSockets, gRPC

**Decisión:** HTTP REST
- **Razones:**
  - Simplicidad de implementación
  - Fácil debugging con herramientas estándar
  - Suficiente para intervalos de 5 minutos entre envíos
  - Compatibilidad universal

## Consideraciones de Despliegue

### Entorno de Producción: Single-server
**Decisión:** Todo en un solo servidor (Flask + SQLite)
- **Razones:**
  - Bajo costo de operación
  - Fácil mantenimiento
  - Suficiente para los requisitos de performance
  - Puede hostearse en Raspberry

## Decisiones de Seguridad

### Autenticación: Simple API key
**Decisión:** Token estático para el ESP32
- **Razones:**
  - Baja complejidad para dispositivo embebido
  - Suficiente para entorno controlado de granja
  - Puede extenderse a autenticación más robusta si es necesario

---

## Resumen de Stack Tecnológico

| Capa | Tecnología | Justificación |
|------|------------|---------------|
| Hardware | ESP32 + Medidor DN20 | Balance costo-funcionalidad |
| Comunicación | WiFi + HTTP REST | Simplicidad y compatibilidad |
| Backend | Flask + SQLite | Rapid development y bajo mantenimiento |
| Frontend | HTML/JS/CSS + Chart.js | Suficiente para dashboard de monitoreo |
| IA | TensorFlow/Keras + LSTM | Especializado en series temporales |
| Control Versiones | Git + GitHub | Estándar industria y portafolio |