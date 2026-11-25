# FinTrack

Aplicativo mobile de gerenciamento financeiro pessoal, desenvolvido com React Native e Expo. O FinTrack permite registrar receitas e despesas, acompanhar o orçamento mensal e importar movimentações bancárias por meio de Open Finance.

## Objetivo

Facilitar o controle das finanças pessoais em um único ambiente, oferecendo uma visão organizada das receitas, despesas e movimentações bancárias.

## Funcionalidades

- Cadastro e autenticação de usuários.
- Gerenciamento de receitas e despesas.
- Criação, edição e exclusão de lançamentos.
- Integração com Open Finance para conexão com instituições bancárias.
- Importação de movimentações bancárias e faturas em modo de leitura.
- Dashboard mensal com saldo, receitas e despesas.
- Gráfico de despesas por categoria.
- Histórico de lançamentos agrupado por dia.
- Busca e filtros por tipo de movimentação.
- Exportação de lançamentos em CSV.
- Isolamento dos dados por usuário utilizando Row Level Security (RLS).

## Tecnologias utilizadas

| Tecnologia | Aplicação |
| --- | --- |
| React Native | Desenvolvimento mobile |
| Expo | Ambiente de desenvolvimento |
| Expo Router | Navegação entre telas |
| TypeScript | Tipagem estática |
| Supabase Auth | Autenticação |
| Supabase PostgreSQL | Banco de dados |
| Supabase Edge Functions | Operações no servidor |
| Pluggy | Integração Open Finance |
| TanStack Query | Gerenciamento de dados |
| React Hook Form | Gerenciamento de formulários |
| Zod | Validação de dados |
| Jest | Testes automatizados |

## Estrutura do projeto

```text
.
├── app/                  # Telas e rotas do aplicativo
│   ├── (app)/            # Área autenticada
│   └── (auth)/           # Autenticação e cadastro
├── assets/               # Recursos visuais
├── src/
│   ├── components/       # Componentes reutilizáveis
│   ├── context/          # Contextos da aplicação
│   ├── hooks/             # Hooks personalizados
│   ├── lib/               # Configurações e bibliotecas
│   ├── schemas/           # Validações
│   ├── services/          # Serviços e comunicação com APIs
│   ├── theme/             # Tema visual
│   ├── types/             # Tipos TypeScript
│   └── utils/             # Funções auxiliares
├── supabase/
│   ├── functions/         # Funções de backend
│   ├── schema.sql         # Estrutura principal do banco
│   └── open-finance.sql   # Estrutura do Open Finance
├── __tests__/             # Testes automatizados
├── .env.example           # Modelo de variáveis de ambiente
└── package.json           # Dependências e scripts
```

## Configuração e execução

### 1. Pré-requisitos

- Node.js e npm.
- Projeto criado no Supabase.
- Expo Go ou ambiente compatível para executar o aplicativo.

### 2. Instalação

Clone o repositório e instale as dependências:

```bash
npm ci
```

### 3. Configuração do banco de dados

Crie um projeto no [Supabase](https://supabase.com).

No SQL Editor do projeto, execute os arquivos nesta ordem:

1. `supabase/schema.sql`
2. `supabase/open-finance.sql`

### 4. Variáveis de ambiente

Copie o arquivo `.env.example` para `.env` e preencha os dados do projeto Supabase:

```env
EXPO_PUBLIC_SUPABASE_URL=https://SEU-PROJETO.supabase.co
EXPO_PUBLIC_SUPABASE_KEY=SUA-CHAVE-PUBLICA
EXPO_PUBLIC_OPEN_FINANCE_SANDBOX=true
```

Não compartilhe chaves privadas nem envie arquivos `.env` com credenciais reais ao repositório.

### 5. Configuração do Open Finance

A integração utiliza a plataforma [Pluggy](https://dashboard.pluggy.ai).

Para habilitar essa funcionalidade:

1. Crie uma aplicação na plataforma Pluggy.
2. Configure as credenciais no ambiente do Supabase.
3. Publique as Edge Functions:

```bash
npx supabase login
npx supabase link --project-ref SEU-PROJECT-REF

npx supabase secrets set PLUGGY_CLIENT_ID=...
npx supabase secrets set PLUGGY_CLIENT_SECRET=...

npx supabase functions deploy bank-connect-token
npx supabase functions deploy bank-sync
npx supabase functions deploy bank-disconnect
```

As credenciais privadas da Pluggy devem permanecer no servidor, nunca no aplicativo mobile.

### 6. Inicialização

Inicie o ambiente de desenvolvimento:

```bash
npx expo start
```

Siga as instruções apresentadas no terminal para abrir o aplicativo em um dispositivo ou emulador compatível.

## Funcionamento do Open Finance

O fluxo de integração ocorre da seguinte maneira:

1. O aplicativo solicita um token de conexão ao backend.
2. O backend consulta a Pluggy e disponibiliza um token temporário.
3. O usuário realiza a conexão e concede autorização à instituição bancária.
4. O aplicativo solicita a sincronização das movimentações.
5. O backend consulta os dados autorizados na Pluggy.
6. As movimentações passam pelas regras de tratamento e são registradas no banco de dados.

A integração é realizada em modo de leitura. O aplicativo não executa transferências ou pagamentos bancários.

## Segurança e privacidade

- Autenticação de usuários pelo Supabase Auth.
- Políticas de Row Level Security para isolamento dos registros.
- Verificação de propriedade das conexões bancárias.
- Credenciais privadas mantidas nas funções do servidor.
- Conexão bancária com autorização do usuário.

## Testes e comandos disponíveis

| Comando | Descrição |
| --- | --- |
| `npm start` | Inicia o Expo |
| `npm test` | Executa os testes automatizados |
| `npm run typecheck` | Verifica os tipos TypeScript |
| `npm run format` | Formata o código com Prettier |

Os testes devem ser executados no ambiente configurado antes de considerar a aplicação validada.
