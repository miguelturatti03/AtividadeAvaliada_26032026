# Avaliação — Engenharia de Software
**Sistema Integrado de Gestão de Farmácia — MVP Definido pelo Estudante**

Aluno: Miguel Turatti Quirino  
RA: 24000776  
Data: 26/03/2026  

---

# 1. Definição do MVP
Descreva aqui **qual parte do sistema** foi incluída no seu MVP.  
Explique claramente:
O MVP está devidamente focado claramente no fluxo e receita da farmacia Saude & Vida

- O que está **dentro** do MVP
* Dentro do MVP a gente encontra o cadastro de clientes, produtos, realizacao de vendas, baixa no estoque, entrada de mercadoria e até mesmo geracao de contas a pagar e receber.
- O que está **fora** do MVP
* Fora do MVP, e possivel encontrar transferencias entre unidades relatorios avançados em PowerBI, gestão de perdas, danos e auditoria detalhada
- Por que você fez essas escolhas
* Pela priorização dos processos para poder desenvolver da melhor forma para divergencias de estoque e falhas financeiras, garantindo as operações básicas, sendo realizadas de forma mais consistentes.
---

# 2. Regras de Negócio (mínimo: 5)
**RN01 — Bloqueio de Estoque:Não é possível realizar a venda com um produto igual a 0**  
**RN02 — Venda de Controlados: Medicamentos com uso controlado exigem uma validação, e assinatura do farmacêutico.**  
**RN03 — Atualização de Estoque: Toda venda realizada feita no balcão ou pelo sistema deve ter acesso ao estoque e ser atualizado.**  
**RN04 — Cadastro de Inadimplentes: Clientes com contas em atraso em até 30 dias, não terão mais a possibilidade em realizar compras seja dentro so sistema ou na propria farmácia.**  
**RN05 — Alerta para reposição: Assim que algum produto estiver com baixa, seja no minimo de 10 produtos realizar uma alerta no sistema para ser preciso repor.**  

---

# 3. Requisitos Funcionais (mínimo: 8)
**RF01 — O sistema deve realizar o cadastro consulta pelo nome do produto.**  
**RF02 — O sistema deve permitir o registro de vendas, tendo vinculo com o estoque.**  
**RF03 — O sistema deve criar automaticamente uma conta a receber quando o cliente escolher pagar a prazo.**  
**RF04 — O sistema poderá realizar o cadastro rápido de clientes sobre os momentos de vendas.**  
**RF05 — O sistema terá a possibilidade para a compra de fornecedores para alimentar o estoque.**  
**RF06 — O sistema deve gerar um lançamento em 'Contas a Pagar' assim que o Gerente confirmar o recebimento de uma compra de fornecedor.**  
**RF07 — O sistema deverá gerar um comprovante ao final da operação de venda.**  
**RF08 — Permitir que o administrador altere o status da venda, como: "aberta/paga".**  

---

#4. Requisitos Não Funcionais (mínimo: 4)
Liste os RNFs do sistema conforme seu MVP.

**RNF01 — Desempenho: A consulta tando no balcão quando no sistema devera ter um prazo de no máximo 2 segundos.**  
**RNF02 — Segurança: O sistema devera exigir autenticação, e realizar a validação deste usuario.**  
**RNF03 — Disponibilidade: O sistema deve ter uma matriz para que todos os dados sejam concentrados e protegido para maior segurança.**  
**RNF04 — Usabilidade: Desenvolver uma interface muito bem otimizada para que não haja travamento e nem mesmo mal uso dos usuários ou técnicos da saúde que estarão no atendimento.**  

---

# 5. Casos de Uso (mínimo: 10)
### Inserir **diagrama de casos de uso geral**, demonstrando claramente:
- os 10 casos
- relação entre eles e atores
- pelo menos 3 includes
- pelo menos 3 extends
- <img width="509" height="313" alt="image" src="https://github.com/user-attachments/assets/7c60d5c1-e2d8-4485-aa7e-c9646c8db419" />


---

# 6. Documentação dos Casos de Uso
Para **cada caso de uso**, utilize o template abaixo:
---

##UC01 - Venda de Produto
**Ator(es): Atendentes**  
**Descrição: Realização da venda de algum medicamento**  
**Pré-condições: Usuário ser autenticado dentro do sistema, e cadastrar o produto.**  
**Pós-condições: Verificar o estoque se foi atualizado ou não, registrar venda e gerar o comprovante**  

###Fluxo Principal
1. Atendente iniciar a venda e identificara o produto, ou com o nome ou o seu codigo.
2. Sistema irá executar UC02-Verificar Estoque.(Include)
3. informará a quantidade desejada pelo cliente e realizará o cálculo do subtotal.
4. Atendente ira finalizar a venda no sistema e executara o UC03-Emitir Comprovante.(Include)

###Fluxos Alternativos / Exceções
- FA01 — Cliente não cadastrado: Atendente terá que executar o UC04-Castrar Clientes.(Extend)
- FA02 — Produto Controlado: Sistema será obrigatorio a solicitação de UC05-Validar Receita.(Extend)
<img width="509" height="684" alt="image" src="https://github.com/user-attachments/assets/c482bf19-fb49-42a9-8c7f-47c6575f10b0" />

---


###UC02 — Verificar Estoque
**Ator(es): Sistema**
**Descrição: Validar se a quantidade solicitada existe na unidade atual.**
**Pré-condições: Produto identificado na venda ou compra.**
**Pós-condições: Confirmação de disponibilidade ou bloqueio.**

###Fluxo Principal
1. Sistema consulta o saldo atual do produto no Banco de Dados.
2. Sistema valida se Saldo (maior ou igual) -> Quantidade Solicitada.
3. Sistema retorna mensagem de sucesso.

###Fluxos Alternativos / Exceções
EX01 — Estoque Insuficiente: Sistema alerta "Quantidade indisponível" e impede a adição do item.
<img width="417" height="405" alt="image" src="https://github.com/user-attachments/assets/1f2b0570-f889-4c36-a2d8-09b36bb1253f" />

---

###UC03 — Emitir Comprovante
**Ator(es): Sistema**
**Descrição: Gerar o documento com detalhes da operação para o cliente.**
**Pré-condições: Venda finalizada com sucesso.**
**Pós-condições: Documento impresso ou exibido na tela.**

###Fluxo Principal
1. Sistema recupera dados da venda (itens, valores, data).
2. Sistema formata o comprovante conforme padrão da rede.
3. Sistema envia comando para a impressora térmica.
<img width="289" height="397" alt="image" src="https://github.com/user-attachments/assets/cb20a652-ef08-44de-ab96-37a33b2c7b28" />

---
###UC04 — Cadastrar Cliente
**Ator(es): Atendente**
**Descrição: Registrar rapidamente um novo cliente para vincular ao histórico de compras.**
**Pré-condições: Atendente no módulo de vendas ou cadastros.**
**Pós-condições: Cliente salvo no banco de dados.**

###Fluxo Principal
1. Atendente solicita dados básicos (Nome, CPF).
2. Sistema valida se o CPF já existe.
3. Atendente salva o registro.
<img width="255" height="397" alt="image" src="https://github.com/user-attachments/assets/f0f39988-78f9-4791-97a8-4617c35d8377" />

---

###UC05 — Validar Receita
**Ator(es): Farmacêutico**
**Descrição: Autorizar a venda de medicamentos que exigem retenção de receita.**
**Pré-condições: Produto de venda controlada selecionado.**
**Pós-condições: Item liberado para venda.**

###Fluxo Principal
1. Farmacêutico analisa a receita médica física.
2. Farmacêutico insere suas credenciais de autorização no sistema.
3. Sistema registra a liberação vinculada ao CRM do profissional.
<img width="276" height="397" alt="image" src="https://github.com/user-attachments/assets/447cdce4-297b-4580-a7eb-214e59084661" />

---

###UC06 — Registrar Conta a Receber
**Ator(es): Sistema**
**Descrição: Gerar lançamento financeiro para vendas a prazo ou convênios.**
**Pré-condições: Forma de pagamento "A Prazo" selecionada.**
**Pós-condições: Título financeiro gerado com status "Aberta".**

###Fluxo Principal
1. Sistema identifica o cliente e o valor total.
2. Sistema calcula a data de vencimento.
3. Sistema grava o registro no módulo financeiro.
<img width="278" height="341" alt="image" src="https://github.com/user-attachments/assets/4389625d-cbf4-4f26-a4d1-886cd7135b60" />

---

###UC07 — Registrar Compra
**Ator(es): Gerente**
**Descrição: Dar entrada em mercadorias enviadas por fornecedores.**
**Pré-condições: Fornecedor cadastrado.**
**Pós-condições: Estoque incrementado; Conta a pagar gerada.**

###Fluxo Principal
1. Gerente informa o fornecedor e a nota fiscal.
2. Gerente insere produtos e quantidades recebidas.
3. Sistema executa UC02 — Verificar Estoque (Include para atualização).
4. Sistema gera lançamento em "Contas a Pagar".
<img width="278" height="397" alt="image" src="https://github.com/user-attachments/assets/f79ebedd-925b-455d-9986-316f4f3a3c00" />

---

###UC08 — Manter Produto
**Ator(es): Gerente**
**Descrição: Cadastrar ou editar informações técnicas de produtos.**
**Pré-condições: Usuário com perfil de Gerente.**
**Pós-condições: Catálogo de produtos atualizado.**

###Fluxo Principal
1. Gerente acessa o módulo de produtos.
2. Gerente insere Descrição, Preço, Fabricante e Nível Mínimo.
3. Sistema salva as alterações.
<img width="256" height="341" alt="image" src="https://github.com/user-attachments/assets/9aa2f529-4053-4794-8e0a-0efeee07c8ea" />

---

###UC09 — Baixar Conta
**Ator(es): Financeiro**
**Descrição: Registrar o pagamento de uma conta a pagar ou o recebimento de uma conta a receber.**
**Pré-condições: Título financeiro existente no sistema.**
**Pós-condições: Status da conta alterado para "Paga" ou "Recebida".**

###Fluxo Principal
1. Usuário pesquisa o título por data ou favorecido.
2. Usuário confirma o valor e a data da transação.
3. Sistema atualiza o status do lançamento.
<img width="262" height="341" alt="image" src="https://github.com/user-attachments/assets/984f86cc-dc8d-4568-94f1-cefd9862eb9a" />

---

###UC10 — Gerar Relatório de Estoque
**Ator(es): Gerente / Administrador**
**Descrição: Listar produtos com estoque abaixo do nível mínimo para decisão de compra.**
**Pré-condições: Existir movimentação de estoque.**
**Pós-condições: Relatório exibido ou exportado.**

###Fluxo Principal
1. Gerente seleciona o filtro "Abaixo do Mínimo".
2. Sistema processa os saldos de todas as unidades.
3. Sistema exibe a lista de reposição necessária.
<img width="284" height="397" alt="image" src="https://github.com/user-attachments/assets/3a845deb-71aa-499d-adbd-5ff9ef5999ba" />
