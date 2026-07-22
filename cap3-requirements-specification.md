<div style="page-break-before: always;"></div>

# Chapter III: Requirements Specification

El enfoque esta en que el CEO y los vendedores tengan una pagina web para la gestion de la empresa, por el momento los clientes solo con acesso a la landing page.

## 3.1. User Stories

La creación de las Epic y los TS se basan en los User Stories, Epic para el punto de vista del usuario (FrontEnd) y los Technical Stories para el punto de vista del developer (BackEnd).


# Documentación de User Stories & Technical Stories - Vidriería Alpamayo

## EPIC01: Landing Page Comercial (Clientes / Visitantes)

| ID | Rol | Título | Descripción | Criterios de Aceptación | Épica |
|:---|:---|:-------|:------------|:------------------------|:------|
| **US01** | Cliente / Visitante | Catálogo Informativo de Productos | Como visitante de la web, deseo explorar los tipos de vidrios y perfiles en la Landing Page, para conocer la oferta del negocio. | **Escenario 1: Consulta del catálogo público**<br>**Dado que** el usuario ingresa a la Landing Page<br>**Cuando** navega a la sección de productos<br>**Entonces** visualiza la lista de vidrios (templado, laminado, crudo) con grosores disponibles e imágenes de muestra<br><br>**Escenario 2: Filtro por perfiles de aluminio**<br>**Dado que** el cliente revisa la Landing Page<br>**Cuando** selecciona la categoría "Aluminio y Accesorios"<br>**Entonces** se despliegan los tipos de perfiles (Serie 20/25, colores mate y bronce) | EPIC01 |
| **US02** | Cliente / Visitante | Solicitud de Cotización Directa | Como cliente interesado, deseo enviar mis datos y detalles de mi proyecto desde la Landing Page, para ser contactado por un vendedor. | **Escenario 1: Envío exitoso de formulario**<br>**Dado que** el cliente está en la Landing Page<br>**Cuando** completa su Nombre, Teléfono, Ciudad y detalle de la obra y presiona "Solicitar Cotización"<br>**Entonces** se muestra el mensaje "Gracias por contactar a Vidriería Alpamayo. Un vendedor se comunicará contigo"<br>**Y** la solicitud se registra en el panel de los Vendedores<br><br>**Escenario 2: Validación de teléfono obligatorio**<br>**Dado que** el cliente deja el teléfono en blanco<br>**Cuando** presiona "Solicitar Cotización"<br>**Entonces** el sistema muestra la alerta "El número telefónico es obligatorio" | EPIC01 |

---

## EPIC02: Autenticación, Usuarios y Seguridad Perimetral (CEO & Vendedores)

| ID | Rol | Título | Descripción | Criterios de Aceptación | Épica |
|:---|:---|:-------|:------------|:------------------------|:------|
| **US03** | CEO / Administrador | Gestión y Alta de Usuarios Internos | Como CEO, deseo crear y administrar las cuentas de los vendedores, para controlar quién accede al sistema interno. | **Escenario 1: Registro de nuevo Vendedor**<br>**Dado que** el CEO ingresa al panel `/admin/users`<br>**Cuando** registra los datos del trabajador, selecciona el rol "Vendedor" y guarda<br>**Entonces** se crea la cuenta activa y se generan las credenciales de acceso<br><br>**Escenario 2: Eliminación de cuenta**<br>**Dado que** un vendedor deja de laborar en la empresa<br>**Cuando** el CEO cambia su estado a "Eliminado"<br>**Entonces** se revocan sus accesos y se invalida su sesión inmediatamente | EPIC02 |
| **US04** | CEO / Vendedor | Inicio de Sesión Interno con JWT | Como trabajador, deseo autenticarme de forma segura, para obtener un token JWT que autorice mis operaciones comerciales. | **Escenario 1: Autenticación exitosa**<br>**Dado que** el trabajador ingresa sus credenciales en el subdominio autorizado<br>**Cuando** el sistema valida su correo y contraseña encriptada<br>**Entonces** emite un token JWT firmado (Bearer Token) con expiración de 24h y otorga acceso al panel interno<br><br>**Escenario 2: Intento fallido**<br>**Dado que** el usuario ingresa una clave incorrecta<br>**Cuando** presiona "Ingresar"<br>**Entonces** el sistema responde con error HTTP 401 Unauthorized y no permite el ingreso | EPIC02 |
| **US05** | CEO / Administrador | Control de Acceso por IP y Subdominio | Como CEO, deseo restringir el ingreso a `admin.vidrieriaalpamayo.com` únicamente a las IPs de la vidriería, para evitar intromisiones remotas. | **Escenario 1: Acceso desde IP autorizada**<br>**Dado que** el CEO o Vendedor intenta ingresar al subdominio administrativo<br>**Cuando** se conecta desde la red con IP autorizada (taller/sucursal)<br>**Entonces** el sistema le permite cargar la pantalla de login<br><br>**Escenario 2: Bloqueo de IP no autorizada**<br>**Dado que** una persona intenta ingresar desde una IP no registrada<br>**Cuando** el firewall/API Gateway evalúa la petición<br>**Entonces** rechaza la conexión con error HTTP 403 Forbidden y registra la IP en el log de auditoría | EPIC02 |

---

## EPIC03: Cotizador Comercial e Inteligencia de Mermas (CEO & Vendedores)

| ID | Rol | Título | Descripción | Criterios de Aceptación | Épica |
|:---|:---|:-------|:------------|:------------------------|:------|
| **US06** | Vendedor / CEO | Calculadora Interna de Vidrios a Medida | Como Vendedor, deseo calcular el costo exacto del vidrio ingresando alto, ancho, grosor y acabados, para cotizar al cliente en tiempo real. | **Escenario 1: Cálculo automático por área**<br>**Dado que** el vendedor atiende a un cliente<br>**Cuando** ingresa Alto (150 cm), Ancho (100 cm), Tipo de vidrio, grosor (6mm) y canto pulido<br>**Entonces** el cotizador calcula el p² y aplica la fórmula: Medida × Medida/p² + Acabados<br><br>**Escenario 2: Aplicación de cobro por medida mínima**<br>**Dado que** el cliente solicita un vidrio pequeño de 10x10 cm<br>**Cuando** se procesa la cotización<br>**Entonces** el cotizador cobra la tarifa sobre el área mínima establecida por la empresa | EPIC03 |
| **US07** | Vendedor / CEO | Cotización de Estructuras de Aluminio | Como Vendedor, deseo agregar marcos y perfiles de aluminio a la cotización, para vender estructuras completas (ventanas/mamparas). | **Escenario 1: Adición de perfiles y accesorios**<br>**Dado que** el vendedor arma la cotización de una ventana corrediza<br>**Cuando** selecciona la Serie 20 (color mate), especifica los metros lineales y añade accesorios (felpas, rodajes)<br>**Entonces** el sistema suma el costo del aluminio al costo total del vidrio | EPIC03 |
| **US08** | Vendedor / CEO | Gestor Multi-Unidad de Medidas (mm, cm, m, pulgadas) | Como Vendedor, deseo ingresar dimensiones en la unidad que solicite el cliente (milímetros, centímetros, metros o pulgadas), para cotizar sin conversiones manuales y evitar errores en producción. | **Escenario 1: Cotización con sistema imperial (pulgadas)**<br>**Dado que** un cliente solicita piezas en pulgadas (ej. 48" x 36")<br>**Cuando** el vendedor selecciona "Pulgadas" como unidad de entrada<br>**Entonces** el cotizador convierte automáticamente las medidas al estándar de taller (cm/mm) y calcula el precio exacto<br><br>**Escenario 2: Conmutación dinámica de unidad**<br>**Dado que** las dimensiones están ingresadas en centímetros<br>**Cuando** el vendedor cambia la unidad a milímetros<br>**Entonces** los valores se transforman a la nueva escala sin alterar las dimensiones reales | EPIC03 |
| **US09** | Vendedor / Operario | Carga e Importación Masiva de Despiece desde Excel | Como Vendedor / Operario, deseo cargar planillas en Excel (.xlsx / .csv) con el listado de medidas, cantidades, unidades y tipo de material, para agilizar la cotización de proyectos de gran escala sin tipeo manual. | **Escenario 1: Importación masiva exitosa**<br>**Dado que** el vendedor tiene un Excel de obra con 50 piezas (Alto, Ancho, Cantidad, Unidad, Tipo de Vidrio)<br>**Cuando** sube la plantilla y valida la asignación de columnas<br>**Entonces** el cotizador carga automáticamente todas las líneas, calcula las áreas individuales y el total general del proyecto<br><br>**Escenario 2: Control de errores en archivo subido**<br>**Dado que** el archivo Excel contiene filas con datos no numéricos o vacíos en las dimensiones<br>**Cuando** el sistema procesa el documento<br>**Entonces** muestra un reporte de error detallando "Línea 12: Dimensión no válida" y permite corregirla antes de confirmar la carga | EPIC03 |
| **US10** | Vendedor / Operario | Optimización Algorítmica de Recortes (Bin Packing NP) | Como Vendedor / Operario, deseo ejecutar un algoritmo de optimización de recortes (Bin Packing 2D / Corte Guillotina), para minimizar el desperdicio de vidrio/aluminio y calcular el máximo aprovechamiento de las planchas. | **Escenario 1: Generación del esquema de corte óptimo**<br>**Dado que** se procesa una orden con múltiples cortes de distintas medidas<br>**Cuando** se ejecuta el optimizador de recortes<br>**Entonces** el algoritmo (NP-Hard) distribuye las piezas sobre las planchas/barras estándar, reduciendo la merma al mínimo y calculando el porcentaje de eficiencia<br><br>**Escenario 2: Integración prioritaria de retazos**<br>**Dado que** existen retazos registrados en almacén<br>**Cuando** el algoritmo calcula el plano de corte<br>**Entonces** prioriza ubicar piezas en los retazos disponibles antes de recortar una plancha o barra nueva | EPIC03 |
| **US11** | Vendedor / CEO | Sugerencia y Aprovechamiento de Retazos | Como Vendedor, deseo consultar sobrantes reutilizables en almacén, para usarlos en cortes pequeños y reducir desperdicios. | **Escenario 1: Asignación automática de retazo**<br>**Dado que** existe un retazo de vidrio laminado de 80x60 cm registrado en almacén<br>**Cuando** el vendedor cotiza una pieza de 40x30 cm del mismo material<br>**Entonces** el cotizador sugiere "Aprovechar Retazo #RT-12" evitando cortar una plancha completa | EPIC03 |

---

## EPIC04: Gestión de Pedidos y Estado de Producción (CEO & Vendedores)

| ID | Rol | Título | Descripción | Criterios de Aceptación | Épica |
|:---|:---|:-------|:------------|:------------------------|:------|
| **US12** | Vendedor / CEO | Conversión de Cotización a Pedido | Como Vendedor, deseo transformar una cotización en pedido registrando el cobro del adelanto inicial, para iniciar la producción. | **Escenario 1: Confirmación de pedido con adelanto**<br>**Dado que** una cotización está aprobada<br>**Cuando** el vendedor registra el abono del 50% en efectivo, Yape o transferencia<br>**Entonces** la cotización cambia a estado "Pedido Confirmado" y se envía la orden de corte al taller<br><br>**Escenario 2: Bloqueo por falta de abono**<br>**Dado que** una cotización no tiene pago registrado<br>**Cuando** se intenta enviar a producción<br>**Entonces** el sistema muestra la alerta "Se requiere ingresar un adelanto para procesar el pedido" | EPIC04 |
| **US13** | Vendedor / CEO | Seguimiento de Pedidos en Taller | Como Vendedor o CEO, deseo monitorear la fase de preparación de los vidrios, para coordinar la entrega con el cliente. | **Escenario 1: Actualización del proceso de corte**<br>**Dado que** el operario o vendedor revisa el pedido #1080<br>**Cuando** actualiza la fase de "En Corte" a "Listo para Recojo/Entrega"<br>**Entonces** el estado se actualiza en el panel comercial notificando que el producto está listo en almacén | EPIC04 |

---

## EPIC05: Inventario, Facturación y Analítica (CEO & Vendedores)

| ID | Rol | Título | Descripción | Criterios de Aceptación | Épica |
|:---|:---|:-------|:------------|:------------------------|:------|
| **US14** | CEO / Administrador | Control de Stock y Alerta de Reabastecimiento | Como CEO, deseo controlar el inventario de planchas y perfiles, para reponer insumos oportunamente. | **Escenario 1: Descuento automático de stock**<br>**Dado que** existen 20 planchas de Vidrio Monolítico 4mm<br>**Cuando** se confirma un pedido que consume 2 planchas<br>**Entonces** el stock se descuenta automáticamente a 18 planchas<br><br>**Escenario 2: Alerta de stock crítico**<br>**Dado que** el stock de perfiles llega a 3 unidades (umbral mínimo: 5)<br>**Cuando** se actualiza el almacén<br>**Entonces** se resalta el producto en rojo en el panel del CEO con la alerta "Stock Mínimo Alcanzado" | EPIC05 |
| **US15** | Vendedor / CEO | Emisión de Proformas en PDF | Como Vendedor, deseo generar proformas oficiales en PDF, para entregar al cliente en mostrador o WhatsApp. | **Escenario 1: Generación de PDF**<br>**Dado que** una cotización está terminada<br>**Cuando** el vendedor hace clic en "Descargar Proforma"<br>**Entonces** se genera un documento PDF con el logo de Vidriería Alpamayo, RUC, medidas, desglose de vidrios, IGV y condiciones de venta | EPIC05 |
| **US16** | CEO / Administrador | Dashboard de Reportes de Ventas para el CEO | Como CEO, deseo visualizar gráficos e indicadores de ingresos, para evaluar la rentabilidad del negocio. | **Escenario 1: Reporte financiero mensual**<br>**Dado que** el CEO accede a `/admin/reports`<br>**Cuando** selecciona el mes en curso<br>**Entonces** el dashboard muestra el total de ingresos cobrados, saldos pendientes de cobro y el ranking de materiales más vendidos | EPIC05 |

---

## EPIC06: Localización e Idioma (i18n) (Clientes / Visitantes)

| ID | Rol | Título | Descripción | Criterios de Aceptación | Épica |
|:---|:---|:-------|:------------|:------------------------|:------|
| **US17** | Cliente / Visitante | Conmutador de Idioma (ES/EN) en Landing | Como visitante extranjero, deseo cambiar el idioma de la Landing Page a inglés, para entender los servicios ofertados. | **Escenario 1: Cambio de idioma en Landing Page**<br>**Dado que** el usuario navega en la Landing Page pública<br>**Cuando** selecciona "English" en el menú superior<br>**Entonces** los títulos, descripciones del catálogo y formulario de contacto se traducen al inglés en tiempo real | EPIC06 |

---

## EPIC07: Infraestructura y Backend (Technical Stories)

| ID | Rol | Título | Descripción | Criterios de Aceptación | Épica |
|:---|:---|:-------|:------------|:------------------------|:------|
| **TS01** | Equipo Técnico | Arquitectura Base Vue 3 + Pinia + PrimeVue | Como Developer, deseo configurar la base del proyecto frontend, para optimizar el cotizador en tiempo real. | **Escenario 1: Compilación de arquitectura**<br>**Dado que** se tiene el repositorio frontend<br>**Cuando** se ejecuta `npm run dev`<br>**Entonces** el servidor inicia integrando Pinia Store (para el estado del cotizador) y PrimeVue para los componentes UI | EPIC07 |
| **TS02** | Equipo Técnico | Pipeline CI/CD para Landing Page en GitHub Pages | Como DevOps Engineer, deseo automatizar el despliegue de la Landing Page en GitHub Pages, para actualizar los cambios tras cada commit. | **Escenario 1: Despliegue continuo**<br>**Dado que** se realiza un push en la rama `main`<br>**Cuando** GitHub Actions ejecuta el build<br>**Entonces** se compilan los archivos estáticos y la Landing Page se actualiza de forma transparente | EPIC07 |
| **TS03** | Equipo Técnico | Aislamiento por Subdominios y Enrutamiento HTTPS | Como DevOps Engineer, deseo forzar el uso de HTTPS/TLS y configurar subdominios independientes, para garantizar comunicaciones seguras. | **Escenario 1: Redirección SSL forzada**<br>**Dado que** se recibe una petición HTTP por el puerto 80<br>**Cuando** llega al servidor web<br>**Entonces** se redirige con HTTP 301 a la versión segura HTTPS (puerto 443)<br><br>**Escenario 2: Aislamiento por subdominio**<br>**Dado que** hay tráfico hacia `vidrieriaalpamayo.com` y `admin.vidrieriaalpamayo.com`<br>**Cuando** el servidor resuelve la petición<br>**Entonces** aplica reglas CORS y rutas aisladas para cada entorno | EPIC07 |
| **TS04** | Equipo Técnico | Middleware Backend de Validación JWT y Filtrado IP | Como Backend Developer, deseo implementar un middleware que verifique el token JWT y valide la IP de origen, para proteger las llamadas privadas. | **Escenario 1: Validación de IP y JWT**<br>**Dado que** se envía una petición a un endpoint privado de la API<br>**Cuando** el Middleware intercepta la llamada<br>**Entonces** verifica que el token JWT sea válido y que la IP del origen coincida con el rango permitido antes de resolver la solicitud | EPIC07 |

## 3.2. Impact Mapping

## 3.3. Product Backlog

Para definir el orden se toma en cuenta la prioridad en el negocio.