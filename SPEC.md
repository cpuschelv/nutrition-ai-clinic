# SPEC.md (MVP B) – Nutrición deportiva + IA con input definido

## Objetivo
Software clínico para:
- levantar anamnesis completa,
- registrar composición corporal,
- registrar entrenamiento (carga/horas/estímulo),
- analizar recordatorio 24 h automáticamente desde texto estructurado,
- permitir carga de documentos (antropometría, reportes, cargas),
- generar outputs: tablas + totales + flags + requerimientos + EA + export.

---

## 1) Entradas soportadas (Input)

### 1.1 Input manual (texto estructurado)

Lenguaje clínico definido (tokens):
- P proteína (1P = 50 g carne/ave/pescado; equivalencias huevo según tabla)
- C cereal/almidón (1C = 30 g CHO)
- F fruta (1F = 15 g CHO)
- G grasa/aceite (1G aceite = 5 g grasa)
- VL verduras libre consumo
- VG verduras consumo general
- L lácteo

Formato por línea:
TIEMPO: item + item + item

Ej:
ALMUERZO: 2C arroz + 2P pollo + VG + 2G aceite
CENA: 2C pan + 1P jamon/queso + palta + 2G aceite

Regla de desambiguación:
Si falta el alimento base (ej: “2C” sin especificar), el sistema pregunta con opciones cerradas:
- cereal: arroz/pasta/pan/papa/legumbre/otro
- proteína: pollo/vacuno/pescado/huevo/legumbre/lácteo/otro
- procesados: marca + porción (si se quiere precisión)

### 1.2 Input manual (formularios)
- Identificación
- Anamnesis clínica (patologías/medicamentos/antecedentes)
- Sueño (horas/calidad)
- Ocupación
- Entrenamiento (horas por día/semana + estímulo)
- Suplementación
- Antropometría (peso, adiposidad, masa adiposa, etc.)

### 1.3 Input por documentos (subida)
Tipos:
- PDF (antropometría, informes, evaluaciones)
- Imágenes (foto de evaluación, tablas, reportes)
- Excel/CSV (cargas, ingestas, listados)
- Export entrenamiento (si existiera: FIT/GPX/CSV)

Resultado esperado de documentos:
El sistema extrae:
- composición corporal (peso, %adiposidad, masa adiposa, etc.)
- carga entrenamiento (horas, TSS, distribución semanal, estímulo)
- recordatorio/ingesta si viene en planilla o informe

Regla crítica:
Todo dato extraído por documento queda con estado:
- EXTRAÍDO_SIN_CONFIRMAR
y requiere confirmación de 1 clic:
- CONFIRMADO_POR_NUTRICIONISTA

---

## 2) Prioridad de fuentes nutricionales
Orden:
1) INTA (base primaria composición)
2) UDD (solo equivalencias/porciones; no como composición final)
3) Etiquetado chileno / OpenFoodFacts (procesados)

Si se usa fallback (por no encontrar ítem), el sistema:
- marca INCERTIDUMBRE=ALTA
- solicita marca o foto de etiqueta

---

## 3) Motor de cálculo (Nutrition Engine)

### 3.1 Conversión porciones → gramos
- 1P = 50 g proteína animal estándar (según regla)
- 1C = 30 g CHO (y se convierte a gramos del alimento base)
- 1G aceite = 5 g grasa
- 1F = 15 g CHO

### 3.2 Cálculo por ítem (REAL)
Para cada alimento:
- g → macros/kcal vía INTA o etiquetado
Salida por ítem:
- kcal, CHO, Prot, Grasa

### 3.3 Agregación
- por tiempo de comida
- total diario
- g/kg (peso)

Flags clínicos MVP:
- CHO < 3,0 g/kg peso
- grasa < 0,5 g/kg peso
- proteína < 1,4 g/kg peso

---

## 4) Outputs obligatorios

### 4.1 Tabla “tipo Excel” (recordatorio 24h)
Por tiempo de comida:
- alimento
- gramos
- kcal
- CHO/Prot/Grasa
- fuente

### 4.2 Totales
- kcal totales
- CHO/Prot/Grasa
- g/kg

### 4.3 Export poblacional
export CSV/Sheets para análisis:
- recall_items
- daily_summary
- training_load
- anthropometry

---

## 5) Modelo de datos mínimo (DB)
Entidades:
- patients (id, nombre, sexo, fecha_nac, ocupación, deporte)
- sessions (id, patient_id, fecha, notas)
- anthropometry (session_id, peso, adiposidad, masa_adiposa, grasa_kg, mlg, etc.)
- training_week (session_id, horas_LV, horas_sab, horas_dom, TSS, estímulos)
- recall_items (session_id, tiempo, alimento, base, porciones, gramos, kcal, cho, prot, grasa, fuente, incertidumbre)
- daily_summary (session_id, kcal, cho, prot, grasa, cho_kg, prot_kg, grasa_kg)
- documents (session_id, tipo, archivo_url, extracción_json, estado_confirmación)
