# Elastic search

## Co to je?
V informatice název fulltextového vyhledávače vycházejícího z Apache Lucene. Disponuje RESTful rozhraním a nabízí vysokou dostupnost, rychlost a škálovatelnost. Je vyvíjený v Javě a komunikovat s ním lze pomocí webového rozhraní. Je šířen zdarma pod licencí Apache.


Elasticsearch je bezschémovou databází. Není proto třeba definovat strukturu databáze, protože ta se vytvoří sama na základě vložených dat.



## Zakladni vlastnosti
**Data v reálném čase**
Hlavním rysem Elasticsearch je jeho rychlost, díky tomu je možné například na webových stránkách používat filtry a výsledky vyhledávání se zobrazují prakticky okamžitě.

**Distribuovaný systém**
Elasticsearch lze snadno škálovat dle toho, jak se zvyšuje zatížení serveru, na němž běží. Pokud výkon serveru nestačí, stačí přidat další server. Takto vzniklý cluster pak rozloží data optimálně mezi vzniklé uzly.

**Vysoká dostupnost**
Pokud některý z uzlů clusteru vykazuje chybu, Elasticsearch jej detekuje a vyřadí z provozu. Data pak rozdělí mezi zbylé uzly tak, aby byla zajištěna co nejvyšší dostupnost a data zůstala v bezpečí.

**Fulltextové vyhledávání**
Elasticsearch využívá k vyhledávání Apache Lucene, což je patrně nejvýkonnější fulltextové vyhledávání dostupné v rámci open source software. Vyhledávání nabízí podporu více jazyků, vyhledávání na základě geografické polohy, vyhledávání podobných nebo příbuzných záznamů, vyhledávání ve stylu "měli jste na mysli". Lze jej také využít k inteligentnímu automatickému doplňování formulářů na webu.

**RESTful API**
Elastic využívá API - téměř každá akce může být provedena pomocí dokumentu ve formátu JSON, který je zasílán přes HTTP. Pro mnoho programovacích jazyků také existují knihovny zjednodušující práci s Elasticsearch.




## Zakladni pojmy
**Dokument**
je textový soubor, který obsahuje informace, v nichž bude probíhat vyhledávání. V případě Elasticsearch jde konkrétně o soubor formátu JSON. 

**Field**
Dokument je tvořen poli (fields) různých datových typů. V případě Elasticsearch není nutné je definovat předem, Elasticsearch je ve výchozím stavu vytvoří sám na základě struktury dokumentu.

**Index a type**
Dokumenty jsou ukládány do indexu, což je obdoba databázového schématu ve světě relačních databází. Na jeho úrovni je možné nastavovat parametry úložiště společné pro celou sadu dokumentů. V rámci indexu jsou pak definovány typy (type), což označuje skupinu dokumentů obdobného tvaru.

**Cluster**
Skupina pocitacu na kterych bezi Elasticsearch. Elasticsearch je od počátku navržen tak, aby běžel v cloudu. Při produkčním nasazení tak budete pravděpodobně chtít vytvořit cluster - nasadit jej na více serverů, což umožní distribuovat zátěž a zvýšit dostupnost služby.

**Shard**
Fyzicke rozdeleni indexu na vice casti a jeho rozmisteni mezi jednotlive clustery.

**Replika**
Aby bylo zajištěno, že nedojde ke ztrátě dat, jsou k shardům vytvářeny jejich repliky. Pokud tak dojde k výpadku serveru, pravděpodobně se nachází kopie ztracených dat na některém z dalších serverů (nodů), která je ihned využita a automaticky replikována na zdravé servery.



## Analyza textu
### Fulltextové vyhledávání
Nejprve obecně k fulltextovému vyhledávání. Patrně nejznámější způsob, jak "fulltextově" vyhledávat, je použití operátoru LIKE %% v relační databázi. Tento přístup však není bezchybný - nedokáže nalézt všechny tvary slov a navíc ani není dostatečně rychlý.

Předpokládejme dva produkty - Jahody čerstvé a Čerstvá šťáva. Pokud bude uživatel vyhledávat výraz cerstvy, pomocí operátoru LIKE ani jeden z produktů nenalezneme. Slova se v názvech produktů od hledaného výrazu liší tím, že nejsou ve stejném tvaru, mají různou velikost písmen a obsahují diakritiku. 

Pokud by se podařilo jednotlivá slova názvu produktu převést do shodného tvaru a vyhledávání by probíhalo právě v nich, už by byla úspěšnost vyhledávání lepší. Lepší možností by bylo použití operátoru MATCH, ukážeme si ale, jak lze detailně nastavit fulltextové vyhledávání v Elasticsearch.

**Postup**
1. Filtrace znaků (character filters): odstranění nechtěných znaků ze vstupu (html značky nebo interpunkce)

2. Tokenizace (tokenizers): rozdělení vstupního textu na slova (tokeny), zpravidla mezerami

3. Filtrace tokenů (token filters): jde o úpravy nad jednotlivými slovy, může jít o převedení do prvního pádu, odstranění předpon/přípon, diakritiky nebo vypuštění nepodstatných slov

### Indexace
Procesu, kdy z textu vybíráme důležitá slova a ty ukládáme v základním tvaru, aby podle nich bylo možné vyhledávat, se nazývá indexace. Jde o činnost podobnou tvorbě rejstříku v knize. Soubor, ve kterém jsou uloženy termíny, v nichž se vyhledává, se nazývá invertovaný index. Úpravám textu na slova v základním tvaru se pak v kontextu Elasticsearch říká analýza.



## Instalace
1. Instalace Elasticsearch
1.1. Overeni instalace Javy
Pro spousteni Elasticsearch je nutne mit nainstalovanou Java 1.8 a vyssi.

```
java -version
```

1.2. Vytvoreni slozky Apps
V domovske slozce vytvorit slozku Apps pro instalovani vsech potrebnych aplikaci pouzivanych v tomto tutorialu.

```
mkdir ~/Apps
cd ~/Apps
```

1.3. Stazeni Elasticsearch
```
# stažení archivu
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.2.zip

# rozbalení staženého archivu
unzip elasticsearch-5.6.2.zip  
```

1.4. Konfigurace Elasticsearch
Nyní je možné nastavit základní parametry Elasticsearch editací souboru elasticsearch.yml ve složce config. První úpravou je nastavení cluster.name, čímž docílíme toho, že se nebude snažit spuštěný Elasticsearch připojit na jiné nepřejmenované běžící instance Elasticsearch. Výsledná podoba souboru je pak následující:

```
# soubor elasticsearch-5.6.2/config/elasticsearch.yml
cluster.name: elasticsearch-tutorial 
```

1.5. Instalace ceskeho slovniku
Pro správnou funkci češtiny při vyhledávání je třeba nainstalovat český slovník. 

Zkopirovat z git repozitare soubory cs_CZ.aff, cs_CZ.dic a settings.yml, které uložte do složky elasticsearch-5.6.2/config/hunspell/cs_CZ.

```
Git url: https://github.com/ludekvesely/elasticsearch-tutorial/tree/master/hunspell
```

1.6. Instalace pluginu ICU
Posledním doplňkem pro korektní funkčnost češtiny je plugin ICU, který umožňuje správnou práci s kódováním Unicode v českém jazyce. 

```
elasticsearch-5.6.2/bin/elasticsearch-plugin install analysis-icu  
```

2. Instalace Kibana
Kibana je grafické rozhraní, které se umí připojit na Elasticsearch a vizualizovat data, která jsou v něm uložena. Primárním účelem tohoto nástroje je rychlé vyhledávání v uložených datech a vytváření vizualizací (grafů a tabulek) a jejich skládání do komplexních dashboardů. 

Pred instalaci je nutne overit, ze jsme stale v adresari Apps
```
pwd #Melo by vratit ~/Apps
```

```
# stažení
wget https://artifacts.elastic.co/downloads/kibana/kibana-5.6.2-darwin-x86_64.tar.gz

# rozbalení archivu
tar xzf kibana-5.6.2-darwin-x86_64.tar.gz
```

3. Instalae Cerebro
Cerebro umožňuje především správu a monitoring clusteru, tedy zobrazení jeho stavu a úpravu konfigurace. Je však užitečný i pro lokální vývoj, kdy graficky zobrazí všechny dostupné indexy. Lepší možnosti monitoringu sice poskytuje X-pack, který jde do Kibany doinstalovat, je však placený, Cerebro je kompletně zdarma.

```
# stažení archivu
wget https://github.com/lmenezes/cerebro/releases/download/v0.6.6/cerebro-0.6.8.zip

# rozbalení archivu
unzip cerebro-0.6.8.zip
```

## Spousteni
```
# Elasticsearch (http://localhost:9200)
~/Apps/elasticsearch-5.6.2/bin/elasticsearch  

# Kibana (http://localhost:5601)
~/Apps/kibana-5.6.2-darwin-x86_64/bin/kibana  

# Cerebro(http://localhost:9000)
~/Apps/cerebro-0.6.8/bin/cerebro 

```



## Vytvoreni zakladni struktury pro full-text hledani
Ukol: full-textove vyhledavani nad obedovym menu

```
DELETE lunch_menu ;

# Vytvoreni indexu s full-textem
PUT lunch_menu  
{
  "settings": {
    "index": {
      "number_of_shards": "1",
      "number_of_replicas": "0",
      "analysis": {
        "analyzer": {
          "czech": {
            "type": "custom",
            "tokenizer": "standard",
            "filter":
            [
            "czech_stop",
            "czech_length",
            "czech_stemmer",  
            "asciifolding",
            "lowercase"
            ]
          }
        },
        "filter":{
          "czech_stop":{
            "type":"stop",
            "stopwords":["_czech_"]
          },
          "czech_length":{
            "type":"length",
            "min":2
          },
          "czech_stemmer":{
            "type":"stemmer",
            "name":"czech"
          }
        }
      }
    }
  }
}

# Vytvoreni schematu a prirazeni full-textu nad atribut name
PUT lunch_menu/_mapping/lunch_menu
{
  "lunch_menu": {
    "properties":{
      "id":{
        "type": "integer"
      },
      "name":{
        "type": "text",
        "analyzer":"czech"
      },
      "day":{
        "type": "keyword"
      }
    }
  }
}
# Vlozeni dat do indexu
PUT lunch_menu/lunch_menu/1
{
  "id":1,
  "name":"smažený řízek",
  "day":"Pondeli"
}
PUT lunch_menu/lunch_menu/2
{
  "id":2,
  "name":"svíčková na smetaně",
  "day":"Utery"
}
PUT lunch_menu/lunch_menu/3
{
  "id":3,
  "name":"vepřo knedlo zelo",
  "day":"Streda"
}
PUT lunch_menu/lunch_menu/4
{
  "id":4,
  "name":"smažák",
  "day":"Ctvrtek"
}
PUT lunch_menu/lunch_menu/5
{
  "id":5,
  "name":"guláš",
  "day":"Patek"
}

# Vypis vsech jidel
GET lunch_menu/_search
# Vyhledavani (vepřo knedlo zelo)
GET lunch_menu/_search
{
  "query": {
    "match": {
      "name": "vepro knedlo zeli"
    }
  }
}

```

