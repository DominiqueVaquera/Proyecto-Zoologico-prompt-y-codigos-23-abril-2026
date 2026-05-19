Actua como un experto desarrollador de software, crea una aplicación multiplataforma
(Android, iOS, Web) para gestión y consulta de información de un zoológico. Incluirá
autenticación segura, catálogo de animales, información de recintos, perfiles de
usuario y sincronización en tiempo real. El desarrollo se centrará en arquitectura
mantenible, UX intuitiva y escalabilidad.
Framework: dart flutter, IDE Principal: VS Code (con extensiones oficiales), IDE Alternativo:
Android Studio / IntelliJ IDEA (nota: "Antigravity" no es un IDE reconocido para Flutter; se
recomienda VS Code o Android Studio) Backend / BaaS Firebase (Auth, Firestore,
Crashlytics, Analytics) Control de Versiones Git + GitHub / GitLab Diseño UI/UX Figma o
Adobe XD (wireframes, prototipos, design system) Pruebas / Emulación Android Emulator,
iOS Simulator, Chrome Web, dispositivo físico Gestión de Entorno Flutter SDK, Dart SDK,
Firebase CLI, Node.js (si se usan scripts).
Arquitectura: Feature-first + Clean-ish (separación por capas: presentation, domain, data,
core) State Management: Provider (gestión de estado local y global, inyección de
dependencias, ChangeNotifier) Navegación: Enrutamiento declarativo con rutas nombradas
o paquete de enrutamiento tipo go_router Persistencia Local: shared_preferences (sesión
básica, preferencias) + caché de imágenes Seguridad: Reglas de Firestore, validación en
cliente y servidor, manejo de errores centralizado
Dependencias Principales (pubspec.yaml) (Solo lista nominal para referencia futura en el
archivo de configuración) Core Flutter: flutter, flutter_lints Firebase: firebase_core,
firebase_auth, cloud_firestore, firebase_crashlytics, firebase_analytics Estado: provider
Navegación: go_router o auto_route UI/Assets: cached_network_image, flutter_svg,
google_fonts, intl, flutter_screenutil o responsive_framework Utilidades: shared_preferences,
envied o flutter_dotenv, uuid, http o dio (si se requiere API externa), equatable / freezed
(modelos inmutables, opcional) Pruebas: flutter_test, mocktail, firebase_auth_mocks,
cloud_firestore_mocks
Identidad Visual: Paleta inspirada en naturaleza (verdes, tierras, azules suaves), tipografía
legible, iconografía lineal/natural. Design System: Tokens de color, espaciado, elevación,
bordes, estados de botón (default, hover, pressed, disabled). Accesibilidad: Contraste
WCAG AA, escalado de texto, soporte para lectores de pantalla, navegación por teclado
(web). Flujos Clave: Onboarding → Registro/Inicio de sesión → Home → Detalle de animal
→ Perfil → Ajustes Responsive: Layouts adaptativos (grid en tablets/desktop, listas en
móviles), manejo de orientación y safe areas. Prototipado: Validar en Figma antes de
codificar; definir estados de carga, vacío, error y éxito.
Fase 1: Configuración Inicial del Proyecto Instalar Flutter SDK, Dart y VS Code con
extensiones oficiales. Ejecutar flutter create zoo_app y verificar ejecución en al menos 2
plataformas. Configurar .gitignore, estructura de carpetas inicial y repositorio remoto.
Establecer convenciones de nombre, formateo (dart format) y linting. Fase 2: Integración
con Firebase Crear proyecto en Firebase Console. Registrar apps para Android, iOS y Web.
Descargar/agregar archivos de configuración (google-services.json,
GoogleService-Info.plist, firebase-web-config). Ejecutar firebase init y flutterfire configure
para generar firebase_options.dart. Verificar conexión básica imprimiendo logs de
inicialización. Fase 3: Arquitectura y State Management Crear estructura por capas (lib/core,
lib/features/auth, lib/features/zoo, lib/providers, lib/services, lib/models). Configurar Provider
a nivel raíz (MultiProvider) con inyección de servicios y modelos. Definir ChangeNotifier para
estado global (usuario autenticado, carga global, tema). Implementar router base con rutas
protegidas y públicas. Fase 4: UI/UX y Navegación Base Importar design system (colores,

tipografías, temas claro/oscuro). Crear widgets reutilizables: AppButton, AppTextField,
LoadingOverlay, EmptyState, ErrorBanner. Implementar estructura de navegación
inferior/superior según prototipo. Validar responsividad y accesibilidad en múltiples
resoluciones. Fase 5: Autenticación (Email / Password) Habilitar método Email/Password en
Firebase Console. Crear vistas: LoginScreen, RegisterScreen, ForgotPasswordScreen.
Implementar validaciones en cliente (formato email, longitud contraseña, coincidencia).
Conectar con FirebaseAuth mediante un servicio aislado (AuthService). Gestionar estados:
carga, éxito, error; persistir sesión automáticamente. Implementar logout y limpieza de
estado/local storage. Agregar reenvío de verificación de email (opcional pero recomendado).
Fase 6: Firestore y Modelo de Datos Diseñar colecciones principales: users: perfil, rol,
preferencias, historial animals: nombre, especie, hábitat, edad, estado, imágenes, recinto
habitats / enclosures: nombre, capacidad, ubicación, horarios visits / bookings (si aplica):
fecha, estado, usuario asociado Definir índices compuestos necesarios para
búsquedas/filtros. Configurar reglas de seguridad en Firestore Console (lectura pública de
catálogo, escritura restringida a roles). Crear repositorios/servicios para CRUD:
AnimalRepository, UserRepository. Implementar escuchas en tiempo real (snapshots) para
datos dinámicos. Habilitar persistencia offline y manejo de conflictos básicos. Fase 7:
Funcionalidades del Zoológico Home/Dashboard: Carrusel destacado, categorías, acceso
rápido a mapa/horarios. Catálogo de Animales: Lista con búsqueda, filtros por
especie/hábitat, paginación/lazy loading. Detalle de Animal: Galería, descripción, cuidados,
curiosidades, recinto asociado. Perfil de Usuario: Editar datos, cambiar contraseña, ver
historial, preferencias. Mapa/Recorridos: Integración estática o con paquete de mapas,
marcadores de recintos. Notificaciones (opcional): Firebase Cloud Messaging para eventos,
alimentaciones o cierres. Ajustes: Idioma, tema, caché, cerrar sesión, políticas/privacidad.
Fase 8: Pruebas, Optimización y Despliegue Pruebas Unitarias: Servicios, modelos, lógica
de negocio. Pruebas de Widget: Flujos de UI, validaciones, estados de carga/error. Pruebas
de Integración: Auth completo, Firestore CRUD offline/online, navegación protegida.
Optimización: Reducción de imágenes, lazy loading, profiling de rendimiento, análisis de
memory leaks. Monitoreo: Configurar Crashlytics y Analytics para métricas en producción.
Build: Generar APK/AAB, IPA (con certificado), build web estático. Despliegue: Play
Console, TestFlight, Firebase Hosting (web), CI/CD básico (GitHub Actions opcional).
Seguridad: Nunca exponer claves sensibles en código; usar flutterfire configure y variables
de entorno. Reglas Firestore: Validar en servidor, no confiar solo en cliente; restringir
escrituras por request.auth.uid. Estado: Mantener Provider acotado; evitar estados globales
innecesarios; usar Selector para rebuilds precisos. Offline First: Diseñar UI que tolere
desconexión; usar cache y sincronización diferida. Accesibilidad: Etiquetas semánticas,
navegación por foco, contraste validado. Plataformas: Verificar comportamientos específicos
(iOS safe area, Android back button, web routing). Versionado: Semantic versioning,
CHANGELOG, releases etiquetados.
Validar este plan con stakeholders o equipo. Diseñar wireframes y prototipo navegable en
Figma. Definir modelo de datos detallado y reglas de Firestore. Confirmar alcance de
funcionalidades MVP vs. futuras versiones. Solicitar generación de código por fase (ej.:
Fase 3 + 5 o Fase 6).
Módulo de fauna — el corazón del sistema. ANIMAL es la entidad central, vinculada a
ESPECIE (con clasificación taxonómica y estado de conservación IUCN) y a HABITAT. La
historia clínica de cada animal se registra en HISTORIAL_MEDICO. Módulo de
instalaciones — HABITAT agrupa los recintos, que a su vez se organizan en ZONA (ej. zona
africana, zona acuática). Permite controlar capacidad, clima y superficie por recinto. Módulo

de personal — EMPLEADO con su CARGO (cuidador, veterinario, guía, etc.). Los
VETERINARIO son una especialización implícita que puede normalizarse como subtipo o
como flag en EMPLEADO. Módulo de visitantes — VISITANTE realiza VISITA a una o varias
zonas. Permite llevar taquilla, control de aforo y estadísticas de asistencia. Módulo de
alimentación y suministros — ALIMENTACION registra cada toma (qué animal, qué
alimento, cuánto, quién lo hizo). SUMINISTRO y PROVEEDOR gestionan las entradas de
insumos (alimento, medicamentos, equipos).
dame una carpeta para modelos
Aqui estan los 5 modulos en la imagen

Usa las siguientes imagenes

Recuerda usar colores atractivos y tenues
