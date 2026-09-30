# ⚖️ Toca.pe — Plataforma LegalTech de Justicia Laboral

> *"Te toca cobrar lo que por ley te corresponde."*  
> **Plataforma tecnológica de contingencias laborales y conciliaciones en el Perú bajo modelo de Cuota Litis (No Win, No Fee).**

---

## 🚀 Visión del Proyecto

**Toca.pe** es la startup LegalTech diseñada para transformar y democratizar el acceso a la justicia laboral en el Perú. Ayudamos a trabajadores víctimas de despidos arbitrarios, hostilidad laboral o locaciones de servicios encubiertas a cobrar su liquidación y beneficios sociales impagos, sin pagar ni un solo sol por adelantado.

La plataforma combina un motor de captación digital masiva (Calculadora Laboral viral), triaje automatizado con verificación de SUNAT/RENIEC, y un sistema de gestión legal para abogados litigantes bajo la Nueva Ley Procesal del Trabajo (NLPT - Ley 29497) y vías extrajudiciales (SUNAFIL y Centros de Conciliación).

---

## 🏛️ Ecosistema de Repositorios

El proyecto está diseñado bajo una arquitectura de micro-frontends desacoplados conectados a un backend central API-First:
                            [ INTERNET ]
                                    │
             ┌──────────────────────┴──────────────────────┐
             ▼ (Pauta & Orgánico)                          ▼ (Leads Registrados)
     ┌───────────────┐                             ┌───────────────┐
     │ toca-marketing│                             │toca-portal-   │
     │ (Next.js SSR) │                             │usuarios (Vite)│
     │  toca.pe      │                             │ app.toca.pe   │
     └───────┬───────┘                             └───────┬───────┘
             │                                             │
             │ POST /leads                                 │ GET/POST /casos
             ▼                                             ▼
     ┌─────────────────────────────────────────────────────────────┐
     │                         toca-server                         │
     │             (Django REST Framework + PostgreSQL)            │
     │                      api.toca.pe                            │
     └───────────────┬─────────────────────────────┬───────────────┘
                     │                             │
                     ▼ (Gestión Legal)             ▼ (Workers en Background)
             ┌───────────────┐             ┌───────────────┐
             │toca-portal-   │             │Celery + Redis │
             │abogados (Vite)│             │• WhatsApp API │
             │legal.toca.pe  │             │• Scraper CEJ  │
             └───────────────┘             │• Docs NLPT    │
                                           └───────────────┘


### 1. [`toca-marketing`](https://github.com/toca-pe/toca-marketing)
* **Framework:** Next.js 14/15 (App Router, TypeScript, Tailwind CSS).
* **Despliegue:** Vercel / Cloudflare Pages.
* **Dominio:** `https://toca.pe`
* **Funcionalidad Principal:**
  * Motor de captación y optimización de pauta (Google Ads / Meta Ads).
  * **Calculadora Laboral interactiva (Lead Magnet):** cálculo instantáneo de indemnización por despido arbitrario (D.Leg 728), CTS, gratificaciones legales truncas (+9% de bonificación extraordinaria) y vacaciones no gozadas.
  * Captura inicial de datos del trabajador (DNI, Teléfono WhatsApp, Sueldo, Fechas de ingreso y cese).

### 2. [`toca-portal-usuarios`](https://github.com/toca-pe/toca-portal-usuarios)
* **Framework:** React + Vite (TypeScript, Tailwind CSS).
* **Despliegue:** Cloudflare Pages (SPA estática ultrarrápida).
* **Dominio:** `https://app.toca.pe`
* **Funcionalidad Principal:**
  * Acceso rápido sin contraseñas difíciles: **Login mediante DNI + código OTP por WhatsApp**.
  * Carga de medios probatorios desde el celular (fotos de boletas de pago, contratos de locación, cartas de despido y capturas de chats).
  * Firma digital simple del **Pacto de Cuota Litis** (acuerdo de honorarios a éxito).
  * Seguimiento en tiempo real tipo semáforo del estado del reclamo.

### 3. [`toca-portal-abogados`](https://github.com/toca-pe/toca-portal-abogados)
* **Framework:** React + Vite (TypeScript, Tailwind CSS, Lucide Icons, Tiptap Editor).
* **Despliegue:** Cloudflare Pages.
* **Dominio:** `https://legal.toca.pe`
* **Funcionalidad Principal:**
  * **Tablero Kanban de Casos:** Flujo visual de estados (*Nuevo Lead $\to$ Evaluación $\to$ Carta Notarial / MTPE $\to$ SUNAFIL $\to$ Demanda Judicial $\to$ Audiencia $\to$ Conciliado / Cobrado*).
  * **Ficha del Caso 360°:** Liquidación numérica auditada, expediente probatorio y notas de triage.
  * **Document Studio:** Generador automático de Cartas Notariales de Requerimiento y Demandas Laborales conformes a la NLPT (Ley 29497) listas para firmar con token/firma digital SINOE.
  * **Módulo de Conciliaciones & Cobranzas:** Registro de acuerdos extrajudiciales con valor de cosa juzgada y control de dispersión de transferencias bancarias (CCI).

### 4. [`toca-server`](https://github.com/toca-pe/toca-server)
* **Framework:** Python / Django (Django REST Framework, PostgreSQL, Celery, Redis).
* **Despliegue:** Docker en VPS / Render / Railway / AWS.
* **Dominio:** `https://api.toca.pe`
* **Funcionalidad Principal:**
  * Autenticación JWT y sistema de roles (*Paralegal, Abogado Patrocinante, Settlement Specialist, Admin*).
  * **Integraciones peruanas:** Consultas en tiempo real a APIs de RENIEC (DNI) y SUNAT (RUC / Razón Social y condición de Activo/Habido).
  * **Motor de Documentos:** Generación de archivos `.docx` y `.pdf` a partir de plantillas legales.
  * **Scraper CEJ (Poder Judicial):** Consultas nocturnas al sistema judicial para detectar resoluciones o programaciones de audiencias automáticamente.
  * **Mensajería:** Conexión con WhatsApp Business Cloud API para avisos y notificaciones al instante.

---

## ⚖️ Fundamento Legal y Modelo de Negocio

* **Pacto de Cuota Litis:** 100% legal en el Perú según los Arts. 52 y 53 del Código de Ética del Abogado. Se acuerda una comisión sobre éxito de entre el **25% y 35%** únicamente si el trabajador recupera su dinero.
* **Vías de Resolución Estratégica:**
  1. **Vía Extrajudicial Rápida:** Carta Notarial de Requerimiento + Conciliación en el MTPE o Centro Privado con título de ejecución.
  2. **Vía SUNAFIL:** Denuncia inspectiva virtual como palanca de presión por multas laborales.
  3. **Vía Judicial NLPT:** Demanda ante los Juzgados Especializados de Trabajo bajo la Ley 29497 (proceso abreviado u ordinario).
* **Red de Abogados Patrocinantes:** Convocatoria a abogados colegiados y hábiles en el Colegio de Abogados de Lima (CAL) y regiones, con Casilla Electrónica SINOE y Firma Digital activa, bajo esquema de comisión compartida por caso atendido.

---

## 🛠️ Buenas Prácticas de Desarrollo

Para garantizar la estabilidad y escalabilidad de la plataforma desde el día 1:

1. **TypeScript Estricto en Frontend:** No usar tipos `any`. Definir interfaces claras para los cálculos laborales y respuestas de la API.
2. **API-First & Swagger/OpenAPI:** Toda funcionalidad en Django debe estar documentada mediante `drf-spectacular` para que los desarrolladores de frontend consuman endpoints con contratos claros.
3. **Mobile-First & WhatsApp-First:** Todo el diseño para el trabajador debe estar pensado para resoluciones de smartphone y conexiones móviles estándar de Perú.
4. **Seguridad y Privacidad (Ley 29733 - LPDP):** Los datos personales y documentos probatorios sensibles de los trabajadores deben almacenarse cifrados en reposo en almacenamiento seguro (S3 / Cloudflare R2).

---

## 👥 Equipo y Contacto

* **Startup:** Toca.pe
* **Operación:** Lima, Perú
* **Contacto:** contacto@toca.pe / legal@toca.pe
