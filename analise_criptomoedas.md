# Análise para Implementação de Criptomoedas

## Documentação da API BRAPI para Criptomoedas

### Endpoint Principal
- **URL**: `GET /api/v2/crypto`
- **Parâmetros obrigatórios**: 
  - `coin`: Uma ou mais criptomoedas separadas por vírgula (ex: BTC,ETH)
  - `token`: Token de autenticação
- **Parâmetros opcionais**:
  - `currency`: Moeda de retorno (padrão: BRL)
  - `range`: Período histórico (1d, 5d, 1mo, 3mo, 6mo, 1y, 2y, 5y, 10y, ytd, max)
  - `interval`: Intervalo entre dados (1m, 2m, 5m, 15m, 30m, 60m, 90m, 1h, 1d, 5d, 1wk, 1mo, 3mo)

### Endpoint de Criptomoedas Disponíveis
- **URL**: `GET /api/v2/crypto/available`
- **Retorna**: `{ coins: [...] }` - Lista de criptomoedas disponíveis

### Formato da Resposta
```json
{
  "coins": [
    {
      "currency": "USD",
      "currencyRateFromUSD": 1,
      "coinName": "Solana USD",
      "coin": "SOL",
      "regularMarketChange": -16.79895,
      "regularMarketPrice": 199.2073,
      "regularMarketChangePercent": -7.777063,
      "regularMarketDayLow": 196.79688,
      "regularMarketDayHigh": 208.90076,
      "regularMarketDayRange": "196.79688 - 208.90076",
      "regularMarketVolume": 5044000256,
      "marketCap": 97023246336,
      "regularMarketTime": "2025-02-05T19:44:00.000Z",
      "coinImageUrl": "https://s2.coinmarketcap.com/static/img/coins/64x64/5426.png",
      "usedInterval": "1d",
      "usedRange": "5d",
      "historicalDataPrice": [...]
    }
  ]
}
```

## Análise do Código Atual

### DynamicFinanceCard.tsx
**Funcionalidades atuais**:
- ✅ Já busca lista de criptomoedas disponíveis via `/api/v2/crypto/available`
- ✅ Detecta tickers de criptomoedas no texto
- ✅ Cria Set de criptomoedas para verificação rápida
- ✅ Renderiza FinanceCard com flag `isCrypto`

**Problemas identificados**:
- ❌ Padrão de detecção muito restritivo para criptomoedas
- ❌ Lista hardcoded de criptos pode estar desatualizada
- ⚠️ Logs excessivos podem impactar performance

### FinanceCard.tsx
**Funcionalidades atuais**:
- ✅ Suporte básico para criptomoedas com flag `isCrypto`
- ✅ Endpoint correto para buscar cotação: `/api/v2/crypto?coin={symbol}`
- ✅ Mapeamento de campos da resposta da API
- ✅ Exibição de logo via `coinImageUrl`

**Problemas identificados**:
- ❌ Não exibe dados históricos para criptomoedas (só para ações)
- ❌ Botão "Análises no Finver" pode não ser relevante para criptos
- ⚠️ Tratamento de erros pode ser melhorado

## Melhorias Propostas

### 1. DynamicFinanceCard.tsx
- Melhorar padrões de detecção de criptomoedas
- Otimizar logs de debug
- Adicionar fallback para criptomoedas populares

### 2. FinanceCard.tsx
- Implementar dados históricos para criptomoedas
- Personalizar ações específicas para criptos
- Melhorar tratamento de erros
- Adicionar informações específicas de criptos (market cap, volume)

### 3. Novos Recursos
- Gráfico histórico para criptomoedas
- Links para exchanges ou análises específicas de crypto
- Informações adicionais como market cap e volume

