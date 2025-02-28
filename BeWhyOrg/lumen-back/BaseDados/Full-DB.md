# Database Decomposition

## Notas Prévias
 - padrões de campos
	 - uuid - assume-se que será possível um link no front end
	 - path - um caminho para algo interno na storage do servidor
	 - src - remove url que justifica o elemento
 - *_anexos
	 - Existem muitas Tabela Anexo, para ir pondo os vários documentos ao tipo de objecto. O Objectivo é poder guardar um link ou um doc, sendo a tabela anexo_tipo o identificador o valor atribuido
 - Campos Enum
	 - Dado à complexidade do modelo de negócio, algumas tabelas têm filhos, sendo o exemplo mais importante, instituicao_legislaturas. São filhos independentes de uma instituição. Os outros objectos podem então relacionar-se tanto com o id do pai, mas também com o id do filho. Os enums representam então, qual o id da tabela que realmente queremos ligar
- Estamos a usar o squema, deve ser sempre importado um dump da BD local, e actualizar a documentação

## Laravel Default
```shell

Table "cache" {
"key" varchar(255) [pk, not null]
"value" mediumtext [not null]
"expiration" int [not null]
}

Table "cache_locks" {
"key" varchar(255) [pk, not null]
"owner" varchar(255) [not null]
"expiration" int [not null]
}



Table "failed_jobs" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"connection" text [not null]
"queue" text [not null]
"payload" longtext [not null]
"exception" longtext [not null]
"failed_at" timestamp [not null, default: `CURRENT_TIMESTAMP`]
}

Table "jobs" {
"id" bigint [pk, not null]
"queue" varchar(255) [not null]
"payload" longtext [not null]
"attempts" tinyint [not null]
"reserved_at" int [default: NULL]
"available_at" int [not null]
"created_at" int [not null]
}

Table "job_batches" {
"id" varchar(255) [pk, not null]
"name" varchar(255) [not null]
"total_jobs" int [not null]
"pending_jobs" int [not null]
"failed_jobs" int [not null]
"failed_job_ids" longtext [not null]
"options" mediumtext
"cancelled_at" int [default: NULL]
"created_at" int [not null]
"finished_at" int [default: NULL]
}

Table "migrations" {
"id" int [pk, not null]
"migration" varchar(255) [not null]
"batch" int [not null]
}

Table "password_reset_tokens" {
"email" varchar(255) [pk, not null]
"token" varchar(255) [not null]
"created_at" timestamp [default: NULL]
}  

Table "personal_access_tokens" {
"id" bigint [pk, not null]
"tokenable_type" varchar(255) [not null]
"tokenable_id" bigint [not null]
"name" varchar(255) [not null]
"token" varchar(64) [not null]
"abilities" text
"last_used_at" timestamp [default: NULL]
"expires_at" timestamp [default: NULL]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "sessions" {
"id" varchar(255) [pk, not null]
"user_id" bigint [default: NULL]
"ip_address" varchar(45) [default: NULL]
"user_agent" text
"payload" longtext [not null]
"last_activity" int [not null]
}
```

## Kernel
```shell
Enum "statuses_type_enum" {
"users"
"documents"
"tasks"
}

Table "anexo_tipos" {
"id" bigint [pk, not null]
"tipo" varchar(100) [not null]
"description" varchar(255) [not null]
"params" json [default: NULL]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "concelhos" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"name" varchar(100) [not null]
"descriptions" json [default: NULL]
"distrito_id" bigint [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "concelho_anexos" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"nome" varchar(255) [not null]
"concelho_id" bigint [not null]
"anexo_tipo_id" bigint [not null]
"path" varchar(255) [default: NULL, note: 'Caminho em server do arquivo']
"src" varchar(255) [default: NULL, note: 'URL Fonte do arquivo']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "contacto_tipos" {
"id" bigint [pk, not null]
"nome" varchar(100) [not null]
"description" text
"params" json [default: NULL]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Separar emails, telefones, endereços, etc. Os params contêm arrays de parametros para o front-end (css classes)'
}

Table "distritos" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"name" varchar(100) [not null]
"descriptions" json [default: NULL]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "distrito_anexos" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"nome" varchar(255) [not null]
"distrito_id" bigint [not null]
"anexo_tipo_id" bigint [not null]
"path" varchar(255) [default: NULL, note: 'Caminho em server do arquivo']
"src" varchar(255) [default: NULL, note: 'URL Fonte do arquivo']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}


Table "freguesias" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"name" varchar(100) [not null]
"descriptions" json [default: NULL]
"distrito_id" bigint [not null]
"concelho_id" bigint [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "freguesia_anexos" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"nome" varchar(255) [not null]
"freguesia_id" bigint [not null]
"anexo_tipo_id" bigint [not null]
"path" varchar(255) [default: NULL, note: 'Caminho em server do arquivo']
"src" varchar(255) [default: NULL, note: 'URL Fonte do arquivo']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}


Table "legislaturas" {
"id" bigint [pk, not null]
"uuid" char(36) [not null]
"nome" varchar(255) [not null]
"code" char(6) [not null]
"republica_id" bigint [not null]
"eleicoes" date [default: NULL]
"formacao" date [not null]
"dissolucao" date [default: NULL]
"sinopse" text
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Representa os ciclos das eleições e formação de Governo|Assembleia da Republica. Vai funcionar excelente para a actualidade pós 1974, republicas.id = 4, teremos de adaptar as anteriores.'
}

Table "legislatura_anexos" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"nome" varchar(255) [not null]
"legislatura_id" bigint [not null]
"anexo_tipo_id" bigint [not null]
"path" varchar(255) [default: NULL, note: 'Caminho em server do arquivo']
"src" varchar(255) [default: NULL, note: 'URL Fonte do arquivo']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Devido à estrutura da DB, acaba por ser os documentos do ramo do executivo do Governo, por presidente representado legislatura_id. Não esquecer que existe uma mais importante instituicao_legislatura_anexos.'
}

Table "nacionalidades" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"nacionalidade" varchar(255) [not null]
"pais" varchar(255) [not null]
"params" json [default: NULL, note: 'Como outras, será classes de css para as bandeiras']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "logs" {
"id" bigint [pk, not null]
"user_id" bigint [default: NULL]
"code" int [not null]
"url" varchar(255) [not null]
"params" json [default: NULL]
"reply" json [default: NULL]
"time" int [default: NULL]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "newsletter_users" {
"id" bigint [pk, not null]
"name" varchar(255) [not null]
"email" varchar(255) [not null]
"phone" varchar(255) [default: NULL]
"hash" varchar(255) [not null, note: 'uuid para unsubscribe']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "presidenciais" {
"id" bigint [pk, not null]
"uuid" char(36) [not null]
"republica_id" bigint [not null]
"eleicoes" date [default: NULL]
"posse" date [not null]
"termino" date [default: NULL]
"sinopse" text
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Representa os ciclos das eleições de Presidente da Republica Republica. Vai funcionar excelente para a actualidade pós 1974, republicas.id = 4, talvez pós 1910, republicas.id > 2.'
}

Table "presidencial_anexos" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"nome" varchar(255) [not null]
"presidencial_id" bigint [not null]
"anexo_tipo_id" bigint [not null]
"path" varchar(255) [default: NULL, note: 'Caminho em server do arquivo']
"src" varchar(255) [default: NULL, note: 'URL Fonte do arquivo']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Devido à estrutura da DB, acaba por ser os documentos do ramo do executivo mas da Presidencia da República, por presidente representado presidencial_id. Não esquecer que existe uma mais importante instituicao_presidencial_anexos.'
}

Table "relacao_tipos" {
"id" bigint [pk, not null]
"entre" relacao_tipos_entre_enum [not null]
"nome" varchar(255) [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Tabela de apoio entre relação entre cidadãos e instituições, sob uma justificação.'
}

Table "republicas" {
"id" bigint [pk, not null]
"uuid" char(36) [not null]
"nome" varchar(100) [not null]
"ano_inicio" smallint [not null]
"ano_fim" smallint [default: NULL]
"sinopse" longtext
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Representa os grandes ciclos históricos desde a Monarquia Constitucional até à atualidade. Exemplo: Primeira República, Estado Novo, Terceira República, etc.'
}

Table "republica_anexos" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"nome" varchar(255) [not null]
"republica_id" bigint [not null]
"anexo_tipo_id" bigint [not null]
"path" varchar(255) [default: NULL, note: 'Caminho em server do arquivo']
"src" varchar(255) [default: NULL, note: 'URL Fonte do arquivo']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Atalhar as nossas procuras à Republica, lançar links externos de interesse, imagens, etc'
}

Table "statuses" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"type" statuses_type_enum [not null]
"name" varchar(100) [not null]
"params" json [not null]
"description" text
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Ref "concelhos_distrito_id_foreign":"distritos"."id" < "concelhos"."distrito_id"

Ref "concelho_anexos_anexo_tipo_id_foreign":"anexo_tipos"."id" < "concelho_anexos"."anexo_tipo_id"

Ref "concelho_anexos_concelho_id_foreign":"concelhos"."id" < "concelho_anexos"."concelho_id"

Ref "distrito_anexos_anexo_tipo_id_foreign":"anexo_tipos"."id" < "distrito_anexos"."anexo_tipo_id"

Ref "distrito_anexos_distrito_id_foreign":"distritos"."id" < "distrito_anexos"."distrito_id"

Ref "freguesias_concelho_id_foreign":"concelhos"."id" < "freguesias"."concelho_id" [delete: cascade]

Ref "freguesias_distrito_id_foreign":"distritos"."id" < "freguesias"."distrito_id" [delete: cascade]

Ref "freguesia_anexos_anexo_tipo_id_foreign":"anexo_tipos"."id" < "freguesia_anexos"."anexo_tipo_id"

Ref "freguesia_anexos_freguesia_id_foreign":"freguesias"."id" < "freguesia_anexos"."freguesia_id"

Ref "legislaturas_republica_id_foreign":"republicas"."id" < "legislaturas"."republica_id"

Ref "legislatura_anexos_anexo_tipo_id_foreign":"anexo_tipos"."id" < "legislatura_anexos"."anexo_tipo_id"

Ref "legislatura_anexos_legislatura_id_foreign":"legislaturas"."id" < "legislatura_anexos"."legislatura_id"

Ref "presidenciais_republica_id_foreign":"republicas"."id" < "presidenciais"."republica_id"

Ref "presidencial_anexos_anexo_tipo_id_foreign":"anexo_tipos"."id" < "presidencial_anexos"."anexo_tipo_id"

Ref "presidencial_anexos_presidencial_id_foreign":"presidenciais"."id" < "presidencial_anexos"."presidencial_id"

Ref "republica_anexos_anexo_tipo_id_foreign":"anexo_tipos"."id" < "republica_anexos"."anexo_tipo_id"

Ref "republica_anexos_republica_id_foreign":"republicas"."id" < "republica_anexos"."republica_id"
```


## CRM
```shell
Enum "user_comentarios_em_enum" {
"users"
"instituicoes"
"cidadaos"
"cidadao_cargos"
"instituicao_legislaturas"
"instituicao_presidenciais"
}

Table "permissions" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"code" varchar(10) [not null]
"name" varchar(100) [not null]
"description" text
"params" json [default: NULL]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "roles" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"code" varchar(10) [not null]
"name" varchar(100) [not null]
"description" text
"params" json [default: NULL]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "role_cargos" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"code" varchar(50) [not null]
"name" varchar(100) [not null]
"description" text
"params" json [default: NULL]
"role_id" bigint [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "role_permissions" {
"id" bigint [pk, not null]
"role_id" bigint [not null]
"permission_id" bigint [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "users" {
"id" bigint [pk, not null]
"uuid" char(36) [not null]
"name" varchar(255) [not null]
"status_id" bigint [not null]
"email" varchar(255) [not null]
"cidadao_id" bigint [default: NULL]
"rgpd" tinyint(1) [not null, default: '1']
"password" varchar(255) [not null]
"email_verified_at" timestamp [default: NULL]
"remember_token" varchar(100) [default: NULL]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "user_comentarios" {
"id" bigint [pk, not null]
"em" user_comentarios_em_enum [not null]
"user_id" bigint
"comentario_id" bigint [not null]
"comentario" text [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Users com permissoes podem deixar comentários em vários objectos|tabelas. por em é a tabela, comentario_id o value da tabela'
}

Table "user_permissions" {
"id" bigint [pk, not null]
"user_id" bigint [not null]
"permission_id" bigint [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "user_roles" {
"id" bigint [pk, not null]
"user_id" bigint [not null]
"role_id" bigint [not null]
"cargo_id" bigint [default: NULL]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Ref "role_cargos_role_id_foreign":"roles"."id" < "role_cargos"."role_id"

Ref "role_permissions_permission_id_foreign":"permissions"."id" < "role_permissions"."permission_id"

Ref "role_permissions_role_id_foreign":"roles"."id" < "role_permissions"."role_id"

Ref "user_permissions_permission_id_foreign":"permissions"."id" < "user_permissions"."permission_id"

Ref "user_permissions_user_id_foreign":"users"."id" < "user_permissions"."user_id"

Ref "user_roles_cargo_id_foreign":"role_cargos"."id" < "user_roles"."cargo_id"

Ref "user_roles_role_id_foreign":"roles"."id" < "user_roles"."role_id"

Ref "user_roles_user_id_foreign":"users"."id" < "user_roles"."user_id"

Ref: "users"."id" < "user_comentarios"."user_id"
```

## Cidadão
```shell
Table "cidadaos" {
"id" bigint [pk, not null]
"uuid" char(36) [not null]
"nome" varchar(255) [not null]
"data_nascimento" date [default: NULL]
"data_falecimento" date [default: NULL]
"freguesia_id" bigint [default: NULL]
"nacional" tinyint(1) [not null, default: '1', note: 'Não queria fazer isto, mas é necessário para até atribuições de medalhas da Republica. Está no Dre...']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "cidadao_anexos" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"nome" varchar(255) [not null]
"cidadao_id" bigint [not null]
"anexo_tipo_id" bigint [not null]
"path" varchar(255) [default: NULL, note: 'Caminho em server do arquivo']
"src" varchar(255) [default: NULL, note: 'URL Fonte do arquivo']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Documentos genéricos para o cidadão, como imagens ou outros links interessantes. não esquecer que deverá ser na tabela cidadao_cargo_anexos que teremos a maioria dos documentos, que são os que são específicos para cada cargo'
}

Table "cidadao_contactos" {
"id" bigint [pk, not null]
"cidadao_id" bigint [not null]
"contacto_tipo_id" bigint [not null]
"contacto" varchar(100) [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "cidadao_dados" {
"id" bigint [pk, not null]
"nif" double [default: NULL]
"cc" double [default: NULL]
"cc_aux" char(5) [default: NULL]
"seg_social" double [default: NULL]
"n_saude" double [default: NULL]
"cidadao_id" bigint [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "cidadao_moradas" {
"id" bigint [pk, not null]
"cidadao_id" bigint [not null]
"morada" text [not null]
"codigo_postal" varchar(10) [default: NULL]
"localidade" varchar(100) [default: NULL]
"concelho_id" bigint [default: NULL]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "cidadao_nacionalidades" {
"id" bigint [pk, not null]
"cidadao_id" bigint [not null]
"nacionalidade_id" bigint [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Ou o cidadao tem o default true, ou tem de herdar multiplas nacionalidades. Tabela para associar cidadao a nacionalidades.'
}

Ref "cidadao_anexos_cidadao_id_foreign":"cidadaos"."id" < "cidadao_anexos"."cidadao_id"

Ref "cidadao_contactos_cidadao_id_foreign":"cidadaos"."id" < "cidadao_contactos"."cidadao_id"

Ref "cidadao_dados_cidadao_id_foreign":"cidadaos"."id" < "cidadao_dados"."cidadao_id"

Ref "cidadao_moradas_cidadao_id_foreign":"cidadaos"."id" < "cidadao_moradas"."cidadao_id"

Ref "cidadao_nacionalidades_cidadao_id_foreign":"cidadaos"."id" < "cidadao_nacionalidades"."cidadao_id"
```

## Instituições
```shell
Table "instituicao_anexos" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"nome" varchar(255) [not null]
"instituicao_id" bigint [not null]
"anexo_tipo_id" bigint [not null]
"path" varchar(255) [default: NULL, note: 'Caminho em server do arquivo']
"src" varchar(255) [default: NULL, note: 'URL Fonte do arquivo']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Documentos genéricos para Instituição; não esquecer que pode ser preferível colocar em instituicao_legislatura_anexos, que são elas que devem estar activas'
}

Table "instituicao_com_ramos" {
"id" bigint [pk, not null]
"instituicao_id" bigint [not null]
"instituicao_ramo_id" bigint [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Talvez este conceito Abstract seja interessante para extender legislativo | executivo | judicial mas ter grandes conceitos, como Cultura, Saude, Educação, etc.'
}

Table "instituicao_contactos" {
"id" bigint [pk, not null]
"instituicao_id" bigint [not null]
"contacto_tipo_id" bigint [not null]
"contacto" text [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "instituicao_dados" {
"id" bigint [pk, not null]
"nif" double [default: NULL]
"certidao_permanente" text
"instituicao_id" bigint [not null]
"descricao" longtext
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "instituicao_legislaturas" {
"id" bigint [pk, not null]
"uuid" char(36) [not null]
"nome" varchar(255) [default: NULL]
"instituicao_id" bigint [not null]
"legislatura_id" bigint [not null]
"data_inicio" date [default: NULL]
"data_fim" date [default: NULL]
"sinopse" text
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Conceito que desmultiplica uma instituição tipo Governo, para um governo especifico, formado devido às eleições de 2023, por exemplo.'
}

Table "instituicao_legislatura_anexos" {
"uuid" varchar(255) [not null]
"nome" varchar(255) [not null]
"instituicao_legislatura_id" bigint [not null]
"anexo_tipo_id" bigint [not null]
"path" varchar(255) [default: NULL, note: 'Caminho em server do arquivo']
"src" varchar(255) [default: NULL, note: 'URL Fonte do arquivo']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Durante os ciclos legislativos, a instituição tem uma nova versão pós formação de orgãos e gera informação'
}

Table "instituicao_leis" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"nome" varchar(255) [not null]
"instituicao_id" bigint [not null]
"lei_id" bigint [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Tabela que mapeará os direitos e deveres de uma instituição, com base nas leis que a regem.'
}

Table "instituicao_moradas" {
"id" bigint [pk, not null]
"instituicao_id" bigint [not null]
"morada" text [not null]
"codigo_postal" varchar(10) [default: NULL]
"localidade" varchar(100) [default: NULL]
"concelho_id" bigint [default: NULL]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "instituicao_nacionalidades" {
"id" bigint [pk, not null]
"instituicao_id" bigint [not null]
"nacionalidade_id" bigint [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Ou a instituicao tem o default true, ou tem de herdar multiplas nacionalidades. Tabela para associar instituicoes a nacionalidades.'
}

Table "instituicao_presidenciais" {
"id" bigint [pk, not null]
"uuid" char(36) [not null]
"nome" varchar(255) [default: NULL]
"instituicao_id" bigint [not null]
"presidencial_id" bigint [not null]
"data_inicio" date [default: NULL]
"data_fim" date [default: NULL]
"sinopse" text
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Como as instituicao_legislativas, algumas instituições, como o conselho de estado mudam com as eleições do presidente .'
}

Table "instituicao_presidencial_anexos" {
"uuid" varchar(255) [not null]
"nome" varchar(255) [not null]
"instituicao_presidencial_id" bigint [not null]
"anexo_tipo_id" bigint [not null]
"path" varchar(255) [default: NULL, note: 'Caminho em server do arquivo']
"src" varchar(255) [default: NULL, note: 'URL Fonte do arquivo']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "instituicao_ramos" {
"id" bigint [pk, not null]
"uuid" char(36) [not null]
"tipo" varchar(255) [not null]
"descricao" text [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Odeio o Nome desta tabela, é uum conceito teorico, que vai ajudar a filtrar as instituições. Inicialmente para mapear o ramo legislativo | executivo | judicial mas a complexidade de ONGs também vai entrar em conta.'
}

Table "instituicoes" {
"id" bigint [pk, not null]
"uuid" char(36) [not null]
"republica_id" bigint [not null, note: 'Inicio da instituição, algumas daquele regime para a frente. Ex Comissão Nacional de Eleições']
"nome" varchar(255) [not null]
"sigla" varchar(255) [default: NULL]
"sinopse" text
"responde_instituicao_id" bigint [default: NULL]
"entidade_juridica_id" bigint [default: NULL]
"nacional" tinyint(1) [not null, default: '1', note: 'Queria fazer isto, Vamos a ver que OGNs é que vão aparecer']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Ref "instituicao_anexos_instituicao_id_foreign":"instituicoes"."id" < "instituicao_anexos"."instituicao_id"

Ref "instituicao_com_ramos_instituicao_id_foreign":"instituicoes"."id" < "instituicao_com_ramos"."instituicao_id"

Ref "instituicao_com_ramos_instituicao_ramo_id_foreign":"instituicao_ramos"."id" < "instituicao_com_ramos"."instituicao_ramo_id"

Ref "instituicao_contactos_instituicao_id_foreign":"instituicoes"."id" < "instituicao_contactos"."instituicao_id"

Ref "instituicao_dados_instituicao_id_foreign":"instituicoes"."id" < "instituicao_dados"."instituicao_id"

Ref "instituicao_legislaturas_instituicao_id_foreign":"instituicoes"."id" < "instituicao_legislaturas"."instituicao_id"

Ref "fk_legislatura_id":"instituicao_legislaturas"."id" < "instituicao_legislatura_anexos"."instituicao_legislatura_id"

Ref "instituicao_moradas_instituicao_id_foreign":"instituicoes"."id" < "instituicao_moradas"."instituicao_id"

Ref "instituicao_nacionalidades_instituicao_id_foreign":"instituicoes"."id" < "instituicao_nacionalidades"."instituicao_id"

Ref "instituicao_presidenciais_instituicao_id_foreign":"instituicoes"."id" < "instituicao_presidenciais"."instituicao_id"

Ref "fk_presidencial_id":"instituicao_presidenciais"."id" < "instituicao_presidencial_anexos"."instituicao_presidencial_id"

Ref "instituicoes_responde_instituicao_id_foreign":"instituicoes"."id" < "instituicoes"."responde_instituicao_id"
```

## Leis
```shell
Enum "lei_emissores_emissor_tipo_enum" {
"instituicao"
"instituicao_legislatura"
"cidadao"
}

Table "diario_republicas" {
"id" bigint [pk, not null]
"uuid" char(36) [not null]
"nome" varchar(255) [not null]
"publicacao" date [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Tabela que aceita uma data unica para publicação. 1 Diário da republica pode ter vários diario_republicas_publicacoes. Exemplo nome: Diário da República n.º 32/2025'
}

Table "diario_republica_glossarios" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"nome" varchar(255) [not null, note: 'Nome do glossário']
"texto" longtext [note: 'Texto da lei, extraido da web']
"path" varchar(255) [not null, note: 'Caminho em server do arquivo']
"src" varchar(255) [not null, note: 'URL Fonte do arquivo']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "diario_republica_publicacao_leis" {
"id" bigint [pk, not null]
"dr_publicacao_id" bigint [not null]
"lei_id" bigint [not null]
"src" text [note: 'Web link']
"paginas" varchar(255) [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "diario_republica_publicacoes" {
"id" bigint [pk, not null]
"uuid" char(36) [not null]
"nome" varchar(255) [not null]
"src" text [note: 'Web link']
"diario_republica_id" bigint [not null]
"serie_id" bigint [not null, note: 'Série do Diário da República']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Tabela complexa, que representa uma publicacao do diário da republica, que pode ter mais de uma série. Exemplo nome: Diário da República n.º 32/2025, Série I de 2025-02-14; Exemplo src: https://diariodarepublica.pt/dr/detalhe/diario-republica/32-2025-907468769'
}

Table "diario_republica_series" {
"id" bigint [pk, not null]
"nome" varchar(255) [not null]
"sinopse" text [not null]
"serie_id" bigint [default: NULL, note: 'As séries poderão ter suplementos, dentro de cada série, havemos de mapear também os suplementos']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Dentro do Dre, cada publicacao vem dentro de series. Cada serie tem um proposito diferente.'
}

Table "leis" {
"id" bigint [pk, not null]
"uuid" char(36) [not null]
"codigo" varchar(255) [not null]
"nome_completo" varchar(255) [not null]
"proponente" varchar(255) [default: NULL, note: 'Caso não se conheça o emissor fica a string extraída']
"sumario" text [note: 'Resumo da lei, extraido da web']
"texto" longtext [note: 'Texto da lei, extraido da web']
"path" longtext [note: 'Caminho para path o ficheiro da lei']
"em_vigor" tinyint(1) [not null, default: '1']
"data_toggle" date [default: NULL, note: 'Leis começam activas, mas mudam para outros quando são revogadas ou substituídas']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "lei_adendas" {
"id" bigint [pk, not null]
"lei_original_id" bigint [not null]
"lei_adenda_id" bigint [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
} 

Table "lei_anexos" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"nome" varchar(255) [not null]
"lei_id" bigint [not null]
"anexo_tipo_id" bigint [not null]
"path" varchar(255) [default: NULL, note: 'Caminho em server do arquivo']
"src" varchar(255) [default: NULL, note: 'URL Fonte do arquivo']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "lei_emissores" {
"id" bigint [pk, not null]
"lei_id" bigint [not null]
"emissor_tipo" lei_emissores_emissor_tipo_enum [not null]
"emissor_id" bigint [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Ref "diario_republica_publicacao_leis_dr_publicacao_id_foreign":"diario_republica_publicacoes"."id" < "diario_republica_publicacao_leis"."dr_publicacao_id"

Ref "diario_republica_publicacao_leis_lei_id_foreign":"leis"."id" < "diario_republica_publicacao_leis"."lei_id"

Ref "diario_republica_publicacoes_diario_republica_id_foreign":"diario_republicas"."id" < "diario_republica_publicacoes"."diario_republica_id"

Ref "diario_republica_publicacoes_serie_id_foreign":"diario_republica_series"."id" < "diario_republica_publicacoes"."serie_id"

Ref "lei_adendas_lei_adenda_id_foreign":"leis"."id" < "lei_adendas"."lei_adenda_id"

Ref "lei_adendas_lei_original_id_foreign":"leis"."id" < "lei_adendas"."lei_original_id"

Ref "lei_anexos_lei_id_foreign":"leis"."id" < "lei_anexos"."lei_id"

Ref "lei_emissores_lei_id_foreign":"leis"."id" < "lei_emissores"."lei_id"
```

## Complicadas
```shell
Enum "cidadao_relacoes_onde_enum" {
"instituicoes"
"instituicao_legislatura"
"instituicao_presidenciais"
}

Enum "relacao_tipos_entre_enum" {
"cidadaos"
"instituicoes"
"instituicao_cidadao"
}

Table "cidadao_cargos" {
"id" bigint [pk, not null]
"cidadao_id" bigint [not null]
"cargo_id" bigint [not null]
"legislatura_id" bigint [not null]
"inicio" date [not null]
"fim" date [default: NULL]
"src" text
"sinopse" text
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Tabela super importante, que mapeará a carreira de um cidadão, com os cargos que ocupou, em que legislatura, etc.'
}

Table "cidadao_cargo_anexos" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"nome" varchar(255) [not null]
"cidadao_cargo_id" bigint [not null]
"anexo_tipo_id" bigint [not null]
"path" varchar(255) [default: NULL, note: 'Caminho em server do arquivo']
"src" varchar(255) [default: NULL, note: 'URL Fonte do arquivo']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Tentativa de armazenar anexos de cargos de cidadãos, para fiscalização de documentos, não esquecer que deverá ser na tabela cidadao_anexos coisas genéricas à pessoa, aqui ao cargo e nomeação'
}

Table "cidadao_relacoes" {
"id" bigint [pk, not null]
"cidadao_id" bigint [not null]
"com_cidadao_id" bigint [not null]
"relacao_tipo_id" bigint [not null]
"onde" cidadao_relacoes_onde_enum [default: NULL, note: 'Onde se passa a relação']
"onde_id" varchar(255) [default: NULL, note: 'ID da relação']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Tabela super importante para nested search. Neste Portugal nepotista, é importante saber quem relacionado com quem.'
}

Table "entidade_juridicas" {
"id" bigint [pk, not null]
"nome" varchar(255) [not null]
"descricao" text
"params" json [default: NULL]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
}

Table "entidade_juridica_anexos" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"nome" varchar(255) [not null]
"entidades_juridica_id" bigint [not null]
"anexo_tipo_id" bigint [not null]
"path" varchar(255) [default: NULL, note: 'Caminho em server do arquivo']
"src" varchar(255) [default: NULL, note: 'URL Fonte do arquivo']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Atalhar as nossas procuras, lançar links externos de interesse, imagens, etc'
}

Table "entidade_juridica_leis" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"nome" varchar(255) [default: NULL]
"entidade_juridica_id" bigint [not null]
"lei_id" bigint [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Entidade Juridica é um termo legal que é coberto por definição legal. Tabela vai agregando as leis que impactam os direitos e deveres da entidade juridica.'
}

Table "instituicao_cargos" {
"id" bigint [pk, not null]
"uuid" char(36) [not null]
"cargo" varchar(255) [not null]
"instituicao_id" bigint [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Um Presidente da República é um cargo da instituição Presidencia da República, um Primeiro Ministro é um cargo da instituição Governo, um Presidente da Assembleia da Republica é um cargo da instituição Assembleia da Republica, etc.'
}

Table "instituicao_cargo_anexos" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"nome" varchar(255) [not null]
"instituicao_cargo_id" bigint [not null]
"anexo_tipo_id" bigint [not null]
"path" varchar(255) [default: NULL, note: 'Caminho em server do arquivo']
"src" varchar(255) [default: NULL, note: 'URL Fonte do arquivo']
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Documentos genéricos para Instituição Cargo, não esquecer que deverá ser no cidadão que individualmente se colocam os documentos, na tabela cidadao_cargo_anexos'
}

Table "instituicao_cargo_leis" {
"id" bigint [pk, not null]
"uuid" varchar(255) [not null]
"nome" varchar(255) [not null]
"instituicao_cargo_id" bigint [not null]
"lei_id" bigint [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'As instituições criam cargos, mas os cargos devem existir dentro de uma autorização legal. Tabela para associar leis que rondam os direitos e deveres dos cargos.'
}

Table "instituicao_relacoes" {
"id" bigint [pk, not null]
"instituicao_id" bigint [not null]
"com_instituicao_id" bigint [default: NULL]
"com_cidadao_id" bigint [default: NULL]
"relacao_tipo_id" bigint [not null]
"created_at" timestamp [default: NULL]
"updated_at" timestamp [default: NULL]
Note: 'Tabela super importante para nested search. Neste Portugal corrupto, é normal que certas instituições tenham relações de financiamento, nomeacao.'
}

Ref "cidadao_cargo_anexos_cidadao_cargo_id_foreign":"cidadao_cargos"."id" < "cidadao_cargo_anexos"."cidadao_cargo_id"

Ref "entidade_juridica_anexos_entidades_juridica_id_foreign":"entidade_juridicas"."id" < "entidade_juridica_anexos"."entidades_juridica_id"

Ref "entidade_juridica_leis_entidade_juridica_id_foreign":"entidade_juridicas"."id" < "entidade_juridica_leis"."entidade_juridica_id"

Ref "instituicao_cargo_anexos_instituicao_cargo_id_foreign":"instituicao_cargos"."id" < "instituicao_cargo_anexos"."instituicao_cargo_id"

Ref "instituicao_cargo_leis_instituicao_cargo_id_foreign":"instituicao_cargos"."id" < "instituicao_cargo_leis"."instituicao_cargo_id"
```


## Documentação
URL: https://dbdiagram.io/d/Lumen-BD-67a92906263d6cf9a08bcfa8
![[Lumen BD.png]]