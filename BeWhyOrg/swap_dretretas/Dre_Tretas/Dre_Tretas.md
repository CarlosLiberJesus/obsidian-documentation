# Objetivo
Resumir a estrutura das tabelas que armazenam informações que os nossos anónimos deixaram

## MetaBD
```shell
Table "dreapp_document" {
"id" int [pk, not null]
"doc_type" varchar(128) [default: NULL]
"number" varchar(32) [default: NULL]
"emiting_body" varchar(2048) [default: NULL]
"source" varchar(128) [default: NULL]
"in_force" tinyint(1) [default: NULL]
"conditional" tinyint(1) [default: NULL]
"date" date [default: NULL]
"notes" text
"dre_pdf" varchar(200) [default: NULL]
"timestamp" timestamp [not null]
"dr_number" varchar(16) [default: NULL]
"series" int [default: '1']
"part" varchar(2) [default: '1']
}

Table "dreapp_documenttext" {
"id" int [pk, not null]
"document_id" int [not null]
"timestamp" timestamp [not null]
"text_url" varchar(200) [not null]
"text" longtext
}

Table "dreapp_document_connects_to" {
"id" int [pk, not null]
"from_document_id" int [not null]
"to_document_id" int [not null]
}  

Ref "dreapp_documenttext_document_id_fkey":"dreapp_document"."id" < "dreapp_documenttext"."document_id"

Ref "from_document_id_refs_id_d129da0e":"dreapp_document"."id" < "dreapp_document_connects_to"."from_document_id"

Ref "to_document_id_refs_id_d129da0e":"dreapp_document"."id" < "dreapp_document_connects_to"."to_document_id"
```

## Documentação
URL: https://dbdiagram.io/d/DRe_Tratas-67c1d139263d6cf9a0d21092
![[DRe_Tratas.png]]

## Exemplos Data
```sql
INSERT INTO dreapp_document (id, doc_type, number, emiting_body, source, in_force, conditional, date, notes, dre_pdf, timestamp, dr_number, series, part) VALUES
(1, 'Decreto-Lei', '245/96', 'Ministério da Agricultura, do Desenvolvimento Rural e das Pescas', 'Diário da República n.º 294/1996, Série I-A de 1996-12-20.', 1, 0, '1996-12-20', 'Aprova o regime jurídico aplicável à circulação de gado...', 'https://dre.pt/application/file/147280', '2016-02-15 06:50:26', '294/1996', 1, '1');

INSERT INTO dreapp_documenttext (id, document_id, timestamp, text_url, text) VALUES
(1, 315219, '2014-02-04 14:03:13', 'http://www.dre.pt/cgi/dr2s.exe?t=d2&cap=&doc=2014004130', 'Despacho n.º 1721/2014...');

INSERT INTO dreapp_document_connects_to (id, from_document_id, to_document_id) VALUES
(1, 1, 2);
```

## Índices e Restrições
- Índices: Foram adicionados índices primários a todas as tabelas para otimização de consultas.
- Restrições: Chaves estrangeiras foram estabelecidas para garantir a integridade referencial entre dreapp_documenttext e dreapp_document, assim como em dreapp_document_connects_to.

