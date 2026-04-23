Para configurar un entorno profesional de Flutter con Firebase, el primer paso crítico es preparar tu máquina para manejar las herramientas de línea de comandos de Google. Aquí tienes la guía técnica detallada para Windows.

---

## 1. Software Necesario: Node.js y NPM
Para instalar el **Firebase CLI** de forma global, necesitas **Node.js**, que incluye automáticamente **NPM** (Node Package Manager).

### Cómo verificar la instalación actual
Antes de instalar nada, abre una terminal (PowerShell o CMD) y ejecuta:

```powershell
node -v
npm -v
```

* **Si ves un número de versión (ej. `v20.11.0`):** Ya está instalado.
* **Si ves "no se reconoce como un comando":** Sigue los pasos de instalación a continuación.

---

## 2. Guía de Instalación: Node.js (Global en Windows)

<Steps>
{/* Reason: El orden es vital; no puedes instalar Firebase CLI sin antes tener Node.js configurado correctamente en el PATH de Windows. */}
  <Step title="Descargar el Instalador" subtitle="Visita nodejs.org">
    Descarga la versión **LTS (Long Term Support)**. Es la más estable para desarrollo profesional.
  </Step>
  <Step title="Ejecutar el Asistente" subtitle="Configuración de PATH">
    Sigue el instalador. **Crucial:** Asegúrate de que la opción "Add to PATH" esté seleccionada (viene marcada por defecto). Esto permite usar `npm` desde cualquier carpeta.
  </Step>
  <Step title="Instalar Herramientas Adicionales" subtitle="Opcional pero recomendado">
    El instalador preguntará si deseas instalar "Tools for Native Modules". Di que sí; esto instalará Chocolatey y Python, necesarios para algunas librerías avanzadas.
  </Step>
  <Step title="Reinicio de Terminal" subtitle="Actualizar variables de entorno">
    Cierra todas las ventanas de terminal abiertas y abre una nueva para que Windows reconozca los nuevos comandos.
  </Step>
</Steps>

---

## 3. Instalación de Firebase CLI (firebase-tools)
Una vez que `npm` funciona, instalamos las herramientas de Firebase para que estén disponibles en todo el sistema.

### Comando de Instalación Global
Ejecuta este comando en tu terminal:
```powershell
npm install -g firebase-tools
```
> **Nota:** El flag `-g` es lo que indica que la instalación es **global**. Esto significa que podrás ejecutar el comando `firebase` desde cualquier carpeta de tu proyecto Flutter.

---

## 4. Comandos Esenciales de Firebase CLI

### Acceso a tu Cuenta de Google
Para vincular tu terminal con tus proyectos de la consola de Firebase:
1.  Escribe: `firebase login`
2.  Se abrirá una ventana en tu navegador.
3.  Selecciona tu cuenta de Google y otorga los permisos.
4.  Al terminar, la terminal mostrará: `✔  Success! Logged in as user@gmail.com`

### Comandos más utilizados en Flutter
| Comando | Propósito |
| :--- | :--- |
| `firebase projects:list` | Muestra todos tus proyectos activos en la consola. |
| `firebase init` | Inicia la configuración de servicios (Firestore, Functions, Hosting) en tu carpeta local. |
| `firebase login:logout` | Cierra la sesión actual por seguridad. |

---

## 5. Integración Final con Flutter
Para que Flutter reconozca Firebase de forma nativa, Google recomienda usar el ejecutable **FlutterFire CLI** después de haber instalado Firebase Tools:

```powershell
dart pub global activate flutterfire_cli
```

Luego, dentro de tu carpeta `xflutterdominique0726/crudzoologico`, ejecutas:
```powershell
flutterfire configure
```
Este comando detectará tus proyectos de la consola y generará automáticamente el archivo `firebase_options.dart` con todas las credenciales necesarias.

<FollowUp label="¿Quieres que te explique cómo configurar FlutterFire CLI para generar el archivo de opciones automáticamente?" query="Explícame cómo configurar FlutterFire CLI paso a paso para generar el archivo firebase_options.dart en mi proyecto." />
