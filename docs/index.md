# 🎯 HistoHunter - Guía Completa del Proyecto

> **Un juego educativo estilo GeoGuessr donde adivinas cuándo y dónde ocurrieron eventos históricos**

![HistoHunter Banner](https://img.shields.io/badge/HistoHunter-Android%20Game-green?style=for-the-badge&logo=android)
![Supabase](https://img.shields.io/badge/Supabase-Database-3ECF8E?style=for-the-badge&logo=supabase)
![OpenStreetMap](https://img.shields.io/badge/OpenStreetMap-API-7EBC6F?style=for-the-badge&logo=openstreetmap)

---

## 📋 Índice

1. [Descripción del Proyecto](#-descripción-del-proyecto)
2. [Requisitos Previos](#-requisitos-previos)
3. [Configuración del Entorno](#-configuración-del-entorno)
4. [Estructura del Proyecto](#-estructura-del-proyecto)
5. [Configuración de Supabase](#-configuración-de-supabase)
6. [Integración de OpenStreetMap](#-integración-de-openstreetmap)
7. [Desarrollo Paso a Paso](#-desarrollo-paso-a-paso)
8. [Sistema de Puntuación](#-sistema-de-puntuación)
9. [Recursos Adicionales](#-recursos-adicionales)

---

## 🎮 Descripción del Proyecto

### ¿Qué es HistoHunter?

HistoHunter es un juego educativo para Android donde los jugadores deben:

1. **Ver un evento histórico** (ej: "Primera llegada del hombre a la Luna")
2. **Marcar en el mapa** dónde creen que ocurrió
3. **Seleccionar la fecha** en que sucedió
4. **Obtener puntos** según la precisión de sus respuestas

### Características Principales

- 🗺️ **Mapa interactivo** con OpenStreetMap
- 📅 **Selector de fechas** para elegir cuándo ocurrió
- 🏆 **Sistema de ranking** global con Supabase
- 📊 **Estadísticas** de precisión por jugador
- 🎯 **Múltiples categorías** de eventos históricos

### Tecnologías Utilizadas

| Tecnología | Uso |
|------------|-----|
| **Android Studio** | IDE de desarrollo |
| **Java/Kotlin** | Lenguaje de programación |
| **Supabase** | Base de datos y autenticación |
| **OpenStreetMap** | API de mapas |
| **OSMDroid** | Librería de mapas para Android |

---

## 📦 Requisitos Previos

### Software Necesario

- [ ] **Android Studio** (versión Arctic Fox o superior)
  - [Descargar Android Studio](https://developer.android.com/studio)
- [ ] **JDK 11** o superior
- [ ] **Git** para control de versiones
- [ ] **Cuenta en GitHub**
- [ ] **Cuenta en Supabase** (gratuita)
  - [Crear cuenta en Supabase](https://supabase.com)

### Conocimientos Recomendados

- Programación básica en Java o Kotlin
- Conceptos básicos de Android
- Fundamentos de APIs REST
- SQL básico

---

## ⚙️ Configuración del Entorno

### Paso 1: Instalar Android Studio

1. Descarga Android Studio desde [developer.android.com/studio](https://developer.android.com/studio)
2. Ejecuta el instalador y sigue las instrucciones
3. Durante la instalación, asegúrate de instalar:
   - Android SDK
   - Android Virtual Device (AVD)
   - Intel HAXM (para emulador)

### Paso 2: Configurar el SDK

1. Abre Android Studio
2. Ve a `File > Settings > Appearance & Behavior > System Settings > Android SDK`
3. Instala:
   - **SDK Platforms**: Android 13 (API 33) o superior
   - **SDK Tools**: Android SDK Build-Tools, Android Emulator

### Paso 3: Crear el Proyecto

1. `File > New > New Project`
2. Selecciona **"Empty Views Activity"**
3. Configura:
   - **Name**: HistoHunter
   - **Package name**: `com.tuescuela.histohunter`
   - **Language**: Java (o Kotlin)
   - **Minimum SDK**: API 24 (Android 7.0)

### Paso 4: Clonar el Repositorio Base

```bash
git clone https://github.com/Pausiar/HistoHunter-Project.git
cd HistoHunter-Project
```

---

## 📁 Estructura del Proyecto

```
📦 HistoHunter/
├── 📂 app/
│   ├── 📂 src/main/
│   │   ├── 📂 java/com/tuescuela/histohunter/
│   │   │   ├── 📂 activities/
│   │   │   │   ├── MainActivity.java
│   │   │   │   ├── GameActivity.java
│   │   │   │   ├── RankingActivity.java
│   │   │   │   └── ResultActivity.java
│   │   │   ├── 📂 models/
│   │   │   │   ├── HistoricalEvent.java
│   │   │   │   ├── Player.java
│   │   │   │   └── Score.java
│   │   │   ├── 📂 api/
│   │   │   │   ├── SupabaseClient.java
│   │   │   │   └── EventsRepository.java
│   │   │   ├── 📂 utils/
│   │   │   │   ├── ScoreCalculator.java
│   │   │   │   ├── DateUtils.java
│   │   │   │   └── DistanceCalculator.java
│   │   │   └── 📂 adapters/
│   │   │       └── RankingAdapter.java
│   │   ├── 📂 res/
│   │   │   ├── 📂 layout/
│   │   │   ├── 📂 values/
│   │   │   └── 📂 drawable/
│   │   └── AndroidManifest.xml
│   └── build.gradle
├── 📂 docs/
│   └── index.md
└── build.gradle
```

---

## 🗄️ Configuración de Supabase

### Paso 1: Crear Proyecto en Supabase

1. Ve a [supabase.com](https://supabase.com) y crea una cuenta
2. Click en **"New Project"**
3. Configura:
   - **Name**: HistoHunter
   - **Database Password**: (guárdala en lugar seguro)
   - **Region**: Elige la más cercana a tu ubicación

### Paso 2: Crear las Tablas

Ve a **SQL Editor** en Supabase y ejecuta:

```sql
-- Tabla de eventos históricos
CREATE TABLE historical_events (
  id BIGSERIAL PRIMARY KEY,
  name VARCHAR(300) NOT NULL,
  description TEXT,
  event_date DATE NOT NULL,
  latitude DECIMAL(10, 8) NOT NULL,
  longitude DECIMAL(11, 8) NOT NULL,
  location_name VARCHAR(200),
  category VARCHAR(50),
  difficulty VARCHAR(20) DEFAULT 'medium',
  image_url TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Tabla de jugadores
CREATE TABLE players (
  id BIGSERIAL PRIMARY KEY,
  username VARCHAR(50) UNIQUE NOT NULL,
  email VARCHAR(100),
  total_score INTEGER DEFAULT 0,
  games_played INTEGER DEFAULT 0,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Tabla de puntuaciones
CREATE TABLE scores (
  id BIGSERIAL PRIMARY KEY,
  player_id BIGINT REFERENCES players(id),
  event_id BIGINT REFERENCES historical_events(id),
  distance_km DECIMAL(10, 2),
  days_difference INTEGER,
  location_score INTEGER,
  date_score INTEGER,
  total_score INTEGER,
  played_at TIMESTAMP DEFAULT NOW()
);

-- Vista del ranking
CREATE VIEW leaderboard AS
SELECT 
  p.username,
  p.total_score,
  p.games_played,
  ROUND(p.total_score::DECIMAL / NULLIF(p.games_played, 0), 2) as avg_score
FROM players p
ORDER BY p.total_score DESC;

-- Índices para mejorar rendimiento
CREATE INDEX idx_scores_player ON scores(player_id);
CREATE INDEX idx_scores_total ON scores(total_score DESC);
```

### Paso 3: Insertar Eventos de Ejemplo

```sql
INSERT INTO historical_events (name, description, event_date, latitude, longitude, location_name, category, difficulty) VALUES
('Llegada del hombre a la Luna', 'El Apollo 11 aterriza en la Luna con Neil Armstrong y Buzz Aldrin', '1969-07-20', 28.5721, -80.6480, 'Centro Espacial Kennedy, Florida', 'ciencia', 'medium'),
('Caída del Muro de Berlín', 'El muro que dividía Berlín Este y Oeste es derribado', '1989-11-09', 52.5163, 13.3777, 'Berlín, Alemania', 'politica', 'easy'),
('Descubrimiento de América', 'Cristóbal Colón llega a América', '1492-10-12', 24.0667, -74.5167, 'San Salvador, Bahamas', 'exploracion', 'easy'),
('Revolución Francesa - Toma de la Bastilla', 'El pueblo de París asalta la prisión de la Bastilla', '1789-07-14', 48.8532, 2.3692, 'París, Francia', 'politica', 'medium'),
('Primera Guerra Mundial - Inicio', 'Asesinato del Archiduque Francisco Fernando', '1914-06-28', 43.8563, 18.4131, 'Sarajevo, Bosnia', 'guerra', 'hard'),
('Bomba atómica en Hiroshima', 'Estados Unidos lanza la primera bomba atómica', '1945-08-06', 34.3853, 132.4553, 'Hiroshima, Japón', 'guerra', 'medium'),
('Hundimiento del Titanic', 'El RMS Titanic se hunde en el Atlántico Norte', '1912-04-15', 41.7258, -49.9469, 'Océano Atlántico Norte', 'desastres', 'hard'),
('Coronación de Isabel II', 'Isabel II es coronada Reina del Reino Unido', '1953-06-02', 51.4994, -0.1248, 'Abadía de Westminster, Londres', 'monarquia', 'medium'),
('Terremoto de San Francisco', 'Devastador terremoto destruye gran parte de la ciudad', '1906-04-18', 37.7749, -122.4194, 'San Francisco, California', 'desastres', 'hard'),
('Firma de la Declaración de Independencia de EEUU', 'Se firma la declaración en Filadelfia', '1776-07-04', 39.9489, -75.1500, 'Filadelfia, Pensilvania', 'politica', 'easy');
```

### Paso 4: Obtener Credenciales API

1. Ve a **Settings > API** en tu proyecto de Supabase
2. Copia:
   - **Project URL**: `https://xxxxx.supabase.co`
   - **anon/public key**: `eyJhbG...`

### Paso 5: Configurar en Android

Crea el archivo `app/src/main/res/values/secrets.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="supabase_url">https://TU_PROJECT_ID.supabase.co</string>
    <string name="supabase_key">TU_ANON_KEY</string>
</resources>
```

> ⚠️ **IMPORTANTE**: Añade `secrets.xml` a tu `.gitignore` para no subir las credenciales

---

## 🗺️ Integración de OpenStreetMap

### Paso 1: Añadir Dependencias

En `app/build.gradle`:

```gradle
dependencies {
    // OSMDroid - Mapas OpenStreetMap para Android
    implementation 'org.osmdroid:osmdroid-android:6.1.17'
    
    // Cliente HTTP para APIs
    implementation 'com.squareup.okhttp3:okhttp:4.11.0'
    
    // Gson para parsear JSON
    implementation 'com.google.code.gson:gson:2.10.1'
    
    // Supabase (opcional - también puedes usar REST directamente)
    implementation 'io.github.jan-tennert.supabase:postgrest-kt:2.0.0'
    implementation 'io.ktor:ktor-client-android:2.3.5'
}
```

### Paso 2: Permisos en AndroidManifest.xml

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
    
    <!-- Permisos necesarios -->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" 
                     android:maxSdkVersion="28"/>
    
    <application
        android:usesCleartextTraffic="true"
        ...>
        <!-- Activities -->
    </application>
</manifest>
```

### Paso 3: Configurar el Mapa

```java
// En tu Activity
import org.osmdroid.config.Configuration;
import org.osmdroid.tileprovider.tilesource.TileSourceFactory;
import org.osmdroid.util.GeoPoint;
import org.osmdroid.views.MapView;
import org.osmdroid.views.overlay.Marker;

public class GameActivity extends AppCompatActivity {
    private MapView mapView;
    private GeoPoint selectedPoint;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        
        // Configuración de OSMDroid
        Configuration.getInstance().setUserAgentValue(getPackageName());
        
        setContentView(R.layout.activity_game);
        
        // Inicializar mapa
        mapView = findViewById(R.id.mapView);
        mapView.setTileSource(TileSourceFactory.MAPNIK);
        mapView.setMultiTouchControls(true);
        
        // Configurar zoom y posición inicial
        mapView.getController().setZoom(3.0);
        mapView.getController().setCenter(new GeoPoint(20.0, 0.0));
        
        // Detectar clicks en el mapa
        mapView.getOverlays().add(new MapEventsOverlay(new MapEventsReceiver() {
            @Override
            public boolean singleTapConfirmedHelper(GeoPoint p) {
                setMarker(p);
                return true;
            }
            
            @Override
            public boolean longPressHelper(GeoPoint p) {
                return false;
            }
        }));
    }
    
    private void setMarker(GeoPoint point) {
        // Limpiar marcadores anteriores
        mapView.getOverlays().clear();
        
        // Crear nuevo marcador
        Marker marker = new Marker(mapView);
        marker.setPosition(point);
        marker.setTitle("Tu selección");
        mapView.getOverlays().add(marker);
        
        // Guardar punto seleccionado
        selectedPoint = point;
        
        mapView.invalidate();
    }
}
```

### Paso 4: Layout del Mapa

`res/layout/activity_game.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    
    <!-- Pregunta del evento -->
    <TextView
        android:id="@+id/tvEventQuestion"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:padding="16dp"
        android:textSize="18sp"
        android:textStyle="bold"
        android:text="¿Dónde y cuándo ocurrió este evento?" />
    
    <TextView
        android:id="@+id/tvEventName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:paddingHorizontal="16dp"
        android:textSize="16sp"
        android:textColor="@color/primary" />
    
    <!-- Mapa -->
    <org.osmdroid.views.MapView
        android:id="@+id/mapView"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1" />
    
    <!-- Selector de fecha -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:padding="16dp">
        
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Fecha: " />
        
        <Button
            android:id="@+id/btnSelectDate"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Seleccionar fecha" />
        
        <TextView
            android:id="@+id/tvSelectedDate"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginStart="8dp" />
    </LinearLayout>
    
    <!-- Botón de confirmar -->
    <Button
        android:id="@+id/btnConfirm"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"
        android:text="CONFIRMAR RESPUESTA"
        android:backgroundTint="@color/primary" />
        
</LinearLayout>
```

---

## 🧮 Sistema de Puntuación

### Fórmula de Cálculo

```java
public class ScoreCalculator {
    
    private static final int MAX_LOCATION_SCORE = 1000;
    private static final int MAX_DATE_SCORE = 500;
    private static final int PERFECT_BONUS = 500;
    
    /**
     * Calcula la puntuación basada en la distancia
     * Máximo: 1000 puntos (si distancia = 0)
     * Mínimo: 0 puntos (si distancia >= 5000 km)
     */
    public static int calculateLocationScore(double distanceKm) {
        if (distanceKm <= 0) return MAX_LOCATION_SCORE;
        if (distanceKm >= 5000) return 0;
        
        // Fórmula: puntos = max - (distancia * factor)
        // Factor = 1000/5000 = 0.2
        return (int) Math.max(0, MAX_LOCATION_SCORE - (distanceKm * 0.2));
    }
    
    /**
     * Calcula la puntuación basada en la diferencia de días
     * Máximo: 500 puntos (si días = 0)
     * Mínimo: 0 puntos (si diferencia >= 365 días)
     */
    public static int calculateDateScore(int daysDifference) {
        if (daysDifference <= 0) return MAX_DATE_SCORE;
        if (daysDifference >= 365) return 0;
        
        // Fórmula: puntos = max - (días * factor)
        // Factor = 500/365 ≈ 1.37
        return (int) Math.max(0, MAX_DATE_SCORE - (daysDifference * 1.37));
    }
    
    /**
     * Calcula la puntuación total con bonus por precisión perfecta
     */
    public static int calculateTotalScore(double distanceKm, int daysDifference) {
        int locationScore = calculateLocationScore(distanceKm);
        int dateScore = calculateDateScore(daysDifference);
        int total = locationScore + dateScore;
        
        // Bonus por precisión perfecta (< 50km y < 30 días)
        if (distanceKm < 50 && daysDifference < 30) {
            total += PERFECT_BONUS;
        }
        
        return total;
    }
    
    /**
     * Calcula la distancia entre dos puntos geográficos (Haversine)
     */
    public static double calculateDistance(double lat1, double lon1, 
                                           double lat2, double lon2) {
        final int R = 6371; // Radio de la Tierra en km
        
        double latDistance = Math.toRadians(lat2 - lat1);
        double lonDistance = Math.toRadians(lon2 - lon1);
        
        double a = Math.sin(latDistance / 2) * Math.sin(latDistance / 2)
                + Math.cos(Math.toRadians(lat1)) * Math.cos(Math.toRadians(lat2))
                * Math.sin(lonDistance / 2) * Math.sin(lonDistance / 2);
        
        double c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
        
        return R * c;
    }
}
```

### Tabla de Puntuación

| Distancia | Puntos Ubicación | Diferencia Días | Puntos Fecha |
|-----------|------------------|-----------------|--------------|
| 0 km | 1000 | 0 días | 500 |
| 100 km | 980 | 7 días | 490 |
| 500 km | 900 | 30 días | 459 |
| 1000 km | 800 | 90 días | 377 |
| 2500 km | 500 | 180 días | 253 |
| 5000+ km | 0 | 365+ días | 0 |

**Bonus de Precisión**: +500 puntos si distancia < 50km Y diferencia < 30 días

---

## 👨‍💻 Desarrollo Paso a Paso

### Fase 1: Configuración Base (Semana 1)

- [ ] Crear proyecto en Android Studio
- [ ] Configurar Supabase
- [ ] Añadir dependencias
- [ ] Crear estructura de carpetas

### Fase 2: Modelos y Base de Datos (Semana 2)

- [ ] Crear clases modelo (Event, Player, Score)
- [ ] Implementar cliente Supabase
- [ ] Probar conexión con la base de datos
- [ ] Crear repositorio de eventos

### Fase 3: Interfaz de Usuario (Semana 3)

- [ ] Diseñar MainActivity (pantalla inicio)
- [ ] Implementar GameActivity con mapa
- [ ] Crear selector de fecha
- [ ] Diseñar ResultActivity

### Fase 4: Lógica del Juego (Semana 4)

- [ ] Implementar cálculo de distancia
- [ ] Implementar cálculo de puntuación
- [ ] Conectar UI con lógica
- [ ] Guardar puntuaciones en Supabase

### Fase 5: Ranking y Pulido (Semana 5)

- [ ] Crear pantalla de ranking
- [ ] Implementar leaderboard
- [ ] Añadir animaciones
- [ ] Testing y corrección de bugs

---

## 📚 Recursos Adicionales

### Documentación Oficial

- [Android Developers](https://developer.android.com/docs)
- [Supabase Docs](https://supabase.com/docs)
- [OSMDroid Wiki](https://github.com/osmdroid/osmdroid/wiki)
- [OpenStreetMap API](https://wiki.openstreetmap.org/wiki/API)

### Tutoriales Recomendados

- [Curso Android en español - YouTube](https://www.youtube.com/results?search_query=curso+android+studio+español)
- [Supabase con Android](https://supabase.com/docs/guides/getting-started/tutorials/with-kotlin)

### APIs Adicionales Gratuitas

| API | Uso | Documentación |
|-----|-----|---------------|
| **Nominatim** | Geocodificación inversa | [nominatim.org](https://nominatim.org/release-docs/develop/api/Overview/) |
| **Calendarific** | Fechas históricas | [calendarific.com](https://calendarific.com/api-documentation) |
| **Wikipedia API** | Info adicional de eventos | [mediawiki.org](https://www.mediawiki.org/wiki/API:Main_page) |

---

## 🤝 Contribuir al Proyecto

1. Haz fork del repositorio
2. Crea una rama para tu feature (`git checkout -b feature/nueva-funcionalidad`)
3. Commit de tus cambios (`git commit -m 'Añadir nueva funcionalidad'`)
4. Push a la rama (`git push origin feature/nueva-funcionalidad`)
5. Abre un Pull Request

---

## 📝 Licencia

Este proyecto es de uso educativo. Siéntete libre de usarlo y modificarlo para tu clase.

---
