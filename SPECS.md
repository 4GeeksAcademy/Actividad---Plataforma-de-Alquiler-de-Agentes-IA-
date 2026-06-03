# Documento de Especificación Técnica: Panel de Administración de AgentHub (SPECS.md)

## 1. Descripción del Producto
AgentHub es una plataforma SaaS diseñada para que empresas puedan alquilar agentes de Inteligencia Artificial (IA) preconfigurados. Estos agentes actúan como asistentes inteligentes que se pueden equipar con habilidades específicas (*skills*) como la navegación web, la lectura y análisis de documentos, o la gestión automatizada de calendarios, permitiendo su despliegue inmediato en flujos de trabajo de negocio.

El **usuario administrador** de este panel interno es un miembro del equipo de operaciones, soporte o gestión de AgentHub. Sus principales necesidades consisten en supervisar la actividad global de la plataforma, gestionar el catálogo de agentes disponibles, controlar las habilidades asignadas, auditar las ejecuciones en tiempo real, monitorear los costos/consumos de infraestructura de IA y gestionar las cuentas de las empresas clientes (*tenants*).

---

## 2. Stack Tecnológico y Restricciones
Para garantizar la portabilidad y simplicidad en la validación por parte del equipo de backend, el prototipo debe cumplir estrictamente con las siguientes restricciones de ingeniería:
- **HTML5 Estructural:** Marcado semántico limpio (`<header>`, `<nav>`, `<aside>`, `<main>`, `<section>`, `<table>`, etc.) sin preprocesadores.
- **Tailwind CSS vía CDN:** Estilización completa utilizando únicamente clases utilitarias provistas por el script oficial de Tailwind CSS. No se permite compilación local con Node.js ni archivos CSS personalizados.
- **JavaScript Vanilla (ES6+):** Toda la interactividad del lado del cliente debe implementarse en código Javascript puro, directamente en etiquetas `<script>` o un archivo `.js` local independiente.
- **Sin Frameworks:** Prohibido el uso de React, Vue, Angular, Svelte, Alpine.js o cualquier librería reactiva.
- **Sin Backend:** El prototipo es 100% estático. Toda la persistencia de estado interactivo (ej. alternancia de modales, dropdowns o temas) ocurrirá en memoria o mediante el almacenamiento del navegador (`localStorage`), con datos estáticos iniciales en código (*hardcodeados*).

---

## 3. Especificaciones Detalladas por Sección

### Vista 1: Dashboard Principal
1. **Cuadrícula de Métricas:** Cuatro tarjetas de métricas distribuidas en una cuadrícula responsive 2x2 en pantallas medianas/grandes y 1x1 en móviles. Cada tarjeta contendrá un icono representativo (SVG), una etiqueta descriptiva (ej. "Agentes Activos"), un valor principal hardcodeado de gran tamaño y una tasa de cambio porcentual en tipografía pequeña.
2. **Estilo Visual de Tarjetas:** Las tarjetas deben aplicar un fondo sólido con una sombra sutil (`shadow-sm`), bordes redondeados (`rounded-xl`) y utilizar colores de acento diferenciados según la naturaleza de la métrica (ej. verde para ingresos, azul para agentes, púrpura para ejecuciones, amarillo para alertas).
3. **Gráfico de Actividad Semanal:** Inmediatamente debajo de la cuadrícula de tarjetas, se ubicará un contenedor de ancho completo (`w-full`) con un borde discontinuo (`border-dashed border-2`) y esquinas redondeadas. En su interior, una etiqueta de texto centrada vertical y horizontalmente representará el marcador de posición (*placeholder*) para el gráfico de actividad semanal.

### Vista 2: Catálogo de Agentes
1. **Grilla de Tarjetas de Agentes:** Una cuadrícula adaptativa que muestre las tarjetas de los agentes disponibles (ej. "Agente de Soporte", "Agente de Análisis Legal"). Cada tarjeta incluirá un avatar circular, el nombre del agente, su estado (badge de "Disponible" o "Mantenimiento") y una descripción breve.
2. **Selector de Estado Individual:** Cada tarjeta de agente incluirá un interruptor (*toggle UI*) interactivo que simule la activación/desactivación del agente en el catálogo, cambiando visualmente la opacidad de la tarjeta cuando el agente esté inactivo.
3. **Panel Inyectado de Skills:** Al final de la descripción de cada agente, se renderizará un listado horizontal de *badges* pequeños que indiquen las habilidades nativas instaladas (ej. "Web Scraping", "OAuth Calendar"), cambiando de color al pasar el cursor (*hover*).

### Vista 3: Gestión de Habilidades (Skills)
1. **Tabla de Control de Skills:** Una tabla estructurada que liste las habilidades disponibles en la plataforma. Columnas requeridas: Nombre de la Skill, Tipo de Permiso (API Key, Lectura, Escritura), Fecha de Actualización y un menú de Acciones Rápidas por fila.
2. **Buscador y Filtrado Superior:** Una barra de herramientas superior con un campo de entrada (`<input type="search">`) con un icono de lupa integrado y un menú desplegable (`<select>`) para filtrar las habilidades por categoría tecnológica (ej. Integraciones, Scraping, Procesamiento de Texto).
3. **Indicador de Carga de Uso:** Cada fila incorporará una barra de progreso visual (un `div` con ancho porcentual de fondo de color de acento) que denote de forma estática el volumen de agentes que consumen dicha habilidad en la plataforma actual.

### Vista 4: Monitor de Ejecuciones (Logs)
1. **Consola Secuencial de Logs:** Un bloque de texto preformateado que simule una consola de desarrollo con fondo oscuro continuo (`bg-slate-900 text-green-400 font-mono text-xs p-4 rounded-lg overflow-y-auto max-h-96`). Mostrará registros cronológicos con marcas de tiempo (*timestamps*).
2. **Badges de Gravedad:** Cada línea de log o fila de auditoría estará antecedida por un componente *badge* de color estricto que determine la severidad del evento: verde para `SUCCESS`, azul para `INFO`, amarillo para `WARNING` y rojo para `CRITICAL`.
3. **Paginación del Monitor:** Un componente de barra de paginación al pie de la lista con botones estructurados de "Anterior", "Siguiente" y números de página, estilizados con estados activos/inactivos para validar el flujo del usuario administrador.

### Vista 5: Configuración de Clientes (Tenants)
1. **Lista de Empresas Contratantes:** Un diseño de lista detallada (*split-screen* o *stacked list*) que muestre los nombres de las empresas clientes, sus planes de suscripción (Enterprise, Business, Startup) y el número de agentes que tienen alquilados simultáneamente.
2. **Modificaciones de Cuota:** Cada cliente tendrá un campo numérico editable con controles para alterar los límites de tokens de IA asignados mensualmente, acompañado de un botón de guardado simulado.
3. **Alerta de Sobrecosto:** Si la cuota simulada de un cliente supera el 90%, el componente de la fila debe renderizar un icono de advertencia parpadeante o resaltado en color ámbar para denotar atención prioritaria.

### Vista 6: Configuración del Sistema
1. **Formularios de API Keys globales:** Secciones estructuradas con campos de texto dedicados para la configuración de llaves de proveedores de modelos (ej. OpenAI, Anthropic). Los campos deben incluir un botón con un icono de ojo para "Mostrar/Ocultar" la clave simulada.
2. **Toggle de Modos de Redirección:** Interruptores visuales claros de tipo switch para activar funciones del sistema como "Modo de Mantenimiento Global", "Logs Depurados Avanzados" y "Forzar Almacenamiento en Caché".
3. **Panel de Almacenamiento de Datos:** Un bloque informativo que resuma los límites del plan maestro de la infraestructura mediante gráficos de barras nativos en HTML/Tailwind, indicando uso de memoria, llamadas a APIs globales y espacio en disco duro simulado.

---

## 4. Inventario de Componentes Reutilizables
Para asegurar la consistencia visual y la modularidad del código, se identifican y especifican los siguientes componentes de UI que operarán de manera transversal en el prototipo:

- **Sidebar (Menú Lateral de Navegación):** Barra fija a la izquierda con el logotipo de AgentHub, enlaces con estados *hover* y activos para las 6 vistas primarias, y el perfil del administrador en la parte inferior. Colapsable en dispositivos móviles a un menú de tipo hamburguesa.
- **Tarjeta de Métrica (Metric Card):** Contenedor con bordes redondeados (`rounded-xl`), fondo blanco/oscuro, sombra sutil, título de métrica grisáceo, número destacado y un indicador de tendencia con una flecha SVG orientada arriba/abajo.
- **Dropdown de Acciones:** Menú contextual flotante que aparece al hacer clic en los botones de tres puntos (`...`) de las tablas o listas. Debe posicionarse de forma absoluta sobre las capas adyacentes y ocultarse al hacer clic fuera.
- **Modal de Confirmación/Creación:** Caja de diálogo superpuesta con un fondo semi-transparente difuminado (`backdrop-blur-sm`). Contiene un título de acción, cuerpo con formulario o texto de advertencia, un botón de confirmación destacado y un botón de cancelación gris.
- **Badge de Estado:** Etiquetas de texto compactas con texto en mayúsculas, esquinas completamente redondeadas (`rounded-full`), tipografía en negrita (`font-semibold`) y colores pastel de fondo con texto oscuro contrastante para estados del sistema.
- **Lista de Skills Colapsable:** Acordeón interactivo vertical que expande o contrae los detalles técnicos y variables de una habilidad específica al hacer clic en el encabezado.
- **Toggle de Modo Oscuro:** Interruptor circular deslizable ubicado comúnmente en el sidebar o el encabezado superior para alternar los esquemas de color de la interfaz completa.

---

## 5. Criterios de Aceptación
El prototipo HTML se considerará completo y listo para su entrega a los desarrolladores de backend cuando cumpla de forma verificable con las siguientes condiciones:

1. **Navegación entre Vistas:** El clic en cualquiera de los enlaces del *Sidebar* debe cambiar la vista en pantalla de forma interactiva (ya sea mediante manipulación del DOM ocultando/mostrando secciones con JS vanilla o cargando subpáginas consistentes) sin romper el diseño global.
2. **Interactividad de Dropdowns:** Al hacer clic en cualquier botón de menú de acciones de una tabla, el menú desplegable (*Dropdown*) correspondiente debe aparecer de forma inmediata. Al hacer clic en cualquier otra parte del documento, el dropdown debe cerrarse automáticamente.
3. **Comportamiento del Modal:** Al presionar un botón de creación (ej. "Agregar Nueva Skill" o "Editar Tenant"), el componente *Modal* debe emerger con una animación sutil. El botón de cerrar (icono "X") o el botón de "Cancelar" deben cerrar la ventana superpuesta restableciendo la vista base.
4. **Acordeón Colapsable:** Al pulsar sobre una habilidad o elemento de la *Lista de Skills Colapsable*, el contenedor de información secundaria debe deslizarse o aparecer de inmediato, mutando el icono de la flecha indicadora del encabezado (de derecha a abajo o viceversa).
5. **Alternancia de Modo Oscuro (Dark Mode):** El activador de modo oscuro debe transformar la paleta de colores global del prototipo de forma armoniosa. Al encenderse, las clases de Tailwind de fondo claro (`bg-white`, `bg-slate-50`) deben mutar a fondos oscuros (`bg-slate-950`, `bg-slate-900`) y los textos oscuros a claros de manera consistente en todas las pantallas.
6. **Diseño Adaptativo (Responsive Design):** La interfaz completa debe ser totalmente navegable y visualmente correcta en tres resoluciones de pantalla estándar de prueba: Móvil (375px de ancho), Tablet (768px de ancho) y Escritorio (1440px de ancho) sin presentar desbordamientos horizontales de texto o componentes (`overflow-x` no deseado).
