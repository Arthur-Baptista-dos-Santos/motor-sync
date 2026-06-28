[![Live Demo](https://img.shields.io/badge/Live%20Demo-abs--motor--sync.streamlit.app-FF4B4B?logo=streamlit&logoColor=white)](https://abs-motor-sync.streamlit.app)

[![Live Demo](https://img.shields.io/badge/Live%20Demo-abs--motor--sync.streamlit.app-FF4B4B?logo=streamlit&logoColor=white)](https://abs-motor-sync.streamlit.app)

# `MotorSync: Monitor de Ativos Industriais`

> Dashboard Streamlit para cadastro tecnico, consulta e visualizacao de dados de motores e equipamentos industriais. Arquitetura desacoplada UI/backend/storage, dark theme industrial, conversao de leituras ADC para unidades fisicas. Sprint 1 do projeto Forzy, FIAP 2026.

---

## `Tecnologias`

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![Streamlit](https://img.shields.io/badge/Streamlit-1.35+-ff4b4b)
![Pandas](https://img.shields.io/badge/pandas-2.0+-green)
![JSON](https://img.shields.io/badge/storage-JSON-lightgrey)
![License](https://img.shields.io/badge/license-MIT-green)

---

## `O que faz`

Sistema web de monitoramento de ativos industriais com 3 paginas:

- **Painel Principal**: KPIs gerais (total, ativos, alertas, criticos), lista de equipamentos com status e metricas em tempo real
- **Equipamentos**: consulta e ficha tecnica completa de cada ativo com conversao de unidades de sensor
- **Cadastro**: formulario de cadastro e edicao de equipamentos
- **Dados Brutos**: visualizacao de leituras de sensores com conversao ADC → unidades fisicas

---

## `Arquitetura`

```
Streamlit UI (pages/)
    |
    EquipmentService (backend/services.py)     logica de negocio, validacoes
        |
        Storage (backend/storage.py)           persistencia JSON (swappable por DB)
        |
        Models (backend/models.py)             dataclasses puras, zero dependencia de UI
        |
        UnitConverter (utils/unit_converter.py) funcoes puras de conversao ADC -> fisica
        |
    UIHelpers (utils/ui_helpers.py)            componentes HTML/CSS reutilizaveis
```

Decisao de design: `backend/` e completamente independente do Streamlit. Trocar JSON por SQLite ou Postgres requer apenas editar `storage.py`. Adicionar um modelo ML no Sprint 2 requer apenas adicionar `ml/predictor.py` e chamar de `services.py`.

---

## `Conversao de Unidades (ADC 12-bit)`

| Sensor | Hardware | Formula | Saida |
|--------|---------|---------|-------|
| Tensao | ZMPT101B | `raw / 4095 x 500` | V (CA) |
| Corrente | ACS712-30A | `raw / 4095 x 30` | A |
| Temperatura | NTC Thermistor | `-20 + (raw / 4095) x 170` | C |
| Vibracao | ADXL345 | `raw / 4095 x 16` | g |
| Velocidade | Hall Effect | `Hz x 60 / PPR` | RPM |
| Potencia | Derivada | `V x I x FP x sqrt(3) / 1000` | kW |

---

## `Design`

- **Tema**: Industrial Dark, fundo `#0d1117`, acento ambar `#f97316`
- **Cores semanticas**: Verde `#22c55e` (OK) · Ambar `#f59e0b` (alerta) · Vermelho `#ef4444` (critico)
- **Tipografia**: Barlow Condensed (display) + Barlow (UI) + JetBrains Mono (dados)
- **Human-in-the-loop**: acoes destrutivas separadas, status editavel pelo operador, dados brutos preservados

---

## `Estrutura`

```
motor-sync/
├── app.py                          entrada principal (Painel/Dashboard)
├── pages/
│   ├── 1_Equipamentos.py           consulta e ficha tecnica
│   ├── 2_Cadastro.py               formulario de cadastro/edicao
│   └── 3_Dados_Brutos.py           leituras de sensores com conversao
├── backend/
│   ├── models.py                   dataclasses puras (Motor, Reading, Status)
│   ├── storage.py                  persistencia JSON (swappable por DB)
│   └── services.py                 logica de negocio e validacoes
├── utils/
│   ├── unit_converter.py           conversao ADC -> unidades fisicas
│   └── ui_helpers.py               componentes HTML/CSS reutilizaveis
├── assets/
│   └── style.css                   tema dark industrial
├── data/
│   ├── equipment.json              dados de equipamentos (auto-criado)
│   └── readings.json               historico de leituras (auto-criado)
├── seed_data.py                    popula dados de demonstracao
└── requirements.txt
```

---

## `Instalacao`

```bash
git clone https://github.com/Arthur-Baptista-dos-Santos/motor-sync.git
cd motor-sync

python -m venv .venv
.venv\Scripts\activate       # Windows
# source .venv/bin/activate  # Linux/Mac

pip install -r requirements.txt

# (opcional) popula dados de demonstracao
python seed_data.py

streamlit run app.py
```

Acesse `http://localhost:8501`.

---

## `Roadmap Forzy`

| Sprint | Foco | Repo |
|--------|------|------|
| Sprint 1 (este) | Cadastro e visualizacao de ativos | `motor-sync` |
| Sprint 2 | Digital Twin: modelo de anomalias + predicao | `digital-twin-assets` |

---

## `Conceitos aplicados`

- **`Streamlit multipage`**: 3 paginas independentes com estado compartilhado via `st.session_state`
- **`Dataclasses`**: modelos de dados tipados e imutaveis, sem dependencia de framework
- **`Repository pattern`**: `storage.py` abstrai a persistencia, permitindo troca de backend sem tocar na UI
- **`Service layer`**: `services.py` concentra toda a logica de negocio, pages sao apenas apresentacao
- **`CSS customizado`**: `assets/style.css` injetado via `st.markdown` para dark theme industrial

---

## `Licenca`

Distribuido sob a licenca MIT. Veja [LICENSE](LICENSE) para mais informacoes.

---

## `Autor`

**Arthur Baptista dos Santos**
RM 565346 · Inteligencia Artificial · FIAP 2025-2026

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Arthur%20Baptista-0077B5?logo=linkedin)](https://linkedin.com/in/arthur-baptista-dos-santos)
[![GitHub](https://img.shields.io/badge/GitHub-Arthur--Baptista--dos--Santos-181717?logo=github)](https://github.com/Arthur-Baptista-dos-Santos)
