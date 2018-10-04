# Elasticsearch

#### 1. Verdades:

* É um servidor de buscas baseado no Lucene
* Full text search
* Permite análise de dados em tempo real
* Trabalha com dados desnormalizados
* Escalável
* Bigdata

#### 2. Como instalar?

[Elasticsearch](https://drive.google.com/open?id=1nCe6mDYdjYeCzzNr8kv4Fr68_UaZp2Xv)

tar -vzxf elasticsearch-x.tar.gz

./bin/elasticsearch

porta http: 9200

[Kibana](https://drive.google.com/open?id=1Lc9xEuHHuLuoSNnrEZReboSTh_gsZwyX)

tar -vzxf kibana-x.tar.gz

./bin/kibana

porta http: 5601


#### 3. Salvando nosso primeiro produto:

##### Criando um índice
PUT /produto

POST /produto/default/1
```javascript
{
	"nome":"Tênis Nikessss",
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

PUT /produto/default/1
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

POST /produto/default/2
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

POST /produto/default/3
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

POST /produto/default/4
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

POST /produto/default/5
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

POST /produto/default/6
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

GET /produto/default/_search

#### 5. Fazendo algumas buscas

GET produto/default/_search?q=ean:E4Dj35D

GET produto/default/_search?q=nome:Raquete

GET produto/default/_search?q=marca:Adidas

GET produto/default/_search?q=modelo:Para Surdos

GET produto/default/_search?q=lojistas.nome:Gaucho Pinkstore

GET produto/default/_search?q=Raquete

GET produto/default/_search?q=raquetes

![](https://github.com/progtiago/talks/blob/master/elastic/indice.jpg)

#### 6. Vendo o mapeamento da collection

GET produto/default/_mapping

#### 7. Alguns problemas

GET produto/default/_search?q=arcoiro

GET produto/default/_search?q=raquetes

GET produto/default/_search?q=cor:amarela

#### 8. Analizers


POST _analyze
```javascript
{
  "analyzer": "portuguese",
  "text":     "O Código bonito, código formoso, código bem feito!"
}
```

POST _analyze
```javascript
{
  "analyzer": "whitespace",
  "text":     "O Código bonito, código formoso, código bem feito!"
}
```

POST _analyze
```javascript
{
  "analyzer": "standard",
  "text":     "O Código bonito, código formoso, código bem feito!"
}
```

##### Estrutura dos analyzers

![](https://github.com/progtiago/talks/blob/master/elastic/analyzers.png)

[Character Filters](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-charfilters.html)

[Tokenizers](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-tokenizers.html)

[Token Filters](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-tokenfilters.html)

##### Customizando analyzers

POST _analyze
```javascript
{
  "analyzer": "standard",
  "text":     "<html><h1>Código bonito, código formoso, código bem feito!</h1></html>"
}
```
PUT testando_meu_proprio_analyzer
```javascript
{
  "settings": {
    "analysis": {
      "analyzer": {
        "my_custom_analyzer": {
          "type": "custom", 
          "tokenizer": "standard",
          "char_filter": [
            "html_strip"
          ],
          "filter": [
            "uppercase"
          ]
        }
      }
    }
  }
}
```

POST testando_meu_proprio_analyzer/_analyze
```javascript
{
  "analyzer": "my_custom_analyzer",
  "text":     "<html><h1>Código bonito, código formoso, código bem feito!</h1></html>"
}
```

#### 9. Mapeando um index

PUT /produto
```javascript
{
  "mappings": {
    "default": {
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
          "analyzer": "portuguese"
        },
        "cor": {
          "fields": {
            "original": {
              "type": "keyword"
            }
          },
          "type": "text",
          "analyzer": "portuguese",
          "search_analyzer": "standard"
        },
        "marca": {
          "fields": {
            "original": {
              "type": "keyword"
            }
          },
          "type": "text",
          "analyzer": "portuguese"
        },
        "modelo": {
          "fields": {
            "original": {
              "type": "keyword"
            }
          },
          "type": "text",
          "analyzer": "portuguese"
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
            "analyzer": "portuguese"
            }
          }
        }
      }
    }
  }
}
```


#### 10. Cadastrando produtos

POST /produto/default/1
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

POST /produto/default/2
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

POST /produto/default/3
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

POST /produto/default/4
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

POST /produto/default/5
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

POST /produto/default/6
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

GET produto/default/_search?q=raquetes

GET produto/default/_search?q=cor:amarela

GET produto/default/_search?q=arcoiro

#### 12. Criando sinônimos

PUT /produto
```javascript
{
  "settings": {
    "analysis": {
      "filter": {
        "portuguese_stemmer": {
          "type":       "stemmer",
          "language":   "light_portuguese"
        },
        "filtro_de_sinonimos": {
            "type": "synonym",
            "synonyms": ["raquete,arcoiro"]
        }
      },
      "analyzer": {
        "sinonimos": {
          "tokenizer":  "standard",
          "filter": [
            "lowercase",
            "filtro_de_sinonimos",
            "portuguese_stemmer"
          ]
        }
      }
    }
  },
  "mappings": {
    "default": {
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
          "analyzer": "sinonimos"
        },
        "cor": {
          "fields": {
            "original": {
              "type": "keyword"
            }
          },
          "type": "text",
          "analyzer": "portuguese"
        },
        "marca": {
          "fields": {
            "original": {
              "type": "keyword"
            }
          },
          "type": "text",
          "analyzer": "portuguese"
        },
        "modelo": {
          "fields": {
            "original": {
              "type": "keyword"
            }
          },
          "type": "text",
          "analyzer": "portuguese"
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
            "analyzer": "portuguese"
            }
          }
        }
      }
    }
  }
}
```

GET /produto/_analyze
```javascript
{
  "analyzer": "sinonimos",
  "text": "arcoiro"
}
```
POST /produto/default/1
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

POST /produto/default/2
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

POST /produto/default/3
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

POST /produto/default/4
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

POST /produto/default/5
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

POST /produto/default/6
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

GET produto/default/_search?q=raquetes

GET produto/default/_search?q=cor:amarela

GET produto/default/_search?q=arcoiro


#### 13. Query DSL

GET /produto/default/_search
```javascript
{
  "query": {
    "match":{
      "nome": "arcoiro"
    }
  }
}
```

GET /produto/default/_search
```javascript
{
  "query": {
    "function_score": {
      "min_score": 2,
      "score_mode": "sum",
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

#### 14. Bulk API

curl -H "Content-Type: application/json" -XPOST "http://localhost:9200/product/default/_bulk?pretty" --data-binary "@test-data.json"
