---
Project: BewhyOrg
Sub-Project: lumen-back
---

# Final url: https://api.bewhy.org
# Git url: https://github.com/CarlosLiberJesus/lumen-back

# Objectivo

Mapear o Diário da República, organizar numa [[Leis de Portugal]] relacional, para conjugar sua informação ao [[Instituições e Ciclos Políticos]] e seu [[Cidadãos]].  Temos ainda que contemplar [[Kernel]], para que se percebam os tipos comuns aos elementos principais

Estamos a por de lado o [[CRM]] mas depende como o projecto evoluir, nem foi feito o ficheiro [[Laravel Default]], apesar de já documentado em [[Full-DB]].

Foi escolhido o *Laravel* para suportar a ligação há base de dados e responder em JSON ao dados inquiridos por [[Angular]] ou parceiros, gerir o ORM, etc.

## Visão

A aplicação ainda terá de passar por um processo de migração [[Swap DReTretas]] só para estruturação de dados. Depois, passaremos por processos [[AI]] para começar a extrair dados do Dre e continuar a mapear mais informação entre as instituições e pessoas. Treinar o nosso LLM especialista em assuntos do estado com a informação recolhida.

## Realismo

Apesar de se pautar pela transparência, e fornecer todos os dados finais, como a LLM, mas apenas parte da BD será pública. teremos de ser comedidos com os grandes nomes, e a extracção será para contra-legislar com o [[Partido Libertário]]

# Database

Foi criado um Modelo completo
![[Full-DB#Documentação]]


Foi dividido em 4 partes principais!

## Cidadão

> [!tip]
> **Identificar e mapear**, muitas destas linhas até já são fornecidas por dados <u>estáticos</u> provenientes do parlamento.pt; outros terão de ser <u>inferidos</u> da documentação

![[Cidadãos#Documentação]]

## Instituições

> [!tip]
> **Prioritário**, mas estático. Dados vindos de Documentação parlamentar. *Importante* porque as Leis têm emissores;
> **Pensar**, nas instituições fora do arco (ex ONGs, CNE), muito inferido de novos DRe's

![[Instituições e Ciclos Políticos#Documentação]]

## Leis

> [!tip]
> O processo [[Dre_Tretas]] foi então contruido para alimentar estas tabelas

![[Leis de Portugal#Documentação]]

## Complicadas

> [!tip]
> Estas tabelas são as que terão de ser mais inferidas dos contextos dos DRe
> Todas estas tabelas deveriam ter o seu documento dado a complexidade que apresentam

![[Complicadas#Documentação]]


