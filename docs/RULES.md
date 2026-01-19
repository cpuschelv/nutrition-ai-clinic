# RULES (MVP B)

## Tokens
- P: proteína (1P = 50 g carne/ave/pescado)
- C: cereal/almidón (1C = 30 g CHO)
- F: fruta (1F = 15 g CHO)
- G: grasa/aceite (1G aceite = 5 g grasa)
- VL: verduras libre consumo
- VG: verduras consumo general
- L: lácteo

## Desambiguación
Si falta base:
- C: arroz/pasta/pan/papa/legumbre/otro
- P: pollo/vacuno/pescado/huevo/legumbre/lácteo/otro
Procesados:
- pedir marca + porción
- si no hay: incertidumbre=ALTA

## Cálculo REAL
- macros/kcal deben venir de INTA (o etiqueta)
- si no existe ítem: incertidumbre=ALTA y pedir etiqueta

## Flags clínicos MVP
- CHO < 3,0 g/kg peso
- grasa < 0,5 g/kg peso
- proteína < 1,4 g/kg peso

## Parser
Línea: TIEMPO: item + item
Ej: ALMUERZO: 2C arroz + 1P pollo + 1G aceite
