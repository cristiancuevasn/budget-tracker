# Context: Budget Tracker App

## Decisions (Locked)
- **Plataforma:** Web app (PWA - funciona en iPhone y navegador)
- **Almacenamiento:** Cloud sync (necesita authentication)
- **Export:** Excel (.xlsx) y CSV
- **Notificaciones:** Alertas cuando se acerca al límite de presupuesto

## Funcionalidad Completa
1. Registros de ingresos y gastos
2. Categorías personalizables (comida, transporte, entretenimiento, etc.)
3. Establecer presupuesto mensual por categoría
4. Dashboard con gráficos y análisis
5. Alertas cuando gastas >80% del presupuesto
6. Historial de transacciones
7. Resumen mensual/anual
8. Exportar reportes a Excel y CSV

## Cloud Storage
- **Recomendado:** Firebase (Google)
- **Cuenta:** zyronotb@gmail.com
- **Servicios a usar:**
  - Firebase Auth (autenticación Google/email)
  - Firestore (base de datos en la nube)
  - Firebase Hosting (opcional para hosting)

## Pendiente para Plan
- [ ] Investigar opción de cloud storage (Firebase/Supabase/localStorage)
- [ ] Diseñar estructura de base de datos
- [ ] Crear SPEC.md con detalles técnicos
- [ ] Definir UI/UX

## Out of Scope (por ahora)
- Autenticación multi-usuario
- Sincronización en tiempo real
- App nativa iOS/Android
- Modo offline completo