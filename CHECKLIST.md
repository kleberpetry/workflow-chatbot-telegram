# ✅ Checklist de Validação - Chatbot Telegram

Use este checklist para garantir que todos os requisitos foram atendidos antes do envio.

---

## 📋 Etapa 1: Segurança e Configuração Inicial

- [ ] Repositório público criado
- [ ] `.gitignore` configurado para proteger `.env`
- [ ] Workflow JSON válido e importável
- [ ] `.env.example` documentado com todas as variáveis
- [ ] README completo com instruções claras
- [ ] Variável `OPENWEATHER_API_KEY` documentada
- [ ] Variável `TELEGRAM_BOT_TOKEN` documentada
- [ ] Nenhuma credencial exposta no código

---

## 📋 Etapa 2: Tratamento de Dados (Input)

- [ ] Input normalizado (acentos, espaços, maiúsculas/minúsculas)
- [ ] Validação de input vazio implementada
- [ ] Formato `Cidade,UF` padronizado
- [ ] Tratamento de caracteres especiais
- [ ] Mensagem de erro para input inválido

---

## 📋 Etapa 3: Integração e Lógica

- [ ] HTTP Request configurado corretamente
- [ ] Parâmetros da API (units=metric, lang=pt_br)
- [ ] Variável de ambiente usada para API Key
- [ ] Timeout configurado (10 segundos)
- [ ] `continueOnFail: true` no nó HTTP
- [ ] Nó IF validando `cod === 200`
- [ ] Conexão de erro do HTTP tratada
- [ ] Saída de sucesso conectada
- [ ] Saída de erro conectada

---

## 📋 Etapa 4: Resposta ao Usuário e UX

- [ ] Mensagem de sucesso formatada com emoji
- [ ] Temperatura arredondada (`Math.round()`)
- [ ] Mensagem de erro amigável (cidade não encontrada)
- [ ] Mensagem de erro para API indisponível
- [ ] Mensagem de erro para input vazio
- [ ] Chat ID capturado corretamente
- [ ] Credencial do Telegram configurada
- [ ] Webhook ativo no bot

---

## 🧪 Testes Obrigatórios

### Teste 1: Cidade Válida
```
Input: São Paulo,SP
Esperado: 🌤️ A temperatura em São Paulo é de XX°C.
```

### Teste 2: Cidade Inválida
```
Input: CidadeInexistente,XX
Esperado: ❌ Cidade não encontrada. Use o formato Cidade,UF (ex.: São Paulo,SP).
```

### Teste 3: Input Vazio
```
Input: (mensagem vazia ou espaços)
Esperado: ⚠️ Por favor, envie o nome de uma cidade no formato: Cidade,UF
```

### Teste 4: Formato Incorreto
```
Input: São Paulo (sem UF)
Esperado: ❌ Cidade não encontrada. Use o formato Cidade,UF (ex.: São Paulo,SP).
```

### Teste 5: Acentuação
```
Input: Brasília,DF
Esperado: Funcionar normalmente
```

### Teste 6: Espaços Extras
```
Input: Rio de Janeiro , RJ (espaços antes/depois da vírgula)
Esperado: Funcionar normalmente
```

---

## 🔍 Verificação Final

### Workflow
- [ ] Nó "Telegram Trigger" está no início
- [ ] Nó "Normalizar Input" processa a mensagem
- [ ] Nó "Validar Input" verifica se não está vazio
- [ ] Nó "HTTP Request" consulta a API
- [ ] Nó "Validar Resposta API" checa código 200
- [ ] Nós de mensagem (sucesso, erro, vazio) estão conectados
- [ ] Nós "Enviar Mensagem" estão configurados
- [ ] Workflow ativado (`"active": true`)

### Docker
- [ ] `docker-compose.yml` presente
- [ ] Serviço N8N configurado
- [ ] Serviço PostgreSQL configurado
- [ ] Serviço Ngrok configurado
- [ ] Portas corretas (5678, 5432, 4040)
- [ ] Volumes persistentes configurados

### Credenciais
- [ ] Token do Telegram válido e testado
- [ ] API Key do OpenWeather válida e testada
- [ ] Authtoken do Ngrok configurado
- [ ] Ngrok rodando e domínio atualizado no `.env`
- [ ] Credenciais configuradas no N8N

---

## 🚀 Comandos de Teste Rápido

```bash
# 1. Verificar se containers estão rodando
docker-compose ps

# 2. Ver logs do N8N
docker-compose logs -f n8n

# 3. Ver domínio do Ngrok
curl http://localhost:4040/api/tunnels | jq '.tunnels[0].public_url'

# 4. Testar API OpenWeather manualmente
curl "https://api.openweathermap.org/data/2.5/weather?q=SaoPaulo,SP&units=metric&appid=SUA_CHAVE"

# 5. Reiniciar serviços após atualizar .env
docker-compose restart
```

---

## 📊 Pontuação Estimada

| Critério | Pontos | Status |
|----------|--------|--------|
| Segurança e Config | 12.5 | ⬜ |
| Tratamento de Input | 12.5 | ⬜ |
| Integração e Lógica | 15.0 | ⬜ |
| Resposta e UX | 10.0 | ⬜ |
| **TOTAL** | **50** | **⬜** |

---

## ⚠️ Problemas Comuns e Soluções

### Problema: Bot não responde
**Solução**: 
1. Verifique se o workflow está ativo
2. Teste a credencial do Telegram
3. Confirme se o Ngrok está rodando
4. Veja os logs: `docker-compose logs -f n8n`

### Problema: "Cidade não encontrada" para cidades válidas
**Solução**: 
1. Confirme o formato `Cidade,UF`
2. Teste a API manualmente
3. Verifique a API Key do OpenWeather

### Problema: Workflow não valida corretamente
**Solução**: 
1. Revise a condição do nó IF
2. Use `cod === "200"` ou `cod === 200`
3. Ative `continueOnFail` no HTTP Request

---

## 📝 Antes de Enviar

- [ ] Todos os testes passaram
- [ ] README completo e claro
- [ ] `.env.example` atualizado
- [ ] `.gitignore` protegendo credenciais
- [ ] Workflow testado no N8N
- [ ] Commit com mensagem descritiva
- [ ] Push para o repositório
- [ ] Link do repositório pronto para envio

---

**Última revisão**: {{ DATA_ATUAL }}
**Status**: ⬜ Pendente | ✅ Aprovado
