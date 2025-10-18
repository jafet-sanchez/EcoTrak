# EcoTrak App

Sistema de gestión de reciclaje desarrollado con Electron para escritorio. Permite registrar, gestionar y despachar materiales reciclables con un sistema FIFO (First In, First Out) y generación de reportes.

![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Electron](https://img.shields.io/badge/electron-28.0.0-blue.svg)

## Características

### Gestión de Registros
- Registro de materiales reciclables con múltiples tipos (Plega, Cartón, Plásticos, PET, etc.)
- Sistema de pesaje y seguimiento de inventario
- Control de estados (Activo/Despachado)
- Seguimiento por persona responsable
- Historial completo con filtros avanzados

### Sistema de Salidas FIFO
- Despacho automático usando metodología FIFO (First In, First Out)
- Agrupación inteligente por tipo de material
- Despacho parcial de registros
- Control de peso personalizado por grupo
- Validación en tiempo real
- Autorización de personal

### Dashboard y Reportes
- Visualización de estadísticas en tiempo real
- Gráficos por tipo de material
- Métricas de peso disponible en bodega
- Reportes detallados por tipo y persona
- Exportación a Excel

### Persistencia de Datos
- Almacenamiento en archivos Excel (.xlsx)
- Dos hojas: "Registros_Reciclaje" y "Salidas_Despachos"
- Sistema de respaldo automático
- Carga y guardado asíncrono

## Requisitos del Sistema

- **Sistema Operativo:** Windows 10/11, macOS 10.13+, o Linux
- **Node.js:** 16.x o superior
- **npm:** 8.x o superior
- **Memoria RAM:** Mínimo 4 GB
- **Espacio en disco:** 500 MB

## Instalación

### 1. Clonar el repositorio

```bash
git clone https://github.com/usuario/EcoTrack.git
cd EcoTrack
```

### 2. Instalar dependencias

```bash
npm install
```

### 3. Iniciar la aplicación en modo desarrollo

```bash
npm start
```

O para modo desarrollo con DevTools:

```bash
npm run dev
```

## Construcción de Ejecutables

### Para Windows

```bash
npm run build-win
```

El instalador se generará en la carpeta `dist/` con el nombre `EcoTrak app Setup X.X.X.exe`

### Para todas las plataformas

```bash
npm run build
```

### Empaquetado sin instalador

```bash
npm run pack
```

## Estructura del Proyecto

```
EcoTrack/
├── src/
│   ├── main/
│   │   └── main.js              # Proceso principal de Electron
│   ├── renderer/
│   │   ├── index.html           # Interfaz principal
│   │   ├── app.js               # Lógica de la aplicación
│   │   └── notifications.js     # Sistema de notificaciones
│   ├── services/
│   │   ├── excelService.js      # Servicio de manejo de Excel
│   │   └── preload.js           # Script preload de Electron
│   ├── utils/
│   │   └── helpers.js           # Funciones auxiliares
│   └── img/
│       └── Banner-ecotrak.jpg   # Banner de bienvenida
├── assets/
│   └── icons/                   # Iconos de la aplicación
│       ├── icon.icns           # Icono macOS
│       ├── icon2.ico           # Icono Windows
│       └── icon3.ico           # Icono Linux
├── package.json                 # Configuración del proyecto
└── README.md                    # Documentación
```

## Uso de la Aplicación

### Nuevo Registro

1. Navega a la sección **"Nuevo Registro"**
2. Completa los campos requeridos:
   - Peso (kg)
   - Tipo de reciclaje
   - Fecha
   - Persona que registra
   - Observaciones (opcional)
3. Haz clic en **"Registrar Reciclaje"**

### Gestión de Salidas

1. Ve a la sección **"Salidas"**
2. Selecciona los grupos de materiales a despachar
3. Ajusta el peso a despachar si es necesario
4. Completa la información de autorización
5. Confirma el despacho

El sistema despachará automáticamente siguiendo el orden FIFO (registros más antiguos primero).

### Consulta de Historial

1. Accede a **"Historial"**
2. Utiliza los filtros para buscar registros específicos:
   - Por tipo de material
   - Por estado
   - Por rango de fechas
3. Ordena por columnas haciendo clic en los encabezados

### Reportes

1. Navega a **"Reportes"**
2. Visualiza estadísticas por tipo de material
3. Haz clic en cualquier tarjeta para ver detalles completos
4. Exporta reportes a Excel usando el menú

## Tipos de Materiales Soportados

- 📑 Plega
- 📦 Cartón
- 🏭 Centro plástico Alta
- 🧴 Plástico limpio
- 📄 Archivo
- 🛍️ Polipropileno
- 🧽 Estopas
- 🥤 PET

## Atajos de Teclado

| Atajo | Acción |
|-------|--------|
| `Ctrl + N` | Nuevo registro |
| `Ctrl + H` | Ver historial |
| `Ctrl + D` | Ir al dashboard |
| `Ctrl + R` | Ver reportes |
| `Ctrl + F` | Búsqueda global |
| `Ctrl + S` | Guardar base de datos |
| `F12` | Abrir DevTools (modo desarrollo) |
| `Esc` | Cerrar modales |

## Tecnologías Utilizadas

### Frontend
- **Electron:** Framework para aplicaciones de escritorio
- **Tailwind CSS:** Framework CSS para estilos
- **Font Awesome:** Iconos

### Backend
- **Node.js:** Runtime de JavaScript
- **xlsx:** Librería para manejo de archivos Excel
- **fs-extra:** Operaciones extendidas del sistema de archivos

### Build Tools
- **Electron Builder:** Empaquetado y distribución

## Arquitectura

### Proceso Principal (Main Process)
- Gestión de ventanas
- Menú de aplicación
- Manejo de archivos
- Seguridad y permisos

### Proceso Renderer
- Interfaz de usuario
- Lógica de negocio
- Comunicación con Excel
- Validaciones

### Comunicación IPC
- Uso de `contextBridge` para comunicación segura
- API expuesta mediante preload script
- Aislamiento de contextos

## Seguridad

- **Context Isolation:** Habilitado
- **Node Integration:** Deshabilitado en renderer
- **Web Security:** Habilitado
- **Sandbox:** Configurado
- Validación de URLs externas
- Prevención de múltiples instancias

## Sistema FIFO Implementado

El sistema de despacho FIFO garantiza que:

1. Los materiales más antiguos se despachan primero
2. Se respeta el orden cronológico de entrada (ID menor = más antiguo)
3. Se puede despachar parcialmente de un registro
4. Se actualiza el peso restante automáticamente
5. Los registros completamente despachados cambian a estado "Despachado"

### Ejemplo de Flujo FIFO

```
Registros disponibles:
#1: Plega - 50kg (más antiguo)
#2: Plega - 30kg
#3: Plega - 20kg

Despacho solicitado: 65kg de Plega

Resultado:
- Registro #1: 50kg despachados → Estado: Despachado
- Registro #2: 15kg despachados → Peso restante: 15kg
- Registro #3: Sin cambios
Total despachado: 65kg
```

## Formato de Datos Excel

### Hoja "Registros_Reciclaje"

| ID | Tipo | Peso_Inicial | Peso_Restante | Fecha_Registro | Persona | Estado | Observaciones |
|----|------|--------------|---------------|----------------|---------|--------|---------------|

### Hoja "Salidas_Despachos"

| ID_Salida | ID_Registro | Tipo | Peso_Despachado | Fecha_Despacho | Persona_Autoriza | Observaciones |
|-----------|-------------|------|-----------------|----------------|------------------|---------------|

## Solución de Problemas

### La aplicación no inicia

```bash
# Verificar versión de Node.js
node --version

# Reinstalar dependencias
rm -rf node_modules package-lock.json
npm install
```

### Error al guardar en Excel

- Verifica que el archivo Excel no esté abierto en otra aplicación
- Comprueba los permisos de escritura en la carpeta
- Revisa que haya espacio disponible en disco

### Datos no se cargan

- Verifica la ruta del archivo Excel en la configuración
- Confirma que el formato de las hojas sea correcto
- Revisa la consola de DevTools para errores específicos

## Desarrollo

### Scripts Disponibles

```json
{
  "start": "electron .",
  "dev": "electron . --dev",
  "build": "electron-builder",
  "build-win": "electron-builder --win",
  "dist": "electron-builder --publish=never",
  "pack": "electron-builder --dir"
}
```

### Variables de Entorno

```bash
# Modo desarrollo
NODE_ENV=development

# Habilitar logs detallados
DEBUG=electron*
```

### Estructura de Configuración (package.json)

```json
{
  "build": {
    "appId": "com.ecotrak.reciclaje",
    "productName": "EcoTrak app",
    "directories": {
      "output": "dist"
    },
    "files": [
      "src/**/*",
      "assets/**/*",
      "node_modules/**/*",
      "package.json"
    ]
  }
}
```

## Contribuir

1. Fork el proyecto
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add: Amazing Feature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## Changelog

### v1.0.0 (2025-01-18)
- Versión inicial estable
- Sistema FIFO implementado
- Gestión completa de registros y salidas
- Dashboard con reportes en tiempo real
- Exportación a Excel
- Aplicación aceptada por los jefes

## Licencia

Este proyecto está bajo la Licencia MIT. Ver archivo `LICENSE` para más detalles.

## Autor

**Jafet Sánchez Ruiz**

## Contacto

Para reportar bugs o solicitar features, por favor abre un issue en el repositorio de GitHub.

---

Hecho con ❤️ para la gestión sostenible de residuos
