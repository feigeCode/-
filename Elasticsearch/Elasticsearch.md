# Elasticsearch简介

[百度百科](https://baike.baidu.com/item/elasticsearch/3411206?fr=aladdin)

Elasticsearch是一个基于[Lucene](https://baike.baidu.com/item/Lucene/6753302)的搜索服务器。它提供了一个分布式多用户能力的[全文搜索引擎](https://baike.baidu.com/item/全文搜索引擎/7847410)，基于RESTful web接口。Elasticsearch是用Java语言开发的，并作为Apache许可条款下的开放源码发布，是一种流行的企业级搜索引擎。Elasticsearch用于[云计算](https://baike.baidu.com/item/云计算/9969353)中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。官方客户端在Java、.NET（C#）、PHP、Python、Apache Groovy、Ruby和许多其他语言中都是可用的。根据DB-Engines的排名显示，Elasticsearch是最受欢迎的企业搜索引擎，其次是Apache Solr，也是基于Lucene。

Elasticsearch是一个实时分布式搜索和分析引擎。它让你以前所未有的速度处理大数据成为可能。
它用于全文搜索、结构化搜索、分析以及将这三者混合使用：

- 维基百科使用Elasticsearch提供全文搜索并高亮关键字，以及输入实时搜索(search-asyou-type)和搜索
	纠错(did-you-mean)等搜索建议功能。
- 英国卫报使用Elasticsearch结合用户日志和社交网络数据提供给他们的编辑以实时的反馈，以便及时了
	解公众对新发表的文章的回应。
- StackOverflow结合全文搜索与地理位置查询，以及more-like-this功能来找到相关的问题和答案。
- Github使用Elasticsearch检索1300亿行的代码。

但是Elasticsearch不仅用于大型企业，它还让像DataDog以及Klout这样的创业公司将最初的想法变成可
扩展的解决方案。
Elasticsearch可以在你的笔记本上运行，也可以在数以百计的服务器上处理PB级别的数据 。
Elasticsearch是一个基于Apache Lucene(TM)的开源搜索引擎。无论在开源还是专有领域，Lucene可以
被认为是迄今为止最先进、性能最好的、功能最全的搜索引擎库。
但是，Lucene只是一个库。想要使用它，你必须使用Java来作为开发语言并将其直接集成到你的应用
中，更糟糕的是，Lucene非常复杂，你需要深入了解检索的相关知识来理解它是如何工作的。
Elasticsearch也使用Java开发并使用Lucene作为其核心来实现所有索引和搜索的功能，但是它的目的是
通过简单的RESTful API来隐藏Lucene的复杂性，从而让全文搜索变得简单。



## 概念

**Near Realtime (NRT)**

Elasticsearch是一个近乎实时的搜索平台。这意味着从索引文档到可以搜索的时间只有轻微的延迟（通常是1秒）。

**Cluster**

集群是一个或多个节点(服务器)的集合，它们共同保存你的整个数据，并提供跨所有节点的联合索引和搜索功能。一个集群由一个唯一的名称标识，默认这个唯一标识的名称是"elasticsearch"。这个名称很重要，因为如果节点被设置为按其名称加入集群，那么节点只能是集群的一部分。

确保不要在不同的环境中用相同的集群名称，否则可能导致节点加入到错误的集群中。例如，你可以使用"logging-dev", "logging-test", "logging-prod"分别用于开发、测试和正式集群的名字。

**Node**

节点是一个单独的服务器，它是集群的一部分，存储数据，并参与集群的索引和搜索功能。就像集群一样，节点由一个名称来标识，默认情况下，该名称是在启动时分配给节点的随机通用唯一标识符(UUID)。如果不想用默认的节点名，可以定义任何想要的节点名。这个名称对于管理来说很重要，因为你希望识别网络中的哪些服务器对应于你的Elasticsearch集群中的哪些节点。

一个节点可以通过配置集群名称来加入到一个特定的集群中。默认情况下，每个节点都被设置加入到一个名字叫"elasticsearch"的集群中，这就意味着如果你启动了很多个节点，并且假设它们彼此可以互相发现，那么它们将自动形成并加入到一个名为"elasticsearch"的集群中。

一个集群可以有任意数量的节点。此外，如果在你的网络上当前没有运行任何节点，那么此时启动一个节点将默认形成一个单节点的名字叫"elasticsearch"的集群。

**Shards & Replicas**

一个索引可能存储大量数据，这些数据可以超过单个节点的硬件限制。例如，一个包含10亿条文档占用1TB磁盘空间的索引可能不适合在单个节点上，或者可能太慢而不能单独处理来自单个节点的搜索请求。

为了解决这个问题，Elasticsearch提供了将你的索引细分为多个碎片（或者叫分片）的能力。在创建索引时，可以简单地定义所需的分片数量。每个分片本身就是一个功能完全独立的“索引”，可以驻留在集群中的任何节点上。

一个分片是一个Lucene索引，一个包含倒排索引的文件目录，倒排索引的结构使 得elasticsearch在不扫描全部文档的情况下，就能告诉你哪些文档包含特定的
关键字。

分片之所以重要，主要有两个原因：

- 它允许你水平地分割/扩展内容卷
- 它允许你跨分片（可能在多个节点上）分布和并行操作，从而提高性能和吞吐量

在一个网络/云环境中随时都有可能出现故障，强烈推荐你有一个容灾机制。Elasticsearch允许你将一个或者多个索引分片复制到其它地方，这被称之为副本。

复制之所以重要，有两个主要原因：

- 它提供了在一个shard/node失败是的高可用性。出于这个原因，很重要的一个点是一个副本从来不会被分配到与它复制的原始分片相同节点上。也就是说，副本是放到另外的节点上的。
- 它允许扩展搜索量/吞吐量，因为搜索可以在所有副本上并行执行。

总而言之，每个索引都可以分割成多个分片。索引也可以被复制零(意味着没有副本)或更多次。一旦被复制，每个索引都将具有主分片(被复制的原始分片)和副本分片(主分片的副本)。在创建索引时，可以为每个索引定义分片和副本的数量。创建索引后，您可以随时动态地更改副本的数量，但不能更改事后分片的数量。

在默认情况下，Elasticsearch中的每个索引都分配了5个主分片和1个副本，这意味着如果集群中至少有两个节点，那么索引将有5个主分片和另外5个副本分片（PS：这5个副本分片组成1个完整副本），每个索引总共有10个分片。

（画外音：副本是针对索引而言的，同时需要注意索引和节点数量没有关系，我们说2个副本指的是索引被复制了2次，而1个索引可能由5个分片组成，那么在这种情况下，集群中的分片数应该是 5 × (1 + 2) = 15 ）



![image-20210824160113851](https://gitee.com/feigeCode/picture/raw/master/img/image-20210824160113851.png)

elasticsearch(集群)中可以包含多个索引(数据库)，每个索引中可以包含多个类型(表)，每个类型下又包
含多 个文档(行)，每个文档中又包含多个字段(列)。



# 倒排索引

[百度百科](https://baike.baidu.com/item/%E5%80%92%E6%8E%92%E7%B4%A2%E5%BC%95/11001569?fr=aladdin)

倒排索引源于实际应用中需要根据属性的值来查找记录。这种索引表中的每一项都包括一个属性值和具有该属性值的各记录的地址。由于不是由记录来确定属性值，而是由属性值来确定记录的位置，因而称为倒排索引(inverted index)。带有倒排索引的文件我们称为倒排[索引文件](https://baike.baidu.com/item/索引文件)，简称[倒排文件](https://baike.baidu.com/item/倒排文件/4137688)(inverted file)。

例：有两个文档 

doc1 **Study every day, good good up to forever** 
doc2 **To forever, study every day, good good up**

|         | doc1 | doc2 |
| :-----: | ---- | ---- |
|  Study  | √    | ×    |
|  every  | √    | √    |
|   day   | √    | √    |
|  good   | √    | √    |
|   up    | √    | √    |
|   to    | √    | ×    |
| forever | √    | √    |
|   To    | ×    | √    |
|  study  | ×    | √    |

现在，我们试图搜索 to forever，只需要查看包含每个词条的文档 score

|         | doc1 | doc2 |
| ------- | ---- | ---- |
| to      | √    | ×    |
| forever | √    | √    |

两个文档都匹配，但是第一个文档比第二个匹配程度更高。如果没有别的条件，现在，这两个包含关键
字的文档都将返回。

# IK分词器插件

分词：即把一段中文或者别的划分成一个个的关键字，我们在搜索时候会把自己的信息进行分词，会把
数据库中或者索引库中的数据进行分词，然后进行一个匹配操作，默认的中文分词是将每个字看成一个
词，不符合我们的要求。

下载地址：https://github.com/medcl/elasticsearch-analysis-ik

下载完成之后在elasticsearch的plugins目录下创建一个ik目录，把把压缩包里的文件解压到ik目录下，重启elasticsearch

可以通过`elasticsearch-plugin list`来查看加载进来的插件

![image-20210825162850448](https://gitee.com/feigeCode/picture/raw/master/img/image-20210825162850448.png)

## kibana做测试

ik_max_word为最细粒度划分！穷尽词库的可能！字典！

~~~json
GET _analyze
{
  "analyzer":"ik_max_word",
  "text":"中华人民共和国"
}
~~~



![image-20210825162216051.png](https://gitee.com/feigeCode/picture/raw/master/img/image-20210825162216051.png)



ik_smart 为最少切分

~~~json
GET _analyze
{
  "analyzer":"ik_smart",
  "text":"中华人民共和国"
}
~~~



![image-20210825163151848](https://gitee.com/feigeCode/picture/raw/master/img/image-20210825163151848.png)



# Rest风格操作elasticsearch



![image-20210825163513759](https://gitee.com/feigeCode/picture/raw/master/img/image-20210825163513759.png)



~~~json
# 创建文档，指定ID
PUT xiaofei/_doc/1
{
  "name":"feige",
  "age":21,
  "tags":["IT","技术宅"]
}

# 通过ID查询
GET xiaofei/_doc/1

# 修改文档
POST xiaofei/_doc/1/_update
{
  "doc":{
    "name":"xiaofei"
  }
}
# 创建文档，随机ID
POST xiaofei/_doc
{
  "name":"feige",
  "age":18,
  "tags":["IT"]
}

# 查询所有
POST xiaofei/_doc/_search



# 通过条件查询
GET xiaofei/_search?q=name:xiaofei&q=age:21

# _source 指定查询的哪些字段，
# sort排序
# 分页from，size和关系型数据库类似从from（第一条是0）开始往后取size条
GET xiaofei/_search
{
  "query": {
    "match": {
      "name": "feige"
    }
  },
  "sort": [
    {
      "age": {
        "order": "desc"
      }
    }
  ], 
  "_source": ["name","tags"],
  "from": 0,
  "size": 2
}
# 类似 where name='feige' and age=21
GET xiaofei/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "feige"
          }
        },
        {
          "match": {
            "age": 21
          }
        }
        ]
    }
  }
}


# 类似 where name='feige' or age=21
GET xiaofei/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "name": "feige"
          }
        },
        {
          "match": {
            "age": 21
          }
        }
        ]
    }
  }
}



# 类似 where name<=>'feige' and age<=>21
GET xiaofei/_search
{
  "query": {
    "bool": {
      "must_not": [
        {
          "match": {
            "name": "xiaofei"
          }
        },
        {
          "match": {
            "age": 21
          }
        }
        ]
    }
  }
}


# 类似 where name='feige' and age between 10 and 21 
# gt大于 gte大于等于 lt小于 lte小于等于
GET xiaofei/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "feige"
          }
        }
        ],
        "filter": [
          {
            "range": {
              "age": {
                "gte": 10,
                "lte": 21
              }
            }
          }
        ]
    }
    }
  }

GET xiaofei/_search
{
  "query": {
    "match": {
      "tags": "技术 IT"
    }
  }
}



# 不分词
GET _analyze
{
  "analyzer":"keyword",
  "text": "中华人民共和国"
}
# 分词
GET _analyze
{
  "analyzer": "standard",
  "text": "中华人民共和国"
}


# 精确匹配
GET xiaofei/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "name": {
              "value": "feige"
            }
          }
        },
        {
          "term": {
            "age": {
              "value": 20
            }
          }
        }
      ]
    }
  }
}
# 高亮搜索
GET xiaofei/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
              "tags": "it"
            }
        },
        {
          "match": {
            "name": "feige"
          }
        }
      ]
    }
  },
  "highlight": {
    "pre_tags": "<p style='color:red;'>", 
    "post_tags": "</p>", 
    "fields": {
      "name": {
        
      },
      "tags": {}
    }
  }
}
~~~

SpringBoot操作elasticsearch

pom.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.11.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.feige</groupId>
    <artifactId>elasticsearch-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>elasticsearch-demo</name>
    <description>elasticsearch-demo</description>
    <properties>
        <java.version>1.8</java.version>
        <elasticsearch.version>7.6.1</elasticsearch.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

~~~



~~~java
package com.feige;


import org.elasticsearch.action.admin.indices.delete.DeleteIndexRequest;
import org.elasticsearch.action.bulk.BulkRequest;
import org.elasticsearch.action.bulk.BulkResponse;
import org.elasticsearch.action.delete.DeleteRequest;
import org.elasticsearch.action.delete.DeleteResponse;
import org.elasticsearch.action.get.GetRequest;
import org.elasticsearch.action.get.GetResponse;
import org.elasticsearch.action.index.IndexRequest;
import org.elasticsearch.action.index.IndexResponse;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.action.support.master.AcknowledgedResponse;
import org.elasticsearch.action.update.UpdateRequest;
import org.elasticsearch.action.update.UpdateResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.client.RestHighLevelClient;
import org.elasticsearch.client.indices.CreateIndexRequest;
import org.elasticsearch.client.indices.CreateIndexResponse;
import org.elasticsearch.client.indices.GetIndexRequest;
import org.elasticsearch.common.unit.TimeValue;
import org.elasticsearch.common.xcontent.XContentType;
import org.elasticsearch.index.query.MatchAllQueryBuilder;
import org.elasticsearch.index.query.MatchQueryBuilder;
import org.elasticsearch.index.query.QueryBuilder;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.elasticsearch.search.fetch.subphase.highlight.HighlightBuilder;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.test.context.SpringBootTest;

import java.io.IOException;
import java.util.HashMap;

@SpringBootTest
class ElasticsearchDemoApplicationTests {

    @Autowired
    @Qualifier("elasticsearchRestHighLevelClient")
    private RestHighLevelClient restHighLevelClient;

    /**
     * @description: 创建索引
     * @author: feige
     * @date: 2021/8/25 17:44
     * @param
     * @return: void
     */
    @Test
    void indexCreate() throws IOException {
        CreateIndexRequest request = new CreateIndexRequest("xiaofei");
        CreateIndexResponse response = restHighLevelClient.indices().create(request, RequestOptions.DEFAULT);
        System.out.println(response.isAcknowledged());
    }
    /**
    * @description: 判断索引是否存在
    * @author: feige
    * @date: 2021/8/25 17:35
    * @param
    * @return: void
    */
    @Test
    public void indexExists() throws IOException {
        GetIndexRequest request = new GetIndexRequest("xiaofei");
        boolean exists = restHighLevelClient.indices().exists(request, RequestOptions.DEFAULT);
        System.out.println(exists);
    }

    /**
     * @description: 删除索引
     * @author: feige
     * @date: 2021/8/25 17:35
     * @param
     * @return: void
     */
    @Test
    public void indexDelete() throws IOException {
        DeleteIndexRequest request = new DeleteIndexRequest("xiaofei");
        AcknowledgedResponse delete = restHighLevelClient.indices().delete(request, RequestOptions.DEFAULT);
        System.out.println(delete.isAcknowledged());
    }

    /**
     * @description: 添加文档
     * @author: feige
     * @date: 2021/8/25 17:45
     * @param
     * @return: void
     */
    @Test
    void addDocument() throws IOException {
        HashMap<String, Object> map = new HashMap<>();
        map.put("name","feige");
        map.put("age",21);
        map.put("introduction","正在学elasticsearch");
        IndexRequest request = new IndexRequest("xiaofei")
                .id("1")
                .source(map, XContentType.JSON)
                .timeout(TimeValue.timeValueSeconds(1));
        IndexResponse response = restHighLevelClient.index(request, RequestOptions.DEFAULT);
        System.out.println(response.toString());
    }

   /**
    * @description: 获取文档，判断是否存在 get /index/doc/1
    * @author: feige
    * @date: 2021/8/25 17:49
    * @param
    * @return: void
    */
    @Test
    public void existsDocument() throws IOException {
        GetRequest request = new GetRequest("xiaofei", "1");
        boolean exists = restHighLevelClient.exists(request, RequestOptions.DEFAULT);
        System.out.println(exists);
    }


    @Test
    public void getDocument() throws IOException {
        GetRequest request = new GetRequest("xiaofei", "1");
        GetResponse documentFields = restHighLevelClient.get(request, RequestOptions.DEFAULT);
        System.out.println(documentFields.getSourceAsString());
        System.out.println(documentFields);
    }

    /**
     * @description: 修改文档
     * @author: feige
     * @date: 2021/8/25 17:45
     * @param
     * @return: void
     */
    @Test
    void updateDocument() throws IOException {
        HashMap<String, Object> map = new HashMap<>();
        map.put("name","xiaofei");
        map.put("age",21);
        map.put("introduction","马上学完elasticsearch");
        UpdateRequest request = new UpdateRequest("xiaofei", "1");
        request.doc(map);
        UpdateResponse response = restHighLevelClient.update(request, RequestOptions.DEFAULT);
        System.out.println(response.toString());
    }

    @Test
    public void deleteDocument() throws IOException {
        DeleteRequest request = new DeleteRequest("xiaofei", "1");
        DeleteResponse delete = restHighLevelClient.delete(request, RequestOptions.DEFAULT);
        System.out.println(delete);
    }


    /**
     * // 查询
     * SearchRequest 搜索请求
     * SearchSourceBuilder 条件构造
     * HighlightBuilder 构建高亮
     * TermQueryBuilder 精确查询
     * MatchAllQueryBuilder
     * xxx QueryBuilder 对应我们刚才看到的命令！
     * @description:
     * @author: feige
     * @date: 2021/8/26 14:31
     * @param
     * @return: void
     */
    @Test
    public void searchDocument() throws IOException {
        SearchRequest request = new SearchRequest("xiaofei");
        SearchSourceBuilder searchSourceBuilder = new SearchSourceBuilder();
        MatchQueryBuilder query = QueryBuilders.matchQuery("name", "feige");
        HighlightBuilder highlightBuilder = new HighlightBuilder();
        highlightBuilder.field("name");
        searchSourceBuilder.query(query);
        searchSourceBuilder.highlighter(highlightBuilder);
        request.source(searchSourceBuilder);
        SearchResponse response = restHighLevelClient.search(request, RequestOptions.DEFAULT);
        System.out.println(response);
    }

    /**
     * @description: 批处理
     * @author: feige
     * @date: 2021/8/26 14:48
     * @param		
     * @return: void
     */
    @Test
    public void bulkDocument() throws IOException {
        BulkRequest request = new BulkRequest();
        HashMap<String, Object> map = new HashMap<>();
        map.put("name","xiaofei");
        map.put("age",21);
        map.put("introduction","马上学完elasticsearch");
        for (int i = 0; i < 10; i++) {
            IndexRequest request1 = new IndexRequest("xiaofei");
            request1.id(String.valueOf(i + 100));
            request1.source(map);
            request.add(request1);
        }

        BulkResponse bulk = restHighLevelClient.bulk(request, RequestOptions.DEFAULT);
        System.out.println(bulk);
    }


}

~~~

