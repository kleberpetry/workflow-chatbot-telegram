# 🤖 Chatbot de Clima no Telegram com N8N

Este projeto consiste na criação de um chatbot no Telegram utilizando o N8N, capaz de informar a temperatura atual de qualquer cidade do Brasil a partir de uma mensagem enviada pelo usuário.

O chatbot recebe o nome da cidade no formato `Cidade,UF`, consulta a API pública do OpenWeather, processa a resposta e retorna uma mensagem curta, clara e amigável com a temperatura atual.

---

## 📌 Funcionalidades

- ✅ Recebe mensagens de texto no Telegram
- ✅ Normaliza a entrada do usuário (espaços, acentuação, letras minúsculas/maiúsculas)
- ✅ Consulta a API do OpenWeather via HTTP Request
- ✅ Retorna a temperatura em graus Celsius
- ✅ Trata erros de forma amigável quando a cidade não é encontrada
- ✅ Diferencia erros de API (404, 500, timeout)
- ✅ Validação de input vazio

---

## 🧩 Tecnologias Utilizadas

- **N8N** – Orquestração do workflow
- **Telegram Bot API** – Comunicação com o usuário
- **OpenWeather API** – Consulta de dados meteorológicos
- **PostgreSQL** – Banco de dados para persistência do N8N
- **Ngrok** – Túnel HTTPS para webhooks

---

## 📁 Estrutura do Repositório

```
/
├── workflow-chatbot-telegram.json   # Workflow exportado do N8N
├── docker-compose.yml               # Configuração Docker
├── .env.example                     # Exemplo de variáveis de ambiente
└── README.md                        # Documentação do projeto
```

---

## 🔐 Configuração das Credenciais

### 📋 Variáveis de Ambiente Necessárias

Crie um arquivo `.env` na raiz do projeto com as seguintes variáveis:

```env
# === PostgreSQL ===
POSTGRES_USER=n8n
POSTGRES_PASSWORD=sua_senha_segura_aqui
POSTGRES_DB=n8n

# === N8N Configuration ===
N8N_HOST=seu-dominio.ngrok.io
WEBHOOK_URL=https://seu-dominio.ngrok.io/
N8N_EDITOR_BASE_URL=https://seu-dominio.ngrok.io

# === APIs Externas ===
OPENWEATHER_API_KEY=sua_chave_openweather_aqui
TELEGRAM_BOT_TOKEN=seu_token_telegram_aqui

# === Ngrok ===
NGROK_AUTHTOKEN=seu_token_ngrok_aqui
```

### 1️⃣ Telegram Bot

1. No Telegram, procure por `@BotFather`
2. Envie o comando `/newbot`
3. Escolha um nome e um username (obrigatoriamente terminando em `bot`)
4. Copie o token gerado
5. Cole no arquivo `.env` na variável `TELEGRAM_BOT_TOKEN`

⚠️ **Importante**: Nunca suba o token no repositório público!

### 2️⃣ OpenWeather API

1. Crie uma conta em: https://home.openweathermap.org/users/sign_up
2. Acesse **API Keys** no painel
3. Copie sua chave de API
4. Cole no arquivo `.env` na variável `OPENWEATHER_API_KEY`

### 3️⃣ Ngrok (para webhooks)

1. Crie uma conta em: https://ngrok.com/
2. Acesse o dashboard e copie seu **Authtoken**
3. Cole no arquivo `.env` na variável `NGROK_AUTHTOKEN`
4. Após iniciar o Docker, acesse `http://localhost:4040` para ver o domínio público gerado
5. Atualize as variáveis `N8N_HOST`, `WEBHOOK_URL` e `N8N_EDITOR_BASE_URL` com o domínio do Ngrok

---

## 🚀 Como Executar o Projeto

### Passo 1: Clone o repositório

```bash
git clone <seu-repositorio>
cd <nome-do-projeto>
```

### Passo 2: Configure as variáveis de ambiente

```bash
cp .env.example .env
# Edite o arquivo .env com suas credenciais
```

### Passo 3: Inicie os containers

```bash
docker-compose up -d
```

### Passo 4: Acesse o N8N

1. Abra o Ngrok em `http://localhost:4040` e copie o domínio HTTPS
2. Atualize o arquivo `.env` com o domínio do Ngrok
3. Reinicie os containers: `docker-compose restart`
4. Acesse o N8N em: `https://seu-dominio.ngrok.io`

### Passo 5: Importe o Workflow

1. No N8N, clique em **Import** ou **Import workflow**
2. Selecione o arquivo `workflow-chatbot-telegram.json`
3. Configure as credenciais do Telegram:
   - Vá em **Credentials**
   - Crie uma credencial do tipo **Telegram API**
   - Cole o token do bot (variável `TELEGRAM_BOT_TOKEN`)
4. Ative o workflow

### Passo 6: Teste o Bot

1. Abra o bot no Telegram
2. Envie uma mensagem no formato: `Cidade,UF`

---

## ✅ Exemplos de Uso

### Mensagens Válidas

```
São Paulo,SP
Belo Horizonte,MG
Curitiba,PR
Rio de Janeiro,RJ
```

### Resposta Esperada (Sucesso)

```
🌤️ A temperatura em São Paulo é de 25°C.
```

### Resposta Esperada (Erro - Cidade não encontrada)

```
❌ Cidade não encontrada. Use o formato Cidade,UF (ex.: São Paulo,SP).
```

### Resposta Esperada (Erro - Problema na API)

```
⚠️ Erro ao consultar a previsão do tempo. Tente novamente mais tarde.
```

---

## 🔍 Validações Implementadas

- ✅ Normalização de acentos e espaços extras
- ✅ Conversão para formato `cidade,UF`
- ✅ Validação de resposta da API (código 200)
- ✅ Tratamento de erros HTTP (timeout, 404, 500)
- ✅ Mensagens diferenciadas por tipo de erro
- ✅ Input vazio ou inválido

---

## 🛠️ Troubleshooting

### O bot não responde

1. Verifique se o workflow está **ativado** no N8N
2. Confirme se o Ngrok está rodando e o domínio está atualizado
3. Teste a credencial do Telegram no N8N
4. Verifique os logs do container: `docker-compose logs -f n8n`

### Erro "Cidade não encontrada" para cidades válidas

1. Confirme o formato: `Cidade,UF` (com vírgula e sigla do estado)
2. Verifique se a API Key do OpenWeather está correta
3. Teste a API manualmente: 
   ```
   https://api.openweathermap.org/data/2.5/weather?q=SaoPaulo,SP&appid=SUA_CHAVE
   ```

### Ngrok desconectando

O plano gratuito do Ngrok expira após 2 horas. Para produção, considere:
- Plano pago do Ngrok com domínio fixo
- Hospedagem com IP público (AWS, DigitalOcean, etc.)

---

## 📊 Arquitetura do Workflow

```
[Telegram Trigger] 
    ↓
[Normalizar Input] 
    ↓
[HTTP Request OpenWeather] 
    ↓ (sucesso)          ↓ (erro)
[Validar Resposta]    [Mensagem de Erro]
    ↓ (200 OK)  ↓ (falha)
[Formatar Temp] [Msg Erro]
    ↓               ↓
[Enviar Telegram] [Enviar Telegram]
```

---

## 📝 Observações Finais

- ✅ Este repositório é público para permitir avaliação automática
- ✅ Nenhuma credencial sensível está incluída nos arquivos
- ✅ O workflow pode ser executado localmente ou na versão cloud do N8N
- ✅ Todas as variáveis de ambiente estão documentadas
- ✅ Tratamento robusto de erros implementado

---

## 🚀 Próximos Passos (Melhorias Futuras)

- [ ] Adicionar histórico de buscas no PostgreSQL
- [ ] Implementar cache de respostas para reduzir chamadas à API
- [ ] Enviar imagens/stickers baseados no clima
- [ ] Suporte a previsão do tempo para os próximos dias
- [ ] Comando `/help` com instruções
- [ ] Internacionalização (outros países além do Brasil)

---

## 📄 Licença

Este projeto é de código aberto para fins educacionais.


