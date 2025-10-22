# GeoFoto: Cámara con Geolocalización

## Descripción General

GeoFoto es una aplicación móvil desarrollada con Ionic + Angular + Capacitor, que permite al usuario tomar fotografías y registrar automáticamente su ubicación GPS (latitud, longitud, precisión y fecha).

Los datos de cada foto se almacenan junto con un enlace directo a Google Maps, y se guardan en un archivo de texto (ubicaciones.txt) dentro del dispositivo para facilitar su consulta posterior.

## Tecnologías Utilizadas

- Ionic Framework (Angular)
- Capacitor Plugins:
  - @capacitor/camera - Captura de fotos
  - @capacitor/geolocation - Obtención de coordenadas GPS
  - @capacitor/filesystem - Lectura/escritura de archivos
  - @capacitor/preferences - Persistencia de datos
- TypeScript - Lenguaje de programación
- Android Studio - Compilación y pruebas del APK

## Funcionalidades Principales

- Captura de fotos con la cámara del dispositivo
- Obtención de coordenadas GPS en tiempo real (latitud, longitud, precisión)
- Almacenamiento de fotos en memoria interna
- Creación automática de archivo ubicaciones.txt con:
  - Nombre del archivo de imagen
  - Fecha y hora de captura
  - Coordenadas GPS completas
  - Enlace directo a Google Maps
- Eliminación de fotos guardadas
- Persistencia de datos mediante Capacitor Preferences

## Estructura del Proyecto

```
GeoFotoApp/
├── src/
│   ├── app/
│   │   ├── services/
│   │   │   ├── photo.ts
│   │   │   ├── photo.spec.ts
│   │   │   ├── geolocation.ts
│   │   │   └── geolocation.spec.ts
│   │   └── app.module.ts
│   │
│   ├── pages/
│   │   ├── tab1/
│   │   ├── tab2/
│   │   └── tab3/
│   │
│   ├── assets/
│   └── main.ts
│
├── android/
├── capacitor.config.ts
├── ionic.config.json
└── package.json
```

## Requisitos Previos

- Node.js (v18 o superior)
- npm (v9 o superior)
- Ionic CLI
- Android Studio (para compilar APK)
- Java JDK 17

## Instalación del Proyecto

### Paso 1: Crear un nuevo proyecto Ionic

```bash
ionic start GeoFotoApp tabs --type=angular
cd GeoFotoApp
```

### Paso 2: Instalar dependencias necesarias

```bash
npm install @capacitor/camera @capacitor/geolocation @capacitor/filesystem @capacitor/preferences
```

### Paso 3: Crear los servicios

#### Servicio de Geolocalización (src/app/services/geolocation.ts)

Este servicio contiene las funciones para:
- getCurrentLocation() - Obtener la ubicación actual del dispositivo
- generateGoogleMapsLink() - Generar enlace a Google Maps
- saveLocationToFile() - Guardar información en ubicaciones.txt

#### Servicio de Fotos (src/app/services/photo.ts)

Este servicio gestiona la cámara y asocia la ubicación a cada foto:
- addNewToGallery() - Tomar foto y obtener ubicación
- savePicture() - Guardar imagen localmente
- deletePhoto() - Eliminar una foto existente
- loadSavedPhotos() - Cargar fotos al iniciar la app

## Configuración de Capacitor

### Paso 4: Inicializar Capacitor

```bash
npx cap init
```

Durante la inicialización, completa:
- App name: GeoFoto
- Package ID: com.geofoto.app

### Paso 5: Agregar plataforma Android

```bash
ionic build
npx cap add android
```

Esto creará la carpeta android/ con toda la estructura nativa.

### Paso 6: Sincronizar cambios

Cada vez que modifiques el código Angular, ejecuta:

```bash
ionic build
npx cap sync android
```

## Configuración de Permisos

Abre el archivo:

```
android/app/src/main/AndroidManifest.xml
```

### Agregar permisos

Dentro de `<manifest>` pero fuera de `<application>`:

```xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

### Configurar el tag application

```xml
<application
    android:requestLegacyExternalStorage="true"
    android:usesCleartextTraffic="true"
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:theme="@style/AppTheme">
</application>
```

## Pruebas Unitarias

El proyecto incluye pruebas básicas para verificar la creación de los servicios:
- geolocation.spec.ts
- photo.spec.ts

Ejecutar pruebas:

```bash
ng test
```

## Construcción del APK

### Generar APK de depuración

```bash
ionic build
npx cap copy android
npx cap open android
```

### En Android Studio

1. Espera a que el proyecto termine de sincronizar
2. Ve a Build > Build Bundle(s) / APK(s) > Build APK(s)
3. Ejecuta la app en un emulador o dispositivo físico conectado

### Ubicación del APK generado

```
android/app/build/outputs/apk/debug/app-debug.apk
```

## Salida del Archivo ubicaciones.txt

Ejemplo de contenido:

```
FOTO CON UBICACION
==================
Fecha: 21/10/2025, 21:45:00
Archivo: photo_1697937905123.jpeg
Latitud: -0.310226
Longitud: -78.448975
Precision: 8 metros
Google Maps: https://www.google.com/maps?q=-0.310226,-78.448975

----------------------------------------

FOTO CON UBICACION
==================
Fecha: 21/10/2025, 22:15:30
Archivo: photo_1697939730456.jpeg
Latitud: -0.312456
Longitud: -78.450123
Precision: 5 metros
Google Maps: https://www.google.com/maps?q=-0.312456,-78.450123

----------------------------------------
```

## Uso de la Aplicación

1. Abrir la aplicación en tu dispositivo Android
2. Permitir permisos de cámara y ubicación
3. Tomar una foto usando el botón de captura
4. Ver la galería para consultar fotos con sus ubicaciones
5. Click en el enlace de Google Maps para ver la ubicación exacta
6. Eliminar fotos deslizando o usando el botón de eliminar

## Recomendaciones

- Si la ubicación no está disponible, la app asigna valores 0,0 por defecto
- Habilita la ubicación del dispositivo antes de abrir la app
- Prueba en dispositivo físico, ya que el emulador a veces no retorna coordenadas reales
- Verifica permisos en Configuración del sistema si no funciona la geolocalización

## Comandos Útiles

```bash
# Ejecutar en navegador (desarrollo)
ionic serve

# Ejecutar en dispositivo Android
ionic cap run android

# Ver logs en tiempo real
npx cap run android -l --external

# Limpiar y reconstruir
ionic build --prod
npx cap sync android

# Actualizar plugins de Capacitor
npm update @capacitor/camera @capacitor/geolocation
npx cap sync
```

## Solución de Problemas

### La ubicación devuelve 0,0

- Verifica que el GPS esté activado en el dispositivo
- Asegúrate de haber concedido permisos de ubicación
- Prueba en exteriores para mejor señal GPS

### La cámara no abre

- Verifica permisos de cámara en la configuración del dispositivo
- Revisa que el AndroidManifest.xml tenga el permiso CAMERA

### Error al compilar Android

```bash
cd android
./gradlew clean
cd ..
npx cap sync android
```

### Plugins no funcionan

```bash
rm -rf node_modules package-lock.json
npm install
npx cap sync android
```

## Documentación Adicional

- Ionic Framework: https://ionicframework.com/docs
- Capacitor Plugins: https://capacitorjs.com/docs/plugins
- Angular Documentation: https://angular.io/docs
- Android Studio Guide: https://developer.android.com/studio/intro

## Autor

Desarrollado por: Mateo Paredes
Framework: Ionic + Angular + Capacitor
Versión: 1.0.0
Licencia: MIT

## Licencia

Este proyecto está bajo la Licencia MIT.

## Contribuciones

Las contribuciones son bienvenidas. Por favor:

1. Fork el proyecto
2. Crea una rama para tu feature (git checkout -b feature/nueva-funcionalidad)
3. Commit tus cambios (git commit -m 'Agregar nueva funcionalidad')
4. Push a la rama (git push origin feature/nueva-funcionalidad)
5. Abre un Pull Request
