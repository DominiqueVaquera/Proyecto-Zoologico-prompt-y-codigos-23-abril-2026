¡Hola! Como desarrollador de software, he diseñado esta guía estructurada para que tus estudiantes no solo aprendan a programar un CRUD, sino a entender la arquitectura detrás de una aplicación profesional en Flutter utilizando Firebase.

---

## 1. Metodología: Agentes y Flujo de Trabajo
Para este proyecto, utilizaremos una metodología basada en **Roles y Responsabilidades**. Esto ayuda a los estudiantes a entender que el código se divide por su función.

| Agente | Rol | Skill Clave |
| :--- | :--- | :--- |
| **Arquitecto de Datos** | Configurar Firebase Console | Manejo de NoSQL (Firestore) |
| **Ingeniero de DevOps** | Configuración de Entorno | Gestión de dependencias y CLI |
| **Desarrollador Logic** | Implementar el CRUD | Programación Asíncrona (Streams/Futures) |
| **Diseñador UI/UX** | Interfaz de Usuario | Manejo de Widgets y Estética |

---

## 2. Preparación del Entorno y Librerías
Primero, cumplimos con la estructura de carpetas solicitada y la integración de dependencias.

### Estructura de Carpetas (Terminal)
```bash
mkdir xflutterdominique0726
cd xflutterdominique0726
flutter create crudzoologico
cd crudzoologico
```

### Configuración de `pubspec.yaml`
Para integrar Firebase y la estética de **Antigravity** (que interpretaremos como una interfaz moderna, fluida y con alto contraste/colores atractivos), añade estas líneas:

```yaml
dependencies:
  flutter:
    sdk: flutter
  # Core de Firebase
  firebase_core: ^2.24.2
  # Base de datos Firestore
  cloud_firestore: ^4.14.0
  # Diseño moderno
  google_fonts: ^6.1.0
```
*Ejecuta `flutter pub get` en la terminal para descargar las librerías.*

---

## 3. Configuración en Firebase Console
1. Ve a [Firebase Console](https://console.firebase.google.com/).
2. Crea un proyecto llamado `crudzoologico`.
3. En el menú lateral, ve a **Firestore Database**.
4. Haz clic en **Crear base de datos** y selecciona "Modo de prueba" para facilitar el aprendizaje inicial.
5. Crea una **Colección** llamada `animales`. No es necesario agregar documentos aún, el código lo hará por ti.

---

## 4. Implementación del Código (Práctica Guiada)

Para que el proyecto sea funcional, dividiremos el código en tres archivos clave dentro de la carpeta `lib/`.

### A. Inicialización (`lib/main.dart`)
Este archivo arranca Firebase y la aplicación.

```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'package:google_fonts/google_fonts.dart';
import 'home_page.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(const ZooApp());
}

class ZooApp extends StatelessWidget {
  const ZooApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'CRUD Zoológico Dominique',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.deepPurple,
          brightness: Brightness.dark,
        ),
        textTheme: GoogleFonts.poppinsTextTheme(ThemeData.dark().textTheme),
      ),
      home: const HomePage(),
    );
  }
}
```

### B. El Motor de Datos (`lib/firebase_service.dart`)
Aquí vive la lógica del CRUD.

```dart
import 'package:cloud_firestore/cloud_firestore.dart';

FirebaseFirestore db = FirebaseFirestore.instance;

// CREATE
Future<void> addAnimal(String nombre, String color, int cantidad) async {
  await db.collection('animales').add({
    'nombre': nombre,
    'color': color,
    'cantidad': cantidad,
  });
}

// READ (Stream para actualización en tiempo real)
Stream<QuerySnapshot> getAnimales() {
  return db.collection('animales').snapshots();
}

// UPDATE
Future<void> updateAnimal(String id, String nombre, String color, int cantidad) async {
  await db.collection('animales').doc(id).update({
    'nombre': nombre,
    'color': color,
    'cantidad': cantidad,
  });
}

// DELETE
Future<void> deleteAnimal(String id) async {
  await db.collection('animales').doc(id).delete();
}
```

### C. Interfaz de Usuario (`lib/home_page.dart`)
Diseño con colores vibrantes y controles para el usuario.

```dart
import 'package:flutter/material.dart';
import 'firebase_service.dart';

class HomePage extends StatefulWidget {
  const HomePage({super.key});
  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  final TextEditingController _nombreCtrl = TextEditingController();
  final TextEditingController _colorCtrl = TextEditingController();
  final TextEditingController _cantidadCtrl = TextEditingController();

  void _showForm(String? id) {
    showModalBottomSheet(
      context: context,
      isScrollControlled: true,
      backgroundColor: Colors.grey[900],
      builder: (_) => Padding(
        padding: EdgeInsets.only(bottom: MediaQuery.of(context).viewInsets.bottom, top: 20, left: 20, right: 20),
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            TextField(controller: _nombreCtrl, decoration: const InputDecoration(labelText: 'Nombre del Animal')),
            TextField(controller: _colorCtrl, decoration: const InputDecoration(labelText: 'Color')),
            TextField(controller: _cantidadCtrl, decoration: const InputDecoration(labelText: 'Cantidad'), keyboardType: TextInputType.number),
            const SizedBox(height: 20),
            ElevatedButton(
              style: ElevatedButton.styleFrom(backgroundColor: Colors.greenAccent, foregroundColor: Colors.black),
              onPressed: () async {
                if (id == null) {
                  await addAnimal(_nombreCtrl.text, _colorCtrl.text, int.parse(_cantidadCtrl.text));
                } else {
                  await updateAnimal(id, _nombreCtrl.text, _colorCtrl.text, int.parse(_cantidadCtrl.text));
                }
                _nombreCtrl.clear(); _colorCtrl.clear(); _cantidadCtrl.clear();
                Navigator.pop(context);
              },
              child: Text(id == null ? 'Agregar' : 'Actualizar'),
            )
          ],
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Zoologico Dominique 0726'), centerTitle: true),
      floatingActionButton: FloatingActionButton(
        backgroundColor: Colors.cyanAccent,
        onPressed: () => _showForm(null),
        child: const Icon(Icons.add, color: Colors.black),
      ),
      body: StreamBuilder(
        stream: getAnimales(),
        builder: (context, snapshot) {
          if (!snapshot.hasData) return const Center(child: CircularProgressIndicator());
          return ListView.builder(
            itemCount: snapshot.data!.docs.length,
            itemBuilder: (context, index) {
              var doc = snapshot.data!.docs[index];
              return Card(
                color: Colors.indigo[900],
                margin: const EdgeInsets.all(8),
                child: ListTile(
                  title: Text(doc['nombre'], style: const TextStyle(color: Colors.white, fontWeight: FontWeight.bold)),
                  subtitle: Text("Color: ${doc['color']} | Cantidad: ${doc['cantidad']}"),
                  trailing: Row(
                    mainAxisSize: MainAxisSize.min,
                    children: [
                      IconButton(icon: const Icon(Icons.edit, color: Colors.orangeAccent), onPressed: () {
                        _nombreCtrl.text = doc['nombre'];
                        _colorCtrl.text = doc['color'];
                        _cantidadCtrl.text = doc['cantidad'].toString();
                        _showForm(doc.id);
                      }),
                      IconButton(icon: const Icon(Icons.delete, color: Colors.redAccent), onPressed: () => deleteAnimal(doc.id)),
                    ],
                  ),
                ),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

## 5. Resumen de Implementación
<Steps>
{/* Reason: El orden de estos pasos es crítico para que la aplicación conecte con Firebase correctamente. */}
  <Step title="Configurar Firebase" subtitle="Consola Web">
    Crea el proyecto y habilita Firestore en modo de prueba. Registra tu app Android/iOS para descargar el archivo `google-services.json`.
  </Step>
  <Step title="Instalar Dependencias" subtitle="pubspec.yaml">
    Añade `firebase_core` y `cloud_firestore`. Ejecuta `flutter pub get`.
  </Step>
  <Step title="Crear Servicios" subtitle="Logística Dart">
    Implementa las funciones asíncronas para conectar Flutter con la base de datos `animales`.
  </Step>
  <Step title="Construir UI" subtitle="Antigravity Style">
    Usa el widget `StreamBuilder` para que los cambios en la consola de Firebase se vean instantáneamente en el celular.
  </Step>
</Steps>

<Elicitations message="¿En qué área te gustaría profundizar para tus estudiantes?">
  <Elicitation label="Explicar el uso de Streams" query="Explícame detalladamente cómo funciona el StreamBuilder en Flutter para que los estudiantes entiendan la reactividad con Firebase." />
  <Elicitation label="Añadir validación de datos" query="¿Cómo puedo agregar validación a los campos del formulario para que no acepten valores vacíos o cantidades negativas?" />
</Elicitations>
