# DATA_MODEL (MVP B)

## Entidades mínimas

### patients
- id (PK)
- nombre
- sexo (M/F/X)
- fecha_nac (nullable)
- ocupacion (nullable)
- deporte (nullable)

### sessions
- id (PK)
- patient_id (FK)
- fecha
- notas (nullable)
- status (OPEN/CLOSED)

### anthropometry
- session_id (UNIQUE)
- peso_kg
- adiposidad_pct (nullable)
- masa_adiposa_kg (nullable)
- grasa_kg (nullable)
- mlg_kg (nullable)

### training_week
- session_id (UNIQUE)
- horas_lv (nullable)
- horas_sab (nullable)
- horas_dom (nullable)
- tss (nullable)
- estimulo (nullable)

### recall_items
- session_id
- tiempo
- alimento_raw
- token (nullable)
- base (nullable)
- porciones (nullable)
- gramos (nullable)
- kcal (nullable)
- cho_g (nullable)
- prot_g (nullable)
- grasa_g (nullable)
- fuente (nullable)
- incertidumbre (BAJA/MEDIA/ALTA)
- needs_clarification (bool)
- clarification_type (nullable)

### daily_summary
- session_id (UNIQUE)
- kcal
- cho_g
- prot_g
- grasa_g
- cho_gkg
- prot_gkg
- grasa_gkg
- flags_json

### documents
- session_id
- tipo (PDF/IMAGE/CSV/XLSX/FIT/GPX/OTHER)
- archivo_url
- extraccion_json (nullable)
- estado_confirmacion (EXTRAÍDO_SIN_CONFIRMAR/CONFIRMADO_POR_NUTRICIONISTA)
