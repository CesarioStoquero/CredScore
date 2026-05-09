# Sistema Inteligente de Análise de Empréstimos com IA

Uma aplicação web full-stack desenvolvida em Django que integra Machine Learning para auxiliar na análise de aprovação de empréstimos. O sistema analisa os dados financeiros do solicitante e prevê se uma solicitação de empréstimo deve ser aprovada ou rejeitada, junto com a probabilidade de aprovação.

O mecanismo de predição utiliza um modelo de Regressão Logística treinado com mais de 975.000 registros financeiros sintéticos e realistas da Índia, permitindo uma tomada de decisão baseada em dados, semelhante a sistemas de avaliação de risco utilizados em ambientes bancários.

## Funcionalidades

- **Preditor de Aprovação de Empréstimo** — Informe seu perfil financeiro e receba em tempo real a probabilidade de aprovação, com feedback visual animado.
- **Análise dos Principais Fatores** — Veja como score de crédito, índice DTI, garantia e empréstimos existentes influenciam suas chances.
- **Dicas de Melhoria** — Receba sugestões personalizadas para aumentar as chances de aprovação.
- **Calculadora EMI** — Calcule a parcela mensal do empréstimo usando três métodos de juros: saldo devedor, taxa fixa e juros compostos.
- **Tabela de Amortização** — Visualize o detalhamento anual do pagamento do empréstimo com gráfico de rosca.
- **Calculadora Automática de DTI** — Calcula automaticamente a relação dívida/renda a partir da renda mensal e das dívidas informadas.

---

## Tecnologias Utilizadas

| Camada                 | Tecnologia                         |
| ---------------------- | ---------------------------------- |
| Backend                | Python, Django                     |
| Modelo de ML           | Scikit-learn (Logistic Regression) |
| Processamento de Dados | Pandas, NumPy                      |
| Frontend               | HTML, CSS, JavaScript puro         |
| Estilização            | Tema escuro customizado            |
| Persistência do Modelo | Joblib                             |

---

## Modelo de Machine Learning

- **Algoritmo** — Regressão Logística
- **Dados de Treinamento** — 975.800 linhas de dados sintéticos de empréstimos indianos
- **Acurácia** — 83,1%
- **Precisão** — 74,8%
- **Recall** — 64,6%
- **F1 Score** — 69,3%

### Variáveis Utilizadas (27 no total)

- Renda do solicitante, renda do co-solicitante, idade e dependentes
- Score de crédito ao quadrado, índice DTI ao quadrado e razão de garantia
- Poupança, valor do empréstimo, prazo do empréstimo e empréstimos existentes
- Situação de emprego, categoria do empregador e nível de escolaridade
- Estado civil, gênero, finalidade do empréstimo e área da propriedade

### Principais Correlações com a Aprovação

| Variável               | Correlação |
| ---------------------- | ---------- |
| Score de Crédito²      | +0.38      |
| Emprego Assalariado    | +0.27      |
| Razão de Garantia      | +0.16      |
| Índice DTI²            | -0.31      |
| Empréstimos Existentes | -0.19      |
| Valor do Empréstimo    | -0.13      |

---

## Estrutura do Projeto

```txt
├── ML/
│   ├── Data/
│   │   ├── loan_approval_data.csv
│   │   └── Processed_loan_approval_data.csv
│   └── Train and Test Model/
│       └── final_model.py
├── model/
│   ├── loan_model.pkl
│   └── scaler.pkl
└── web/
    ├── manage.py
    ├── settings.py
    ├── urls.py
    ├── views.py
    ├── wsgi.py
    ├── templates/
    │   └── index.html
    └── static/
        ├── style.css
        └── script.js
```

---

## Instalação e Configuração

### Pré-requisitos

- Python 3.8+
- Anaconda (recomendado)

### Passos

**1. Clone o repositório**

```bash
git clone https://github.com/yourusername/CredScore.git
```

**2. Instale as dependências**

```bash
pip install django scikit-learn pandas numpy joblib whitenoise gunicorn
```

**3. Execute o servidor de desenvolvimento**

```bash
cd web
python manage.py runserver
```

**4. Abra no navegador**

```txt
http://127.0.0.1:8000
```

---

## Retreinamento do Modelo

Caso queira retreinar o modelo com novos dados:

```bash
# Passo 1 — Execute o notebook de pré-processamento
# Abra ML/data_Preprocessing.ipynb e execute todas as células
# Isso irá gerar o arquivo ML/Data/Processed_loan_approval_data.csv

# Passo 2 — Treine o modelo
python "ML/Train and Test Model/final_model.py"

# Passo 3 — Reinicie o servidor
cd web
python manage.py runserver
```

---

## Intervalos de Entrada

O modelo aceita valores reais em rúpias indianas:

| Campo                | Intervalo                |
| -------------------- | ------------------------ |
| Renda mensal         | ₹10,000 – ₹1,00,00,000   |
| Valor do empréstimo  | ₹10,000 – ₹50,00,00,000  |
| Poupança             | ₹1,000 – ₹10,00,00,000   |
| Valor da garantia    | ₹0 – ₹1,00,00,00,000     |
| Score de crédito     | 300 – 900                |
| Índice DTI           | 0.05 – 0.90              |

---

## 📊 Dataset

O dataset utilizado para treinar este modelo está disponível publicamente no Kaggle:

🔗 **[Loan Approval Dataset — Kaggle](https://www.kaggle.com/datasets/mayankchouhan263/loan-approval-datasetrealistic-indian-rupee-data)**

- 1.000.000 de solicitações sintéticas de empréstimos indianos
- Intervalos realistas em rúpias indianas, como renda entre ₹10,000 e ₹1,00,00,000
- 13 features, incluindo score de crédito, índice DTI, garantia e situação de emprego
- Target: `Loan_Approved` (1 = Aprovado, 0 = Rejeitado)

---

## Deployment

Este projeto está configurado para deployment no **Render**.

### Variáveis de Ambiente

| Chave           | Valor                        |
| --------------- | ---------------------------- |
| `SECRET_KEY`    | Sua chave secreta            |
| `DEBUG`         | `False`                      |
| `ALLOWED_HOSTS` | `your-app-name.onrender.com` |

### Build Command

```bash
pip install -r requirements.txt && cd web && python manage.py collectstatic --noinput
```

### Start Command

```bash
cd web && gunicorn wsgi:application --bind 0.0.0.0:$PORT
```

---

## Autor

**Mayank** — Desenvolvido como um projeto full-stack de Machine Learning, combinando ciência de dados, backend em Django e design de interface frontend.

---

## Licença

Este projeto é destinado a fins educacionais.