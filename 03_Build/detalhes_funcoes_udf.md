# Funções DAX UDF

---

## 1. variacao_percentual

Calcula a variação percentual entre dois valores (atual vs anterior).

### Código
```DAX
FUNCTION variacao_percentual = (
    atual    : NUMERIC,
    anterior : NUMERIC
) =>
    IF(
        ISBLANK(anterior) || anterior = 0,
        BLANK(),
        DIVIDE(atual - anterior, anterior)
    )
```

---

## formatar_numero

Formata valores numéricos em escala monetária.

### Código
```DAX
FUNCTION formatar_numero = (
    valor : NUMERIC
) =>
    VAR vFormatoBase = "R$ #,0"
    VAR vCasaDecimal = "00"

    RETURN
    SWITCH(
        TRUE(),
        valor < 1000, vFormatoBase & "." & vCasaDecimal,
        valor <1000000, vFormatoBase & ",." & vCasaDecimal & " K",
        valor <100000000, vFormatoBase & ",,." & vCasaDecimal & " MI",
        vFormatoBase & ",,,." & vCasaDecimal & " BI"
    )
```

---

## status_meta

Classifica o desempenho com base no percentual da meta.

### Código
```DAX
FUNCTION status_meta = (
    pct : NUMERIC
) =>
    SWITCH(TRUE(),
        ISBLANK(pct),  "Sem meta",
        pct >= 1,      "Sucesso",
        pct >= 0.85,   "Em risco",
        "Abaixo da Meta"
    )
```

---

## seta_delta_formatada

Adiciona indicador visual de seta.

### Código
```DAX
FUNCTION seta_delta_formatada = (
    delta : NUMERIC
) =>
    IF(
        ISBLANK(delta), BLANK(),
        IF(delta >= 0, "▲ ", "▼ ")
    )
```

---

## categorizacao_prazo

Classifica prazos por categoria.

### Código
```DAX
FUNCTION categorizacao_prazo = (
    dias_delta : NUMERIC
) =>
    SWITCH(TRUE(),
        ISBLANK(dias_delta),  BLANK(),
        dias_delta < 0,      "Antecipado",
        dias_delta = 0,      "No prazo",
        dias_delta <= 3,     "Atraso leve",
        dias_delta <= 7,     "Atraso moderado",
        "Atraso crítico"
    )
```

---

## titulo_com_data

Gera as datas min e max e pode ser concatenada com os titulos.

### 💻 Código
```DAX
FUNCTION titulo_com_data = (
    dt_min : DATETIME,
    dt_max : DATETIME
) =>
    VAR _ini = FORMAT(dt_min, "MMM/YYYY")
    VAR _fim = FORMAT(dt_max, "MMM/YYYY")
    RETURN
    IF(
        _ini = _fim,
        _ini,
        _ini & " – " & _fim
    )
```
