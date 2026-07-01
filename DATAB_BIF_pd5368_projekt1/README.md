# DATAB Projekt - pd5368

## Wyniki zapytań SQL

### Zapytanie 4ii – Testy, w których uczestniczyło więcej niż 30 pacjentów

**Zapytanie**

```sql
SELECT
    tests.test_type,
    COUNT(patient_test.patient_id) AS num_patients
FROM tests
INNER JOIN patient_test
ON tests.test_id = patient_test.test_id
GROUP BY tests.test_type
HAVING COUNT(patient_test.patient_id) > 30;
```

**Wynik**

| test_type | num_patients |
|-----------|-------------:|
| SNP Array | 49 |
| NGS-panel | 43 |

---

### Zapytanie 4iii – Średnia liczba wariantów dla każdego testu

**Zapytanie**

```sql
SELECT
    t.test_type,
    AVG(t.variant_count) AS avg_variants
FROM (
    SELECT
        tests.test_id,
        tests.test_type,
        COUNT(results.result_id) AS variant_count
    FROM tests
    INNER JOIN results
        ON tests.test_id = results.test_id
    GROUP BY tests.test_id, tests.test_type
) AS t
GROUP BY t.test_type;
```

**Wynik**

| test_type | avg_variants |
|-----------|-------------:|
| SNP Array | 3.0000 |
| NGS-panel | 3.6857 |
| WES | 3.5294 |
| WGS | 3.6071 |
