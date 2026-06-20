# Descrição do Protótipo Exploratório — Sistema de Gestão da Associação de Apicultores

## Como usar este documento

Este texto detalha, tela por tela, a estrutura e os componentes do protótipo exploratório citado na sua metodologia (etapa "Elicitação de requisitos — Prototipação exploratória / Mobile First"). Ele foi construído a partir do Documento Detalhado de Requisitos (RF01–RF22, RNF001–RNF007) que você já levantou, mas — como o próprio fluxo metodológico prevê — é um ponto de partida exploratório, não a especificação final. Espera-se que mude após as rodadas com os stakeholders.

Você pode usar este documento de duas formas:
1. Como referência ao desenhar as telas manualmente no Figma.
2. Colando os blocos de "Componentes" de cada tela diretamente em uma IA generativa de UI do Figma, tela por tela (funciona melhor pedir uma tela por vez do que o documento inteiro de uma vez).

---

## 1. Diretrizes Gerais de Design

### 1.1 Direção visual sugerida

Como não há ainda um manual de identidade oficial, sugiro uma direção ancorada no universo real da apicultura (favo, cera, fumaça do apiário, madeira da casa do mel) em vez de um "amarelo genérico de mel":

**Paleta (nomeada, com hex de referência):**
- `Mel escuro` `#B8730A` — cor primária (botões principais, links ativos, ícones de destaque)
- `Favo claro` `#F2D38A` — cor secundária/destaque suave (fundos de cards de destaque, badges neutros)
- `Casca de eucalipto` `#4A3526` — texto principal e elementos escuros (evoca a madeira da casa do mel)
- `Cera natural` `#FBF4E4` — fundo geral das telas (mais quente que um branco puro)
- `Verde pasto` `#5E7A45` — estados positivos (pago, ativo, saldo positivo)
- `Terracota alerta` `#B5482F` — estados de erro/atraso/pendência
- `Fumaça` `#8A8071` — texto secundário, bordas, ícones inativos

**Tipografia:**
- Títulos/marca: uma serifada com leve toque artesanal (ex.: Fraunces, Lora) — transmite tradição e associativismo.
- Corpo de texto e formulários: uma sans-serif humanista de alta legibilidade (ex.: Inter, IBM Plex Sans).
- Números/tabelas financeiras: a mesma sans-serif, mas com variante de **algarismos tabulares**, essencial para alinhar valores em R$ nas telas financeiras.

**Elemento de assinatura (signature element):** o hexágono do favo de mel, usado de forma estrutural — não decorativa. Sugestões de uso:
- Os cantos de cards de resumo financeiro/produção têm leve corte hexagonal (em vez de border-radius padrão).
- O gráfico de rateio entre sócios é representado como um "favo" de células hexagonais, uma por sócio, proporcional ao valor recebido — isso conecta literalmente a metáfora do favo à divisão de produção, que é o coração do sistema.
- Divisores de seção usam uma micro-trama hexagonal sutil em vez de uma linha reta simples.

### 1.2 Padrões de componentes recorrentes

- **Botão primário**: preenchido em `Mel escuro`, texto em `Cera natural`, canto levemente arredondado/hexagonal.
- **Botão secundário**: contorno em `Mel escuro`, fundo transparente.
- **Badges de status**: pílulas coloridas — verde (`Ativo`/`Pago`), terracota (`Pendente`/`Atrasado`), cinza (`Inativo`).
- **Campos de formulário**: label sempre visível acima do campo (não usar apenas placeholder), mensagem de erro abaixo do campo em terracota, ícone de "olho" para campos de senha.
- **Cards**: usados para resumos no dashboard e para itens de lista em telas mobile (em vez de tabelas densas).
- **Tabelas**: usadas em telas desktop/financeiras onde a densidade de dados compensa; em mobile, a mesma informação se converte em lista de cards.
- **Estados de tela obrigatórios** (desenhar os 4 para cada tela de listagem):
 1. **Carregando**: skeleton screens (blocos cinza animados no formato dos cards/linhas reais).
 2. **Vazio**: ilustração simples + frase de orientação + botão de ação (ex.: "Nenhum sócio cadastrado ainda" + botão "Cadastrar primeiro sócio").
 3. **Erro**: mensagem clara, sem jargão técnico, com botão "Tentar novamente" (atende ao RNF007, que pede mensagens de erro com código de referência para suporte).
 4. **Preenchido**: estado normal com dados.

### 1.3 Navegação

- **Mobile** (prioridade pela abordagem Mobile First): barra de navegação inferior fixa com 5 ícones — Início, Sócios, Financeiro, Produção, Mais. O item "Mais" abre um menu com Prestação de Contas, Administração (se Admin) e Perfil/Sair.
- **Desktop**: a mesma navegação se transforma em uma barra lateral fixa à esquerda, expandida (ícone + texto), com cabeçalho superior mostrando nome do usuário, perfil ativo (badge) e botão de logout.
- Viewport de referência para desenhar mobile: 375×812. Para desktop: ≥1280px de largura.

---

## 2. Perfis de Acesso (RF06–RF08)

| Perfil | Visão geral de acesso |
|---|---|
| **Administrador** | Acesso total, incluindo gestão de perfis/permissões e logs de auditoria |
| **Tesoureiro** | Financeiro completo, produção/rateio, prestação de contas, sócios (leitura e edição) |
| **Associado** | Seu próprio perfil, sua produção lançada, status de mensalidade, prestações de contas (leitura) |
| **Visitante** | Conteúdo institucional público e leitura limitada (ex.: prestação de contas já publicada) |

Cada tela abaixo indica, quando relevante, quais perfis a acessam.

---

## 3. Mapa de Telas

**Autenticação:** Login · Cadastro de usuário · Recuperar senha · Redefinir senha
**Navegação principal:** Dashboard (varia por perfil) · Menu/navegação
**Sócios:** Lista de sócios · Cadastro/edição de sócio · Perfil detalhado do sócio · Documentos do sócio
**Financeiro:** Controle de mensalidades · Registro de receita · Registro de despesa · Fluxo de caixa
**Produção e rateio:** Lançamento de produção · Cálculo/relatório de rateio
**Prestação de contas:** Relatório de gastos mensais · Demonstrativo de produção e pagamentos
**Administração:** Gerenciamento de perfis e permissões · Logs de auditoria
**Conta:** Perfil do usuário / configurações

---

## 4. Detalhamento das Telas

### 4.1 Autenticação

#### Tela de Login
**Objetivo:** autenticar o usuário (RF02) e direcioná-lo ao dashboard correspondente ao seu perfil.
**Componentes:**
- Logo/nome da associação centralizado no topo
- Campo "E-mail ou usuário" (validação de formato em tempo real)
- Campo "Senha" com ícone de mostrar/ocultar
- Checkbox "Manter-me conectado"
- Botão primário "Entrar" (com spinner de carregamento durante a chamada)
- Link "Esqueci minha senha"
- Link "Não tem conta? Cadastre-se" (se o cadastro for autoatendido)
- Área de erro inline acima do botão para "credenciais inválidas"
**Estados:** padrão · carregando · erro de credencial
**Navegação:** sucesso → Dashboard · "Esqueci minha senha" → tela de recuperação

#### Tela de Cadastro de Usuário (RF01)
**Componentes:**
- Campos: nome completo, e-mail, senha, confirmar senha
- Indicador visual de força da senha (atende aos critérios de complexidade exigidos)
- Checkbox de aceite de termos/política de uso de dados
- Botão primário "Criar conta"
- Link "Já tenho conta"
**Validações:** e-mail único (mensagem "este e-mail já está cadastrado"), senha com critérios mínimos detalhados em texto de apoio abaixo do campo

#### Tela de Recuperar Senha (RF03) — etapa 1
**Componentes:** campo de e-mail, botão "Enviar link de recuperação", e um estado de confirmação ("Verifique seu e-mail") que substitui o formulário após o envio.

#### Tela de Redefinir Senha — etapa 2 (acessada via link do e-mail)
**Componentes:** campo "Nova senha", campo "Confirmar nova senha", botão "Redefinir senha", mensagem de sucesso com redirecionamento automático ao login.

---

### 4.2 Dashboard (Início)

**Objetivo:** dar uma visão imediata da saúde da associação, adaptada ao perfil logado.

**Componentes (Administrador/Tesoureiro):**
- Card grande de destaque: "Saldo em caixa" (RF17), com variação em relação ao mês anterior
- Dois cards menores: "Total arrecadado no mês" / "Total gasto no mês"
- Card: "Mensalidades pendentes" (contagem + valor total)
- Mini-gráfico de barras: entradas x saídas dos últimos 6 meses
- Card: "Produção do mês" (kg de mel total entregue)
- Lista "Atividade recente" (últimos lançamentos, últimos sócios cadastrados)

**Componentes (Associado):**
- Card: "Minha mensalidade" com status (Pago/Pendente) e botão de ação se pendente
- Card: "Minha produção este mês" (kg entregues)
- Card: "Valor a receber" (calculado conforme RF20)
- Aviso/banner de comunicados da associação, se houver

**Componentes (Visitante):**
- Bloco institucional resumido (sobre a associação)
- Link para a última prestação de contas publicada

---

### 4.3 Módulo de Sócios (RF10–RF13)

#### Lista de Sócios
**Componentes:**
- Barra de busca por nome/CPF
- Filtro por status: chips "Todos / Ativos / Inativos" (RF13)
- Em mobile: lista de cards (nome, status badge, data de admissão)
- Em desktop: tabela (nome, CPF mascarado, status, data de admissão, ações)
- Botão flutuante "+" (mobile) ou botão "Novo sócio" no topo (desktop)
- Paginação ao final da lista

#### Cadastro/Edição de Sócio (RF10, RF11)
**Componentes:**
- Campos pessoais: nome completo, CPF (com máscara), telefone, e-mail
- Endereço: CEP (com autopreenchimento), rua, número, bairro, cidade, UF
- Campo "Data de admissão"
- Campo "Data de desligamento" (visível apenas em edição, opcional)
- Botões "Salvar" e "Cancelar"

#### Documentos do Sócio (RF12)
**Componentes:**
- Área de upload (arrastar e soltar ou botão "Selecionar arquivo")
- Lista de documentos anexados: ícone por tipo de arquivo, nome, data de envio, botões de visualizar/baixar/remover
- Indicador de progresso durante upload

#### Perfil Detalhado do Sócio
**Componentes:**
- Cabeçalho: avatar/iniciais, nome, status badge, datas de admissão/desligamento
- Abas: "Dados pessoais" · "Documentos" · "Produção" · "Pagamentos"
- Botões de ação: "Editar", "Alterar status", "Desligar sócio"

---

### 4.4 Módulo Financeiro e Tesouraria (RF14–RF17)

#### Controle de Mensalidades (RF14)
**Componentes:**
- Filtro por mês de referência e por status (Pago/Pendente/Atrasado)
- Lista/tabela: nome do sócio, mês, valor, status badge, botão "Dar baixa"
- Modal de confirmação de baixa: valor, data do pagamento, forma de pagamento (PIX/Dinheiro), botão "Confirmar"

#### Registro de Receita (RF15)
**Componentes (modal ou tela dedicada):**
- Tipo de receita (dropdown: mensalidade, venda de mel, outro)
- Valor (campo monetário)
- Forma de recebimento: toggle PIX / Dinheiro
- Data
- Campo de observação
- Botão "Salvar"

#### Registro de Despesa (RF16)
**Componentes:**
- Categoria (dropdown: água, energia, internet, manutenção da casa do mel, outro)
- Valor
- Data
- Campo de descrição
- Upload opcional de comprovante
- Botão "Salvar"

#### Fluxo de Caixa (RF17)
**Componentes:**
- Card de saldo atual em destaque (Total arrecadado − Total gasto, calculado automaticamente)
- Seletor de período (mês atual / últimos 3 meses / personalizado)
- Gráfico de entradas x saídas por período
- Tabela detalhada de transações com busca e filtro por tipo (receita/despesa)

---

### 4.5 Módulo de Produção e Rateio (RF18–RF20)

#### Lançamento de Produção por Sócio (RF18)
**Componentes:**
- Campo "Sócio" (dropdown com busca)
- Campo "Data da entrega"
- Campo "Peso entregue (kg)"
- Botão "Registrar"
- Lista dos últimos lançamentos abaixo do formulário, com opção de editar/excluir

#### Cálculo de Rateio (RF19, RF20)
**Componentes:**
- Campo de configuração: "Valor de venda do mel por kg"
- Campo de configuração: "Percentual de retenção da associação (Casa do Mel)"
- Botão "Calcular"
- Resultado em forma de "favo" — células hexagonais, uma por sócio, com tamanho proporcional ao valor líquido recebido (o elemento de assinatura visual mencionado na seção 1.1)
- Tabela de apoio abaixo do favo: sócio, peso entregue, valor bruto, retenção da associação, valor líquido
- Botão "Gerar relatório" (exportação em PDF, útil para a reunião com sócios)

---

### 4.6 Módulo de Prestação de Contas (RF21–RF22)

#### Relatório de Gastos Mensais (RF21)
**Componentes:**
- Seletor de mês/ano
- Lista detalhada de despesas do período (data, categoria, valor, descrição)
- Total geral ao final
- Botão "Exportar/Imprimir"

#### Demonstrativo de Produção e Pagamentos (RF22)
**Componentes:**
- Resumo no topo: total de mel produzido, valor total vendido, valor retido pela associação, valor distribuído aos sócios
- Tabela por sócio (mesma estrutura do módulo de rateio)
- Gráfico simples mostrando a proporção retida x distribuída

---

### 4.7 Módulo de Administração (RF06–RF09) — apenas Administrador

#### Gerenciamento de Perfis e Permissões
**Componentes:**
- Lista de perfis (Administrador, Tesoureiro, Associado, Visitante)
- Matriz de permissões: checkboxes por módulo (Criar / Editar / Excluir / Visualizar)
- Tela de atribuição: selecionar usuário → atribuir perfil via dropdown

#### Logs de Auditoria (RF09)
**Componentes:**
- Tabela: data/hora, usuário, ação realizada, status (sucesso/falha)
- Filtros por período, usuário e tipo de ação

---

### 4.8 Perfil do Usuário / Configurações
**Componentes:**
- Dados da conta: nome, e-mail, foto/avatar
- Botão "Alterar senha"
- Preferências de notificação (se aplicável ao escopo final)
- Botão "Sair"

---

## 5. Dicas para Uso com a IA do Figma

- Cole uma tela por vez (não o documento inteiro), começando pela seção "Componentes" daquela tela específica.
- Sempre informe o viewport antes do prompt: "Desenhe para mobile, 375×812px" ou "Desenhe para desktop, 1280px de largura".
- Cole o bloco da seção 1.1 (paleta + tipografia) uma única vez no início da sessão, para a IA manter consistência entre as telas seguintes.
- Peça explicitamente os 4 estados (carregando, vazio, erro, preenchido) quando a tela for de listagem — isso costuma ser esquecido pela IA se não for pedido.

---

## 6. Observação Final

Esta descrição é o ponto de partida do **protótipo exploratório** da sua metodologia — ela existe para ser apresentada aos stakeholders, gerar discussão e ser ajustada nos ciclos iterativos descritos na Figura 1. Os requisitos aqui referenciados (RF/RNF) ainda não são oficiais, conforme você mesmo observou, e devem ser revistos após a validação do protótipo junto aos stakeholders.