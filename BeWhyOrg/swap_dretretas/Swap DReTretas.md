---
Project: BewhyOrg
Sub-Project: swap-dretretas 
---



# Objectivo
Melhorar um projecto que já existe online, [dre_tretas](https://dre.tretas.org/), com o seu [repositório](https://gitlab.com/hgg/dre/) e fazer melhor :) mais analise da BD em [[Dre_Tretas]]

## Estrutura do Projecto
Directórios e ficheiros do projecto swap_dretretas:
```sh

carlos@bewhyorg-main:~/BewhyOrg/swap_dretretas$ tree -I 'vendor|node_modules'
.
├── LICENSE
├── php
│   ├── composer.json
│   ├── composer.lock
│   ├── index.php
│   ├── lib
│   │   └── MigrationHandler.php
│   ├── models
│   │   ├── Laravel
│   │   │   ├── AnexoTipo.php
│   │   │   ├── Cidadao
│   │   │   │   ├── CidadaoAnexo.php
│   │   │   │   ├── CidadaoCargoAnexo.php
│   │   │   │   ├── CidadaoCargo.php
│   │   │   │   ├── CidadaoContacto.php
│   │   │   │   ├── CidadaoDados.php
│   │   │   │   ├── CidadaoMorada.php
│   │   │   │   ├── CidadaoNacionalidade.php
│   │   │   │   ├── Cidadao.php
│   │   │   │   └── CidadaoRelacao.php
│   │   │   ├── ContactoTipo.php
│   │   │   ├── Instituicao
│   │   │   │   ├── InstituicaoAnexo.php
│   │   │   │   ├── InstituicaoCargoAnexo.php
│   │   │   │   ├── InstituicaoCargoLei.php
│   │   │   │   ├── InstituicaoCargo.php
│   │   │   │   ├── InstituicaoComRamo.php
│   │   │   │   ├── InstituicaoContacto.php
│   │   │   │   ├── InstituicaoDados.php
│   │   │   │   ├── InstituicaoLegislaturaAnexo.php
│   │   │   │   ├── InstituicaoLegislatura.php
│   │   │   │   ├── InstituicaoLei.php
│   │   │   │   ├── InstituicaoMorada.php
│   │   │   │   ├── InstituicaoNacionalidade.php
│   │   │   │   ├── Instituicao.php
│   │   │   │   ├── InstituicaoPresidencialAnexo.php
│   │   │   │   ├── InstituicaoPresidencial.php
│   │   │   │   ├── InstituicaoRamo.php
│   │   │   │   └── InstituicaoRelacao.php
│   │   │   ├── Legislatura.php
│   │   │   ├── Lei
│   │   │   │   ├── DiarioRepublica.php
│   │   │   │   ├── DiarioRepublicaPublicacaoAnexo.php
│   │   │   │   ├── DiarioRepublicaPublicacaoLei.php
│   │   │   │   ├── DiarioRepublicaPublicacao.php
│   │   │   │   ├── DiarioRepublicaSerie.php
│   │   │   │   ├── EntidadeJuridicaAnexo.php
│   │   │   │   ├── EntidadeJuridicaLei.php
│   │   │   │   ├── EntidadeJuridica.php
│   │   │   │   ├── LeiAdenda.php
│   │   │   │   ├── LeiAnexo.php
│   │   │   │   ├── LeiEmissor.php
│   │   │   │   └── Lei.php
│   │   │   ├── Presidencial.php
│   │   │   ├── RelacaoTipo.php
│   │   │   ├── Republica.php
│   │   │   └── Search
│   │   │       ├── Concelho.php
│   │   │       ├── Freguesia.php
│   │   │       └── Nacionalidade.php
│   │   └── Tretas
│   │       ├── DocumentDReConnectsTo.php
│   │       ├── DocumentDRe.php
│   │       └── DocumentDReText.php
│   └── src
├── python
└── README.md
12 directories, 56 files
```

## Contexto dos Directórios

- php: 
	Pasta principal onde o desenvolvimento do código PHP está ocorrendo. Aqui, estamos definindo a lógica principal da migração e transformação dos dados, incluindo modelos e handlers para a migração.

	 - Laravel representa a Base de Dados que queremos e a extrutura das tabelas
		- [[Cidadãos]] - Tudo do Cidadao
		- [[Instituições e Ciclos Políticos]] - Tudo da intituicao
		- [[Leis de Portugal]] - Tudo da lei
		- [[Kernel|Search]] - Objectos genéricos, que devido ao número de rows, terá que ser consultado na base dados final para ter id's
		- [[Kernel| Ficheiros Soltos]] representam tabelas parâmetro, que ambas as 3 entidades principais também usam, mas os ids possíveis de obter estão no próprio ficheiro
	- [[Dre_Tretas]]  - Os ficheiros que tem as tabelas que estamos a analisar

- python: 
	Contém um extractor existente do Diário da República Electrónico (DRe). O objectivo é melhorar este extractor para capturar todos os dados relevantes, incluindo links que estão faltando ou desactualizados, possibilitando uma actualização completa ou refazer a extracção.

## Fases do Desenvolvimento

- [[Fase 1]] (Current): Análise da base de dados original. Estamos no processo de extrair elementos individuais da base de dados original e decompor em aproximadamente 40 objectos diferentes, partindo de 3 tabelas principais.

 - [[Fase 2]]: Integração de detalhes adicionais ao projecto, utilizando requisições à [[Wikidata]] através da biblioteca Guzzle para enriquecer os dados existentes.

 - [[Fase 3]]: Criar outros *middle-wares* que trabalhem sobre a informação, aka [[AI]]
 - [[Fase 4]]: Documentar e activar mais Instituições ou tipo de dados

Próximos Passos:

    -continuar a melhorar a estrutura de dados

## Tarefas

 - [ ] 🔺 Mapear Instituições Monarquia #bewhyorg/swap_dretretas #task ⏳ 2025-03-05 📅 2025-03-08
 - [ ] 🔺 Analisar BD tretas, mapear campos #bewhyorg/swap_dretretas #task ⏳ 2025-03-27 📅 2025-03-05

