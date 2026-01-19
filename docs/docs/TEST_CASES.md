# TEST_CASES (MVP B)

## Caso 01: Jaime
- Peso: 75 kg

Recordatorio:
DESAYUNO: 1C pan + 1P huevo + 1G aceite
ALMUERZO: 2C arroz + 1P pollo + VG + 1G aceite
CENA: 1C papa + 1P pescado + VL + 1G aceite
COLACION: 1F platano

Expected:
- cho_gkg < 3.0  -> CHO_LOW true
- grasa_gkg < 0.5 -> FAT_LOW true
- prot_gkg < 1.4 -> PROT_LOW true
