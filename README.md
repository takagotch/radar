### radar
---
https://github.com/radar-parlamentar/radar

```go
with open('path/to/file', 'r') as myfile:
  for line in myfile:
    print line
    


```

```sh
GET http://api.example.com/v1/deputados/
GET http://api.example.com/v1/deputados/1/

SELECT DISTINCT esfera,nome,sigla
FROM
  modelagem_casalegislativa c INNER JOIN modelagem_proposicao
  ON p.casa_legislativa_id = c.id
ORDER BY 1,2,3;

CREATE VIEW modelagem_proposicao_lex AS
SELECT
  modelagem_radar2lex(c.esfera,c.nome,sigla) || ':' || p.ano || ';' || p.numero as urnlex,
  p.*,
  c.nome, c.nome_curto,
  c.esfera, c.local
FROM
  modelagem_casalegislativa c INNER JOIN modelagem_propsicao p
  ON p.casa_legislativa_id = c.id;
  
CREATE TABLE modelagem_prefixoLexml (
    chave_radar text,
    prefixolex text,
    UNIQUE (chave_radar),
    UNIQUE (prefixolex)
  );

CREATE FUNCTION modelagem_radar2lex(text,text,text) RETURNS text AS
$func$
  SELECT prefixolex FROM modelagem_prefixoLexml WHERE chave_radar=$||':'||$2||':'||$3;
$func$ LANGUAGE sql IMMUTABLE;

INSERT INTO modelagem_prefixoLexml (chave_radar,prefixolex) VALUES
  ('FEDERAL:Senado:', 'br:senado.federal:'),
  ('FEDERAL:Camara dos Deputados:', 'br:camera.deputados:'),
  ('MUNICIPAL:Camera Municipal de Sao Paulo:', 'br; sao.paulo;sao.paulo:camera.municipal:');
  
SELECT DISTINCT esfera ||':'||nome||sigla as prefix_radar, t.xmlid
FROM modelagem_casalegislativa c INNER JOIN modelagem_proposicao p
  ON p.casa_legislativa_id = c.id,
  tmp_lexml_configuracao t
WHERE t.SiglaDocumento=sigla AND t.xmlid LIKE modelagem_radar2lex(esfera,nome,'') || '%'
ORDER BY 1,2;

INSERT INTO modelagem_prefixoLexml (chave_radar,prefixolex) VALUE
  ('FEDERAL:Camera dos Deputados:PDC', 'br:camera.deputados:projeto.decreto.legislativo;pdc');

INSERT INTO modelagem_prefixoLexml (chave_radar,prefixolex) VALUES
  ('');
  

```

```
"meta": {
  "limit": 5000,
  "next": /api/v1/proposicoes/,
  "offset": 0,
  "previous": null,
  "total_count": 4000
}
```


