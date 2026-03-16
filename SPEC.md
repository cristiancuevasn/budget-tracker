# Budget Tracker App - SPEC.md

## Project Overview
- **Name:** Budget Tracker (Presupuesto Personal)
- **Type:** Web App (PWA)
- **Core:** Gestión completa de finanzas personales con cloud sync
- **Target:** Individuos que quieren control total de sus finanzas

---

## UI/UX Specification

### Color Palette
- **Primary:** #6366f1 (Indigo)
- **Primary Dark:** #4f46e5
- **Secondary:** #10b981 (Emerald - ingresos)
- **Danger:** #ef4444 (Gastos)
- **Warning:** #f59e0b (Alertas)
- **Background:** #f8fafc
- **Card:** #ffffff
- **Text:** #1e293b
- **Text Light:** #64748b

### Typography
- **Primary Font:** -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto
- **Headings:** 700 weight
- **Body:** 400 weight, 16px

### Responsive
- Mobile-first (iPhone priority)
- Desktop: 2-column layout
- Breakpoints: 640px (mobile), 1024px (tablet), 1280px (desktop)

### Layout Structure
```
┌─────────────────────────────────────┐
│ Header (Logo + User + Logout)       │
├─────────────────────────────────────┤
│ Overview Cards (Ingresos/Gastos/   │
│ Balance/Presupuesto剩余)            │
├─────────────────────────────────────┤
│ ┌─────────────┬─────────────────┐   │
│ │ Sidebar     │ Main Content    │   │
│ │ - Dashboard │ - Gráficos      │   │
│ │ - Transac. │ - Transacciones │   │
│ │ - Categories│ - Categorías    │   │
│ │ - Reports   │ - Reportes      │   │
│ │ - Settings  │ - Configuración │   │
│ └─────────────┴─────────────────┘   │
└─────────────────────────────────────┘
```

---

## Functionality Specification

### 1. Authentication (Firebase Auth)
- Login con Google
- Login con Email/Password
- Logout
- Sesión persistente
- Protected routes

### 2. Dashboard
- **Overview Cards:**
  - Ingresos del mes
  - Gastos del mes
  - Balance (ingresos - gastos)
  - Presupuesto total vs usado
- **Gráficos:**
  - Gráfico circular: Gastos por categoría
  - Gráfico de barras: Ingresos vs Gastos (últimos 6 meses)
  - Tendencia de gasto diario (últimos 7 días)
- **Alertas:**
  - Notificación cuando gasto > 80% del presupuesto
  - Notificación cuando gasto > 100% del presupuesto

### 3. Transacciones
- **Agregar transacción:**
  - Tipo: Ingreso / Gasto
  - Cantidad (número, 2 decimales)
  - Descripción (texto)
  - Categoría (dropdown)
  - Fecha (date picker, default: hoy)
  - Método de pago (efectivo, tarjeta, transferencia)
- **Lista de transacciones:**
  - Orden cronológico (más reciente primero)
  - Filtros: fecha, tipo, categoría
  - Búsqueda por descripción
  - Editar / Eliminar transacción
- **Estados vacío:** "No hay transacciones"

### 4. Categorías
- **Categorías por defecto:**
  - Ingresos: Salario, Freelance, Inversión, Regalo, Otro
  - Gastos: Comida, Transporte, Vivienda, Entretenimiento, Servicios, Salud, Shopping, Otro
- **Gestión de categorías:**
  - Crear nueva categoría
  - Editar nombre
  - Asignar icono y color
  - Eliminar (solo si no tiene transacciones)
- **Presupuesto por categoría:**
  - Establecer límite mensual por categoría
  - Ver progreso (barrita de progreso)
  - Alerta cuando > 80%

### 5. Reportes
- **Resumen mensual:**
  - Total ingresos
  - Total gastos
  - Diferencia
  - Top 3 categorías de gasto
- **Rango de fechas:**
  - Este mes
  - Mes pasado
  - Últimos 3 meses
  - Este año
  - Personalizado
- **Exportar:**
  - ✅ Excel (.xlsx) - usando SheetJS
  - ✅ CSV
  - Contenido: Transacciones con columnas: Fecha, Tipo, Categoría, Descripción, Cantidad, Método

### 6. Configuración
- **Perfil:**
  - Nombre
  - Email
  - Foto de perfil (de Google)
- **Preferencias:**
  - Moneda (USD, EUR, PR$ - por defecto USD)
  - Tema (Light/Dark/System)
- **Notificaciones:**
  - Activar/desactivar alertas de presupuesto
  - Configurar umbral de alerta (default: 80%)

### 7. Cloud Sync
- **Firebase Firestore:**
  - Colección: users/{uid}/transactions
  - Colección: users/{uid}/categories
  - Colección: users/{uid}/budgets
  - Colección: users/{uid}/settings
- **Sincronización:**
  - Escucha en tiempo real (onSnapshot)
  - Offline persistence enabled
  - Conflicto: último escribe gana

### 8. PWA Features
- ✅ Installable (manifest.json)
- ✅ Offline mode (cache básico)
- ✅ Mobile-responsive
- ✅ Add to Home Screen

---

## Data Models

### Transaction
```typescript
{
  id: string;
  type: 'income' | 'expense';
  amount: number;
  description: string;
  category: string;
  date: Timestamp;
  paymentMethod: 'cash' | 'card' | 'transfer';
  createdAt: Timestamp;
  updatedAt: Timestamp;
}
```

### Category
```typescript
{
  id: string;
  name: string;
  icon: string;
  color: string;
  type: 'income' | 'expense';
  budget?: number; // mensual
}
```

### Settings
```typescript
{
  currency: 'USD' | 'EUR' | 'PR';
  theme: 'light' | 'dark' | 'system';
  alertsEnabled: boolean;
  alertThreshold: number; // 80 default
}
```

---

## Tech Stack

- **Frontend:** Vanilla HTML/CSS/JS (lightweight)
- **Backend:** Firebase (Auth + Firestore)
- **Export:** SheetJS (xlsx) + CSV native
- **Icons:** Emoji (no external dependencies)
- **Charts:** Chart.js (CDN)

---

## Acceptance Criteria

1. ✅ Usuario puede hacer login con Google
2. ✅ Usuario puede agregar/edit/delete transacciones
3. ✅Dashboard muestra gráficos en tiempo real
4. ✅ Alertas appearing cuando gasto > 80% presupuesto
5. ✅ Usuario puede exportar a Excel y CSV
6. ✅ Datos se sincronizan en la nube (Firebase)
7. ✅ App funciona offline (cache básico)
8. ✅ Se puede instalar en iPhone (PWA)
9. ✅ Responsive en móvil y desktop
10. ✅ Categorías tienen presupuestos individuales

---

## Files Structure
```
budget-app/
├── index.html        # Main app
├── manifest.json     # PWA manifest
├── app.js           # Lógica principal
├── styles.css       # Estilos
├── firebase.js      # Config Firebase
├── export.js        # Export functions
├── icon.svg         # App icon
└── .gsd/
    └── CONTEXT.md   # GSD contexto
```