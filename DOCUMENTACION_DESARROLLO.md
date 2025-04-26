# Documentación del Proceso de Desarrollo - IEMS-CONTABLE

## Introducción

Este documento detalla el proceso de desarrollo de la aplicación contable educativa, incluyendo las decisiones técnicas, arquitectura y evolución del proyecto desde su concepción hasta su estado actual.

La **IEMS-CONTABLE** es una aplicación web diseñada para enseñar conceptos contables y financieros de manera interactiva, utilizando técnicas de gamificación para mejorar la experiencia de aprendizaje. La aplicación incluye ejercicios prácticos, juegos educativos y contenido teórico sobre contabilidad básica, finanzas personales y aspectos fiscales.

## Arquitectura General

### Tecnologías Principales
- **Frontend**: React + TypeScript + Vite
- **Estilizado**: Tailwind CSS, Shadcn UI
- **Enrutamiento**: React Router
- **Autenticación**: Supabase Auth
- **Base de Datos**: Supabase PostgreSQL
- **Componentes UI**: Radix UI, Shadcn UI
- **Funcionalidad Drag & Drop**: @dnd-kit (migrado desde react-beautiful-dnd)
- **Animaciones**: framer-motion
- **Efectos Visuales**: canvas-confetti

### Estructura de la Base de Datos
```sql
-- Esquema para almacenar progreso de usuarios
CREATE TABLE user_progress (
  id UUID PRIMARY KEY REFERENCES auth.users(id),
  total_points INTEGER DEFAULT 0,
  completed_modules INTEGER[] DEFAULT '{}',
  completed_lessons INTEGER[] DEFAULT '{}',
  last_active TIMESTAMP WITH TIME ZONE DEFAULT now()
);

-- Esquema para almacenar logros desbloqueados
CREATE TABLE user_achievements (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES auth.users(id),
  achievement_id INTEGER NOT NULL,
  unlocked_at TIMESTAMP WITH TIME ZONE DEFAULT now(),
  UNIQUE(user_id, achievement_id)
);

-- Esquema para almacenar resultados de ejercicios
CREATE TABLE exercise_results (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES auth.users(id),
  exercise_id INTEGER NOT NULL,
  score INTEGER NOT NULL,
  completed_at TIMESTAMP WITH TIME ZONE DEFAULT now(),
  attempts INTEGER DEFAULT 1
);
```

## Cronología de Desarrollo

### 16 de Abril 2025 - Inicialización del Proyecto

**Commits realizados:** 2 (1 hora)

#### Decisiones técnicas:
- Se estableció la estructura base del proyecto utilizando React y TypeScript
- Se creó el README.md inicial con la descripción y objetivos del proyecto
- Se configuraron los archivos básicos de TypeScript para garantizar un desarrollo con tipado estricto
- Se definieron los requerimientos iniciales del proyecto
- Se creó el archivo de tareas (TAREAS_IEMS-CONTABLE.md) para organizar el desarrollo

#### Justificación:
Se eligió React por su robustez en la creación de interfaces de usuario interactivas, mientras que TypeScript se seleccionó para proporcionar tipado estático, mejorando así la calidad del código y facilitando el mantenimiento futuro. La combinación de Vite como bundler ofrece un entorno de desarrollo más rápido comparado con Create React App. Esta combinación permite desarrollar una aplicación contable con componentes reutilizables y un código menos propenso a errores.

### 19 de Abril 2025 - Mejoras de Estructura y Autenticación

**Commits realizados:** 7 (2 horas)

#### Decisiones técnicas:
1. **Actualización de dependencias y autenticación:**
   - Se implementó inicio de sesión con Google mediante Supabase Auth
   - Se mejoró la estructura de rutas con React Router
   - Se eliminaron archivos innecesarios
   - Se añadieron nuevos elementos UI con Shadcn UI y Tailwind CSS
   - Se crearon páginas de inicio de sesión, registro y recuperación de contraseña
   - Se diseñó el dashboard principal

2. **Gestión de tareas:**
   - Se agregó sección "Siempre" en Task-List.mdc
   - Se estableció verificación del archivo TAREAS_IEMS-CONTABLE.md

3. **Actualización de bibliotecas:**
   - Se añadieron dependencias como @formkit/drag-and-drop, class-variance-authority, clsx, tailwind-merge y uuid
   - Se optimizó el juego de clasificación de cuentas
   - Se mejoró la lógica de arrastre y soltado
   - Se añadieron nuevas lecciones al módulo de contabilidad básica
   - Se implementó visualizador de esquemas de mayor (38 esquemas)

4. **Refactorización de juegos:**
   - Se optimizaron los componentes de clasificación (BalanceSheetGame, AccountClassificationGame, IncomeStatementGame, CostClassificationGame)
   - Se implementó useCallback para mejorar el rendimiento
   - Se eliminaron efectos innecesarios
   - Se desarrolló el juego de arrastrar cuentas a categorías

5. **Sistema de logros:**
   - Se actualizaron las interfaces Achievement y UserAchievement
   - Se mejoraron las funciones para obtener y desbloquear logros
   - Se optimizó la lógica de verificación de logros
   - Se mejoró el manejo de errores en consultas a Supabase

6. **Mejoras en BalanceSheetGame:**
   - Se implementó un manejo más eficiente del estado
   - Se mejoró la clasificación de elementos
   - Se añadió visualización de estadísticas
   - Se optimizó el sistema de arrastre y soltado
   - Se utilizaron componentes de Shadcn UI y Tailwind CSS
   - Se implementó la sección de conceptos básicos del balance general

#### Justificación:
- La autenticación con Google a través de Supabase se implementó por su facilidad de uso, seguridad y por no requerir backend propio, mejorando la experiencia de usuario y reduciendo el tiempo de desarrollo.
- La refactorización de los componentes de juego utilizando useCallback fue crucial para evitar renderizados innecesarios, optimizando así el rendimiento en componentes con interacciones frecuentes.
- La elección de Shadcn UI y Tailwind CSS permitió crear interfaces coherentes y responsivas con un desarrollo más rápido, evitando escribir CSS personalizado y manteniendo consistencia visual.
- Supabase se seleccionó como backend por su facilidad de integración y potentes capacidades de base de datos y autenticación, permitiendo un desarrollo más ágil sin necesidad de implementar un backend completo.

### 20 de Abril 2025 - Optimización de Componentes

**Commits realizados:** 2 (1 hora)

#### Decisiones técnicas:
- Se refactorizó BalanceSheetGame para exportarlo como componente default
- Se eliminaron importaciones innecesarias
- Se optimizó la estructura del código
- Se mejoró la interfaz del juego con mejor feedback visual

#### Justificación:
La exportación por defecto simplifica las importaciones en otros archivos, mejorando la legibilidad del código. La eliminación de importaciones innecesarias reduce el tamaño del bundle y mejora el tiempo de carga de la aplicación. La reorganización del código facilita su mantenimiento y extensibilidad.

### 21 de Abril 2025 - Integración de Arrastre y Animaciones

**Commits realizados:** 4 (2 horas)

#### Decisiones técnicas:
1. **Nuevas dependencias:**
   - Se añadieron canvas-confetti para efectos visuales al completar ejercicios
   - Se integró framer-motion para animaciones fluidas en componentes
   - Se implementó funcionalidad de arrastre y soltado con react-beautiful-dnd
   - Se añadieron efectos visuales y animaciones para mejorar la experiencia
   - Se desarrolló un sistema de puntuación basado en la dificultad y tipo de actividad

2. **Migración a @dnd-kit:**
   - Se reemplazó react-beautiful-dnd por @dnd-kit/core, @dnd-kit/modifiers y @dnd-kit/sortable
   - Se refactorizó BalanceSheetGame para utilizar la nueva biblioteca
   - Se implementó AchievementContext para gestión de logros
   - Se creó componente AchievementNotification para mostrar logros desbloqueados

3. **Mejoras en componentes:**
   - Se añadió @radix-ui/react-progress para barras de progreso
   - Se optimizó la gestión del estado y la clasificación de elementos
   - Se mejoró la lógica de puntuación y visualización de estadísticas
   - Se creó interfaz para mostrar puntuación y logros obtenidos

4. **Corrección de errores:**
   - Se refactorizó useBalanceSheetGame para guardar correctamente puntuaciones
   - Se eliminaron variables innecesarias en useUserProgress
   - Se actualizaron importaciones para utilizar el cliente de Supabase correctamente
   - Se mejoró la gestión de errores en el componente de inicio de sesión

#### Justificación:
- Se migró de react-beautiful-dnd a @dnd-kit por su mejor rendimiento, accesibilidad y soporte para React 18, además de proporcionar una API más modular y extensible.
- Las animaciones con framer-motion y los efectos con canvas-confetti se implementaron para mejorar la experiencia de usuario y hacer la aplicación más atractiva, reforzando la sensación de logro al completar ejercicios.
- La refactorización del sistema de puntuación asegura que los datos se guarden correctamente en Supabase, manteniendo la integridad de la información del usuario y permitiendo implementar funciones como la tabla de clasificación.

### 22 de Abril 2025 - Ampliación de Funcionalidades y Soporte Móvil

**Commits realizados:** 4 (1 hora)

#### Decisiones técnicas:
1. **Actualización de documentación:**
   - Se actualizó TAREAS_IEMS-CONTABLE.md
   - Se reflejó la finalización de tareas relacionadas con Supabase
   - Se incluyó el nuevo componente IncomeStatement
   - Se completó la sección de conceptos básicos del estado de resultados

2. **Configuración de API externa:**
   - Se actualizó el archivo .env con nueva clave de Hugging Face
   - Se amplió la documentación para incluir nuevas tareas
   - Se planificó la integración de IA para asistencia en aprendizaje

3. **Implementación de soporte táctil:**
   - Se añadieron funciones para manejar arrastres táctiles en componentes de juego (BalanceSheetGame, AccountClassificationGame, IncomeStatementGame, CostClassificationGame)
   - Se optimizó DragDropContainer para mejorar la experiencia en dispositivos móviles
   - Se implementó vibración táctil como confirmación de acciones

4. **Refactorización de hooks:**
   - Se optimizó useUserProgress
   - Se añadieron funciones para registrar finalización de juegos, lecciones y módulos
   - Se mejoraron las operaciones con la API
   - Se corrigieron errores en la obtención de datos de Supabase
   - Se implementó el hook useBalanceSheetGame con soporte para niveles de dificultad

#### Justificación:
- El soporte para interacciones táctiles era crucial para garantizar una buena experiencia en dispositivos móviles, considerando la creciente tendencia de acceso desde smartphones y tablets.
- La refactorización de hooks, especialmente useUserProgress, mejoró la separación de responsabilidades y facilitó el mantenimiento del código, siguiendo principios SOLID.
- La integración con Hugging Face se implementó para potenciar funcionalidades de IA en la aplicación, permitiendo una experiencia de aprendizaje más personalizada.

### 23 de Abril 2025 - Mejoras de Responsividad

**Commits realizados:** 1

#### Decisiones técnicas:
- Se actualizó TAREAS_IEMS-CONTABLE.md
- Se optimizaron componentes clave para dispositivos móviles:
  * **Navigation.tsx**: Implementado menú desplegable para móviles con atributos de accesibilidad
  * **ProfileDashboard.tsx**: Optimización de tarjetas y reducción de tamaño de avatares
  * **BalanceSheetGame.tsx**: Mejorada interacción táctil con feedback visual
  * **CostClassificationGame.tsx**: Adaptado layout y reducción de información en vista móvil
  * **Leaderboard.tsx & LeaderboardItem.tsx**: Reestructuración de botones de filtro
- Se corrigieron errores de tipado en BalanceSheetGame.tsx
- Se implementaron mejoras adicionales de responsividad en juegos

#### Justificación:
La responsividad es esencial para garantizar que la aplicación sea accesible desde cualquier dispositivo. Las mejoras implementadas aseguran una experiencia fluida tanto en desktop como en dispositivos móviles, aumentando así el alcance potencial de la aplicación y mejorando la retención de usuarios.

## Decisiones Técnicas Generales

### Elección de Tecnologías

1. **React y TypeScript:**
   - Proporcionan una base sólida para el desarrollo de interfaces interactivas
   - El tipado estático mejora la calidad del código y facilita el mantenimiento
   - Vite como bundler ofrece un entorno de desarrollo más rápido

2. **Supabase:**
   - Ofrece una solución completa para backend, autenticación y base de datos
   - Facilita la implementación de características como autenticación con Google y almacenamiento de progreso
   - Reduce significativamente el tiempo de desarrollo al no requerir backend personalizado

3. **Bibliotecas de UI:**
   - Shadcn UI y Tailwind CSS permiten un desarrollo rápido de interfaces coherentes y responsivas
   - Reducen la necesidad de CSS personalizado y garantizan consistencia visual
   - Radix UI proporciona componentes accesibles y personalizables

4. **Bibliotecas de arrastre y soltado:**
   - @dnd-kit proporciona una solución moderna y eficiente para interacciones de arrastre
   - Mejora la accesibilidad y soporte para dispositivos táctiles
   - API más modular que alternativas como react-beautiful-dnd

5. **Gamificación:**
   - Implementación de sistema de puntos, logros y notificaciones para mejorar la experiencia
   - Uso de canvas-confetti y framer-motion para feedback visual y celebraciones
   - Diseño de insignias y logros visuales para motivar el aprendizaje

### Arquitectura de la Aplicación

1. **Componentes modulares:**
   - Facilitan la reutilización y el mantenimiento
   - Permiten un desarrollo paralelo más eficiente
   - Organización por funcionalidad y responsabilidad

2. **Hooks personalizados:**
   - Encapsulan la lógica compleja
   - Mejoran la separación de responsabilidades
   - Facilitan pruebas unitarias

3. **Gestión de estado:**
   - Utilización de Context API para estado global (logros, autenticación, progreso)
   - Estado local para componentes específicos
   - Optimización con useCallback y useMemo para evitar renderizados innecesarios

4. **Sistema de progreso y logros:**
   - Almacenamiento en Supabase para persistencia
   - Verificación y actualización eficiente
   - Notificaciones visuales con animaciones

5. **Estructura de archivos:**
   - Organización por tipo y funcionalidad
   - Separación clara entre componentes, hooks, contextos y utilidades
   - Reutilización de lógica común

### Módulos Educativos

1. **Módulo 1: Conceptos básicos de Contabilidad**
   - Visualizador de esquemas de mayor (38 esquemas)
   - Juego de clasificación de cuentas
   - Juego de estructuración del balance general
   - Conceptos básicos del balance general
   - Juego de estructuración del estado de resultados
   - Conceptos básicos del estado de resultados

2. **Módulo 2: Finanzas personales** (en desarrollo)
   - Introducción a finanzas personales
   - Juego de clasificación de costos fijos y variables
   - Ejercicios prácticos de finanzas personales
   - Estrategias para saldar deudas
   - Juego de toma de decisiones para saldar deudas
   - Herramienta para determinar costos de proyectos escolares

3. **Módulo 3: Pagos provisionales y declaración anual** (planificado)
   - Conceptos básicos según normatividad vigente
   - Plantillas para prellenado de pagos provisionales
   - Plantillas para prellenado de declaración anual

## Conclusiones y Lecciones Aprendidas

1. **Importancia de la refactorización continua:**
   - Las mejoras constantes en componentes como BalanceSheetGame resultaron en un código más limpio y mantenible
   - La migración de bibliotecas (como de react-beautiful-dnd a @dnd-kit) mejoró el rendimiento y la accesibilidad
   - La eliminación regular de código no utilizado mantiene el proyecto optimizado

2. **Enfoque en la experiencia de usuario:**
   - Las animaciones, efectos visuales y un diseño responsivo mejoraron significativamente la interacción
   - El soporte para dispositivos táctiles amplió el alcance de la aplicación
   - El sistema de gamificación aumenta la motivación y el compromiso del usuario

3. **Optimización temprana:**
   - La implementación de useCallback y la eliminación de efectos innecesarios previnieron problemas de rendimiento
   - La gestión eficiente del estado redujo renderizados innecesarios
   - La selección cuidadosa de dependencias evitó problemas de compatibilidad

4. **Documentación y seguimiento:**
   - El mantenimiento de TAREAS_IEMS-CONTABLE.md facilitó el seguimiento del progreso
   - La estructura clara de commits permitió una evolución controlada del proyecto
   - La documentación detallada facilita la incorporación de nuevos desarrolladores

5. **Diseño orientado a dispositivos móviles:**
   - La adaptación de componentes para dispositivos móviles desde etapas tempranas evitó refactorizaciones mayores
   - El enfoque en interacciones táctiles mejoró la experiencia en smartphones y tablets
   - Las pruebas en múltiples dispositivos permitieron identificar y resolver problemas de usabilidad

## Próximos Pasos

1. **Mejoras pendientes:**
   - Implementar realimentación visual y sonora durante los juegos
   - Completar sistema de clasificación (leaderboard) entre usuarios
   - Terminar mejoras de responsividad en componentes de juegos
   - Corregir errores de tipado pendientes

2. **Nuevas funcionalidades:**
   - Implementar sistema de ejercicios prácticos con operaciones contables
   - Desarrollar módulos de finanzas personales y pagos provisionales
   - Mejorar UI/UX general de la aplicación
   - Implementar sistema de seguimiento estadístico para estudiantes

3. **Optimizaciones:**
   - Mejorar rendimiento en juegos de arrastre y soltado
   - Implementar carga diferida (lazy loading) de módulos
   - Optimizar tamaño del bundle para carga inicial más rápida
   - Implementar estrategias de caché para datos frecuentemente utilizados 
