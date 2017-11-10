# Elasticsearch

#### 1. Verdades:

* É um servidor de buscas baseado no Lucene
* Full text search
* Permite análise de dados em tempo real
* Trabalha com dados desnormalizados
* Escalável
* Bigdata

#### 2. Como instalar?

[Elasticsearch](https://drive.google.com/open?id=1oAjyOFQZGWfM7n5u9-DcZLfHjx9CpKcB)

tar -vzxf elasticsearch-x.tar.gz

./bin/elasticsearch

porta http: 9200

[Kibana](https://drive.google.com/open?id=1a04jDzrWHZNA_6sslsQfCtV59pkUPCGT)

tar -vzxf kibana-x.tar.gz

./bin/kibana

porta http: 5601


#### 3. Salvando nosso primeiro produto:

POST /catalogo/produtos/

POST /catalogo/produtos/1

PUT /catalogo/produtos/1

```javascript
{
	"nome":"Tênis Nike",
	"cor":"Preto+Azul",
	"marca":"Nike",
	"modelo":"modelo do caipira",
	"ean":"D4D435D",
	"lojistas":[{
		"nome":"Macau BigStore",
		"codigo":"10"
	}]
}
```

POST /catalogo/produtos/2
```javascript
{
	"nome":"Raquete de Tênis",
	"cor":"Amarelo",
	"marca":"Raquetol",
	"modelo":"Raquete do Guga",
	"ean":"E4D835D",
	"lojistas":[{
		"nome":"Gaucho PinkStore",
		"codigo":"24"
	}]
}
```

POST /catalogo/produtos/3
```javascript
{
	"nome":"Capa de Chuva",
	"cor":"Cinza",
	"marca":"Molha tudo",
	"modelo":"Modelo para gordos",
	"ean":"E4D835Y",
	"lojistas":[{
		"nome":"Seu Boneco Qualidade",
		"codigo":"12"
	}]
}
```

POST /catalogo/produtos/4
```javascript
{
	"nome":"Carregador de Celular",
	"cor":"Rosa Pink",
	"marca":"China equipamentos",
	"modelo":"Kebra Cellphone",
	"ean":"E4D835L",
	"lojistas":[{
		"nome":"Seu Boneco Qualidade",
		"codigo":"12"
	}]
}
```

POST /catalogo/produtos/5
```javascript
{
	"nome":"Áudio book",
	"cor":"Preto+Vermelho+Branco+Lilás",
	"marca":"Surdo Mania",
	"modelo":"Para Surdos",
	"ean":"E4Dj35D",
	"lojistas":[{
		"nome":"Gaucho PinkStore",
		"codigo":"24"
	}]
}
```

POST /catalogo/produtos/6
```javascript
{
	"nome":"Meia Adidas",
	"cor":"Vermelho+Rosa+Amarelo",
	"marca":"Adidas",
	"modelo":"meia para descalços",
	"ean":"D4D4350",
	"lojistas":[{
		"nome":"Seu Boneco Qualidade",
		"codigo":"12"
	}]
}
```

#### 4. Visualizando todos os produtos

GET /catalogo/produtos/_search

#### 5. Fazendo algumas buscas

GET catalogo/produtos/_search?q=ean:E4Dj35D

GET catalogo/produtos/_search?q=nome:Raquete

GET catalogo/produtos/_search?q=marca:Adidas

GET catalogo/produtos/_search?q=modelo:Para Surdos

GET catalogo/produtos/_search?q=lojistas.nome:Gaucho Pinkstore

GET catalogo/produtos/_search?q=Raquete

GET catalogo/produtos/_search?q=raquetes


#### 6. Vendo o mapeamento da collection

GET catalogo/produtos/_mapping

#### 7. Alguns problemas

GET produtos/v1/_search?q=arcoiro

GET produtos/v1/_search?q=raquetes

GET produtos/v1/_search?q=cor:amarela

#### 8. Analizers:

```javascript
POST _analyze
{
  "analyzer": "portuguese",
  "text":     "O Código bonito, código formoso, código bem feito"
}
```

POST _analyze
```javascript
{
  "analyzer": "whitespace",
  "text":     "O Código bonito, código formoso, código bem feito"
}
```

POST _analyze
```javascript
{
  "analyzer": "standard",
  "text":     "O Código bonito, código formoso, código bem feito"
}
```

#### 9. Mapeando um index

PUT catalogo2/
```javascript
{
  "settings" : {
        "number_of_shards" : 1
    },
  "mappings": {
    "produtos": {
      "_all": {
        "type": "text",
        "analyzer": "portuguese"
      },
      "properties": {
        "ean": {
          "type": "text"
        },
        "nome": {
          "fields": {
            "original": {
              "type": "keyword"
            }
          },
          "type": "text",
          "analyzer": "portuguese",
          "search_analyzer": "portuguese"
        },
        "cor": {
          "fields": {
            "original": {
              "type": "keyword"
            }
          },
          "type": "text",
          "analyzer": "portuguese",
          "search_analyzer": "portuguese"
        },
        "marca": {
          "fields": {
            "original": {
              "type": "keyword"
            }
          },
          "type": "text",
          "analyzer": "portuguese",
          "search_analyzer": "portuguese"
        },
        "modelo": {
          "fields": {
            "original": {
              "type": "keyword"
            }
          },
          "type": "text",
          "analyzer": "portuguese",
          "search_analyzer": "portuguese"
        },
        "lojistas": {
          "properties": {
            "codigo": {
              "type":"text"
            },
            "nome": {
              "fields": {
                "original": {
                  "type": "keyword"
                }
              },
            "type": "text",
            "analyzer": "portuguese",
            "search_analyzer": "portuguese"
            }
          }
        }
      }
    }
  }
}
```

#### 10. Cadastrando produtos

POST /catalogo2/produtos/1
```javascript
{
	"nome":"Tênis Nike",
	"cor":"Preto+Azul",
	"marca":"Nike",
	"modelo":"modelo do caipira",
	"ean":"D4D435D",
	"lojistas":[{
		"nome":"Macau BigStore",
		"codigo":"10"
	}]
}
```

POST /catalogo2/produtos/2
```javascript
{
	"nome":"Raquete de Tênis",
	"cor":"Amarelo",
	"marca":"Raquetol",
	"modelo":"Raquete do Guga",
	"ean":"E4D835D",
	"lojistas":[{
		"nome":"Gaucho PinkStore",
		"codigo":"24"
	}]
}
```

POST /catalogo2/produtos/3
```javascript
{
	"nome":"Capa de Chuva",
	"cor":"Cinza",
	"marca":"Molha tudo",
	"modelo":"Modelo para gordos",
	"ean":"E4D835Y",
	"lojistas":[{
		"nome":"Seu Boneco Qualidade",
		"codigo":"12"
	}]
}
```

POST /catalogo2/produtos/4
```javascript
{
	"nome":"Carregador de Celular",
	"cor":"Rosa Pink",
	"marca":"China equipamentos",
	"modelo":"Kebra Cellphone",
	"ean":"E4D835L",
	"lojistas":[{
		"nome":"Seu Boneco Qualidade",
		"codigo":"12"
	}]
}
```

POST /catalogo2/produtos/5
```javascript
{
	"nome":"Áudio book",
	"cor":"Preto+Vermelho+Branco+Lilás",
	"marca":"Surdo Mania",
	"modelo":"Para Surdos",
	"ean":"E4Dj35D",
	"lojistas":[{
		"nome":"Gaucho PinkStore",
		"codigo":"24"
	}]
}
```

POST /catalogo2/produtos/6
```javascript
{
	"nome":"Meia Adidas",
	"cor":"Vermelho+Rosa+Amarelo",
	"marca":"Adidas",
	"modelo":"meia para descalços",
	"ean":"D4D4350",
	"lojistas":[{
		"nome":"Seu Boneco Qualidade",
		"codigo":"12"
	}]
}
```

#### 11. E agora?

GET produtos/v1/_search?q=raquetes

GET produtos/v1/_search?q=cor:amarela

GET produtos/v1/_search?q=arcoiro

#### 12. Criando sinônimos

PUT /catalogo3
```javascript
{
  "settings": {
    "index": {
      "number_of_shards": 1,
      "number_of_replicas": 0
    },
    "analysis": {
      "filter": {
        "portuguese_stop": {
          "type":       "stop",
          "stopwords":  "_portuguese_" 
        },
        "portuguese_keywords": {
          "type":       "keyword_marker",
          "keywords":   ["exemplo"] 
        },
        "portuguese_stemmer": {
          "type":       "stemmer",
          "language":   "light_portuguese"
        },
        "filtro_de_sinonimos": {
            "type": "synonym",
            "synonyms": [
                "raquete,arcoiro"
            ]
        }
      },
      "analyzer": {
        "sinonimos": {
          "tokenizer":  "standard",
          "filter": [
            "filtro_de_sinonimos",
            "lowercase",
            "portuguese_stop",
            "portuguese_keywords",
            "portuguese_stemmer"
          ]
        }
      }
    }
  },
  "mappings": {
    "produtos": {
      "_all": {
        "type": "text",
        "analyzer": "sinonimos",
        "search_analyzer": "sinonimos"
      },
      "properties": {
        "ean": {
          "type": "text"
        },
        "nome": {
          "fields": {
            "original": {
              "type": "keyword"
            }
          },
          "type": "text",
          "analyzer": "portuguese",
          "search_analyzer": "portuguese"
        },
        "cor": {
          "fields": {
            "original": {
              "type": "keyword"
            }
          },
          "type": "text",
          "analyzer": "portuguese",
          "search_analyzer": "portuguese"
        },
        "marca": {
          "fields": {
            "original": {
              "type": "keyword"
            }
          },
          "type": "text",
          "analyzer": "portuguese",
          "search_analyzer": "portuguese"
        },
        "modelo": {
          "fields": {
            "original": {
              "type": "keyword"
            }
          },
          "type": "text",
          "analyzer": "portuguese",
          "search_analyzer": "portuguese"
        },
        "lojistas": {
          "properties": {
            "codigo": {
              "type":"text"
            },
            "nome": {
              "fields": {
                "original": {
                  "type": "keyword"
                }
              },
            "type": "text",
            "analyzer": "portuguese",
            "search_analyzer": "portuguese"
            }
          }
        }
      }
    }
  }
}
```

POST /catalogo3/_analyze
```javascript
{
  "analyzer": "sinonimos",
  "text": "arcoiro"
}
```
POST /catalogo3/produtos/1
```javascript
{
	"nome":"Tênis Nike",
	"cor":"Preto+Azul",
	"marca":"Nike",
	"modelo":"modelo do caipira",
	"ean":"D4D435D",
	"lojistas":[{
		"nome":"Macau BigStore",
		"codigo":"10"
	}]
}
```
POST /catalogo3/produtos/2
```javascript
{
	"nome":"Raquete de Tênis",
	"cor":"Amarelo",
	"marca":"Raquetol",
	"modelo":"Raquete do Guga",
	"ean":"E4D835D",
	"lojistas":[{
		"nome":"Gaucho PinkStore",
		"codigo":"24"
	}]
}
```

POST /catalogo3/produtos/3
```javascript
{
	"nome":"Capa de Chuva",
	"cor":"Cinza",
	"marca":"Molha tudo",
	"modelo":"Modelo para gordos",
	"ean":"E4D835Y",
	"lojistas":[{
		"nome":"Seu Boneco Qualidade",
		"codigo":"12"
	}]
}
```

POST /catalogo3/produtos/4
```javascript
{
	"nome":"Carregador de Celular",
	"cor":"Rosa Pink",
	"marca":"China equipamentos",
	"modelo":"Kebra Cellphone",
	"ean":"E4D835L",
	"lojistas":[{
		"nome":"Seu Boneco Qualidade",
		"codigo":"12"
	}]
}
```

POST /catalogo3/produtos/5
```javascript
{
	"nome":"Áudio book",
	"cor":"Preto+Vermelho+Branco+Lilás",
	"marca":"Surdo Mania",
	"modelo":"Para Surdos",
	"ean":"E4Dj35D",
	"lojistas":[{
		"nome":"Gaucho PinkStore",
		"codigo":"24"
	}]
}
```

POST /catalogo3/produtos/6
```javascript
{
	"nome":"Meia Adidas",
	"cor":"Vermelho+Rosa+Amarelo",
	"marca":"Adidas",
	"modelo":"meia para descalços",
	"ean":"D4D4350",
	"lojistas":[{
		"nome":"Seu Boneco Qualidade",
		"codigo":"12"
	}]
}
```

GET catalogo3/produtos/_search?q=arcoiro

GET catalogo3/produtos/_search?q=raquetes

GET catalogo3/produtos/_search?q=cor:amarela

#### 13. DLS Query

```javascript
GET /catalogo3/produtos/_search
{
  "query": {
    "function_score": {
      "min_score": 2,
      "score_mode": "sum",
      "query": {},
      "functions": [
        {
          "filter": {
            "match": {
              "ean": "D4D4350"
            }
          },
          "weight": 1
        },
        {
          "filter": {
            "match": {
              "marca": "mormaii"
            }
          },
          "weight": 3
        },
        {
          "filter": {
            "match": {
              "cor": "amarela"
            }
          },
          "weight": 7
        }
      ]
    }
  }
}
```



