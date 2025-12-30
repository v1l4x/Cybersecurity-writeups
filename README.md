# InspectorBin v1.0 🔍
### Herramienta de Diagnóstico y Auditoría de Entorno para Linux

InspectorBin es una utilidad ligera desarrollada en Bash diseñada para profesionales de seguridad y administradores de sistemas. Su objetivo es realizar un triaje rápido del entorno de ejecución, verificando la integridad de binarios, la seguridad de los directorios y la configuración del sistema.

---

## ✨ Características Principales
- **Validación de Comandos:** Verifica la existencia y la ruta absoluta de binarios esenciales, detectando posibles suplantaciones.
- **Auditoría de Directorios:** Identifica si el script se está ejecutando desde rutas inseguras (ej. `/tmp`, `/var/tmp`) que podrían ser vulnerables a ataques de escritura.
- **Análisis de Permisos:** Comprueba los privilegios de ejecución del script y reporta estados de riesgo.
- **Información del Sistema:** Recopila datos críticos (Hostname, Kernel, PATH) para informes de auditoría rápidos.
- **Interfaz Profesional:** Salida visual optimizada mediante paleta de colores y manejo de señales de sistema (`SIGINT`).

---

## 🚀 Uso y Ejemplos

### Instalación
```bash
git clone [https://github.com/tu-usuario/InspectorBin-Audit](https://github.com/tu-usuario/InspectorBin-Audit)
cd InspectorBin-Audit
chmod +x inspectorbin.sh
```
Comandos Disponibles
La herramienta utiliza un sistema de parámetros para ejecutar tareas específicas:

- `-v`	Verifica si el directorio actual es seguro.
- `-c <comando>`	Verifica la ruta de uno o varios comandos (ej: -c ls nmap).
- `-i`	Muestra un resumen detallado de la información del sistema.
- `-p`	Analiza los permisos de ejecución del archivo.
- `-h`	Muestra el menú de ayuda.

Ejemplo de ejecución completa:

```bash
./inspectorbin.sh -v -i -p -c bash ssh git
```
---
## 🧠 Lógica de Seguridad Aplicada
Como proyecto de ciberseguridad, InspectorBin implementa:

**Control de Errores:** Validación de argumentos para evitar ejecuciones fallidas.

**Higiene de PATH:** Fomenta la verificación de dónde residen los binarios para prevenir el Path Hijacking.

**Manejo de Señales:** Uso de trap para asegurar una salida limpia del programa ante interrupciones del usuario.
