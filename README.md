# Vigía HH — Modelamiento de paradas mayores y optimización de horas hombre

> **Capstone (APT122) · Ingeniería en Informática · Duoc UC** — Metodología: Cascada
> **Sección:** `Sigla_seccion` · **Equipo:** `Equipo_XX`
> **Caso base:** ENAP (extensible a otros clientes) · *nombre de trabajo, por confirmar*

Plataforma que modela las modalidades de jornada de una **parada mayor de planta**,
optimiza las **horas hombre (HH)** y **alerta cuando se supera un límite** —de
jornada, plazo o alcance—, comparando escenarios de costo, supervisión y cumplimiento.

No reemplaza al cronograma (Primavera P6, Cleopatra): se ubica **encima** de la
lógica de HH y agrega la capa de cumplimiento normativo chileno que esos software
globales no modelan.

---

## Tabla de contenido
- [El problema y la solución](#el-problema-y-la-solución)
- [Cómo levantar el sistema](#cómo-levantar-el-sistema)
- [Cómo correr las pruebas](#cómo-correr-las-pruebas)
- [Estructura del repositorio](#estructura-del-repositorio)
- [Metodología (cascada) y fases](#metodología-cascada-y-fases)
- [Elementos obligatorios del instructivo](#elementos-obligatorios-del-instructivo)
- [Equipo](#equipo)

---

## El problema y la solución

Cuando una planta se detiene para una parada mayor, no produce, y cada día parado
cuesta muchísimo dinero. Hoy las HH y los turnos se combinan **a mano, en Excel**,
con alto riesgo de error y de superar sin querer un límite legal.

**Cada modalidad tiene su propio tipo de límite** y no se controla igual:

| Modalidad | Tipo de límite | Qué se controla |
|---|---|---|
| 40 horas (jornada ordinaria) | **jornada** | Horas/día y horas/semana |
| 4×3 / 7×7 (sistemas excepcionales) | **jornada** | Horas/día y días del ciclo (autorización DT) |
| Servicios transitorios (Ley 20.123) | **plazo** | 90 días (eventos) / 180 días (proyectos), sin renovación |
| Contrato por obra o faena | **alcance** | Vale mientras exista la obra |

> Los topes son **referenciales** y deben confirmarse con el líder técnico y los
> convenios colectivos vigentes.

---

## Cómo levantar el sistema

### Opción A — Docker (recomendada)
```bash
cp .env.example .env      # y edita las credenciales
docker compose up --build
```
La API queda en `http://localhost:8000` y la documentación interactiva en
`http://localhost:8000/docs`.

### Opción B — Local (sin Docker)
```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
uvicorn api.main:app --reload
```

---

## Cómo correr las pruebas
```bash
pytest -q                 # todas las pruebas
pytest -q --cov=motor     # con reporte de cobertura
```

---

## Estructura del repositorio
```
vigia-hh/
├── motor/                 # Núcleo: validadores normativos y supervisión
│   ├── validadores.py     #   jornada / plazo / alcance
│   └── supervision.py     #   brecha de supervisión
├── api/                   # API REST (FastAPI)
│   └── main.py
├── tests/                 # Pruebas unitarias (pytest)
├── Fase 1/                # Evidencia de la fase (convención APT122)
│   ├── Evidencias Individuales/
│   └── Evidencias Grupales/
├── docker-compose.yml
├── Dockerfile
└── requirements.txt
```
> Las carpetas `Fase 2` … `Fase 5` se agregan a medida que avanza el proyecto,
> siguiendo la misma convención de nombres. Los artefactos de diseño (arquitectura,
> modelo de datos, UML) irán en `Fase 2/Evidencias Grupales`.

---

## Metodología (cascada) y fases

| Fase | Artefacto (evidencia) | Estado |
|---|---|---|
| 1 · Requerimientos / Definición | Guía del Estudiante — Definición del Proyecto APT (+ formativa, autoevaluaciones) | 🟡 en curso |
| 2 · Desarrollo (Avance) | Guía del Estudiante — Desarrollo + informe de avance | ⬜ |
| 3 · Diseño / Desarrollo | Documento de Diseño + código fuente versionado | ⬜ |
| 4 · Pruebas | Plan de Pruebas + evidencias | ⬜ |
| 5 · Despliegue | Manual Técnico / de Despliegue | ⬜ |

---

## Elementos obligatorios del instructivo

1. **Arquitectura** → `Fase 2/Evidencias Grupales/` (diagrama de componentes y comunicación)
2. **Modelo de datos** → `Fase 2/Evidencias Grupales/` (ER: trabajador, paquete, asignación, escenario)
3. **Diagramas UML mínimos** → `Fase 2/Evidencias Grupales/` (casos de uso, clases, secuencia)
4. **Requisitos no funcionales** → seguridad (auth/RBAC), rendimiento, escalabilidad, portabilidad
5. **Docker** → `Dockerfile`, `docker-compose.yml`, variables en `.env.example`, y este README
6. **Pruebas** → `tests/` (unitarias, integración, rendimiento, seguridad)
7. **Innovación** → modela el marco laboral chileno que el software global de STO no cubre

---

## Stack

| Capa | Tecnología |
|---|---|
| Motor / optimización | Python · OR-Tools |
| Datos | pandas · openpyxl |
| API | FastAPI |
| Base de datos | PostgreSQL |
| Interfaz | React |
| Despliegue | Docker Compose |

---

## Equipo

| Integrante | Frente (dueño) |
|---|---|
| Adrián Nájera Prado | Motor + API + Base de datos · coordinación |
| Andrés Gamboa | Datos (ingesta y normalización) |
| Rodrigo Llanquinado | Interfaz + Visualización + Docker |
