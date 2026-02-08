# 🤖 Chatbot de Clima no Telegram com N8N

Este projeto consiste na criação de um **chatbot no Telegram** utilizando o **N8N**, capaz de informar a **temperatura atual de qualquer cidade do Brasil** a partir de uma mensagem enviada pelo usuário.

O chatbot recebe o nome da cidade no formato `Cidade,UF`, consulta a API pública do **OpenWeather**, processa a resposta e retorna uma mensagem curta, clara e amigável com a temperatura atual.

---

## 📌 Funcionalidades

* Recebe mensagens de texto no Telegram
* Normaliza a entrada do usuário (espaços, acentuação, letras minúsculas/maiúsculas)
* Consulta a API do OpenWeather via **HTTP Request**
* Retorna a temperatura em **graus Celsius**
* Trata erros de forma amigável quando a cidade não é encontrada

---

## 🧩 Tecnologias Utilizadas

* **N8N** – Orquestração do workflow
* **Telegram Bot API** – Comunicação com o usuário
* **OpenWeather API** – Consulta de dados meteorológicos

---

## 📁 Estrutura do Repositório

```text
/
├── workflow-chatbot-telegram.json   # Workflow exportado do N8N
├── README.md                        # Documentação do projeto
└── (opcional) docker-compose.yml    # Para execução local do N8N via Docker
```

---

## 🚀 Como Importar o Workflow no N8N

1. Abra o **N8N** (local ou cloud).
2. Clique em **Import** ou **Import workflow**.
3. Selecione o arquivo `workflow-chatbot-telegram.json`.
4. Após a importação, o workflow aparecerá na sua lista.

⚠️ **Importante:** o workflow não contém credenciais. Elas devem ser configuradas manualmente após a importação.

---

## 🔐 Configuração das Credenciais

### 1️⃣ Telegram Bot

1. No Telegram, procure por **@BotFather**.
2. Envie o comando `/newbot`.
3. Escolha um nome e um username (obrigatoriamente terminando em `bot`).
4. Copie o **token** gerado.

No N8N:

* Vá em **Credentials**
* Crie uma nova credencial do tipo **Telegram API**
* Cole o token do bot

> ⚠️ Nunca suba o token no repositório.

---

### 2️⃣ OpenWeather API

1. Crie uma conta em: [https://home.openweathermap.org/users/sign_up](https://home.openweathermap.org/users/sign_up)
2. Acesse **API Keys** no painel.
3. Copie sua chave de API.

Configure a variável de ambiente:

```bash
OPENWEATHER_API_KEY=SUACHAVEAQUI
```

No N8N:

* Vá em **Settings → Environment Variables**
* Adicione a variável `OPENWEATHER_API_KEY`

---

## ▶️ Como Executar o Chatbot

1. Ative o workflow no N8N.
2. Abra o bot no Telegram.
3. Envie uma mensagem no formato:

```text
Cidade,UF
```

### Exemplos válidos

```text
São Paulo,SP
Belo Horizonte,MG
Curitiba,PR
```

### Resposta esperada

```text
🌤️ A temperatura em São Paulo é de 25°C.
```

---

## ❌ Tratamento de Erros

Se a cidade não for encontrada ou o formato estiver incorreto, o bot retornará:

```text
❌ Cidade não encontrada. Use o formato Cidade,UF (ex.: São Paulo,SP).
```

---

## ✅ Checklist de Validação

* [x] Workflow inicia com Telegram Trigger
* [x] Entrada do usuário normalizada
* [x] HTTP Request com parâmetros corretos
* [x] Uso de variável de ambiente para a API Key
* [x] Validação da resposta da API
* [x] Mensagens amigáveis
* [x] Credenciais fora do JSON e do repositório

---

## 📝 Observações Finais

* Este repositório é **público** para permitir avaliação automática.
* Nenhuma credencial sensível está incluída nos arquivos.
* O workflow pode ser executado localmente ou na versão cloud do N8N.
* Caso utilize Docker, sinta-se à vontade para incluir um `docker-compose.yml`.
