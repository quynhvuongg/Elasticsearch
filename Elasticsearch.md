# Elasticsearch

Elasticsearch là một công cụ tìm kiếm dựa trên nền tảng Apache Lucene. Nó cung cấp một bộ máy tìm kiếm dạng phân tán, có đầy đủ công cụ với một giao diện web HTTP có hỗ trợ dữ liệu JSON. Elasticsearch được phát triển bằng Java và được phát hành dạng nguồn mở theo giấy phép Apache.

## Features

Elasticsearch cung cấp tìm kiếm `near real-time full-text` và phân tích được tất cả loại dữ liệu dù là dạng văn bản có cấu trúc hay không có cấu trúc, dữ liệu số hay dữ liệu không gian địa lý đều đươc Elasticsearch lưu trữ và set chỉ số cho nó một cách hiệu quả hỗ trợ việc tìm kiếm nhanh chóng hơn. Nhờ đó, nó được sử dụng trong nhiều trường hợp như:

- Thêm search box cho một ứng dụng hoặc website có khả năng tìm kiếm nhanh chóng.
- Lưu trữ và phân tích logs, metrics và dữ liệu sự kiện bảo mật.
- Sử dụng machine learning để tự động mô hình hóa hành vi của dữ liệu trong thời gian thực.
- Tự động hóa quy trình công việc bằng cách sử dụng Elaticsearch làm công cụ lưu trữ.
- Quản lý, tích hợp và phân tích thông tin không gian bằng cách sử dụng Elaticsearch làm hệ thống thông tin địa lý.
- Lưu trữ và xử lý dữ liệu di truyền bằng cách sử dụng Elaticsearch làm công cụ nghiên cứu tin sinh học.

## Characteristics

- Elasticsearch là 1 open source được phát triển bằng Java.
- Elasticsearch là một search engine.
- Elasticsearch được kế thừa từ Lucene Apache.
- Elasticsearch thực chất hoạt động như 1 web server, có khả năng tìm kiếm nhanh chóng (near realtime) thông qua giao thức RESTful.
- Elasticsearch có khả năng phân tích và thống kê dữ liệu.
- Elasticsearch chạy trên server riêng và đồng thời giao tiếp thông qua RESTful do vậy nên nó không phụ thuộc vào client viết bằng gì hay hệ thống hiện tại viết bằng gì nên việc tích hợp nó vào hệ thống bạn là dễ dàng, chỉ cần gửi request http lên là nó trả về kết quả.
- Elasticsearch là 1 hệ thống phân tán và có khả năng mở rộng tuyệt vời (horizontal scalability) bằng cách thêm node cho nó, nó sẽ tự động auto mở rộng cho bạn.

## Glossary

1. Near real-time:

Elasticsearch cho phép tìm kiếm nhanh chóng tuy không phải là ngay lập tức nhưng chỉ trễ  hơn không đến 1s.

2. Full-text:

Elasticsearch cho phép tìm kiếm bất cứ đâu trong trường mong muốn của Document.

3. Document

Document là một JSON object với một số dữ liệu, đây là đơn vị nhỏ nhất để lưu trữ dữ liệu trong Elasticsearch.

4. Shard

Shard là đối tượng của Lucene , là tập con các document của 1 Index. Một Index có thể được chia thành nhiều shard.
Mỗi node bao gồm nhiều Shard. Chính vì thế Shard mà là đối tượng nhỏ nhất, hoạt động ở mức thấp nhất, đóng vai trò lưu trữ dữ liệu.
Chúng ta gần như không bao giờ làm việc trực tiếp với các Shard vì Elasticsearch đã support toàn bộ việc giao tiếp cũng như tự động thay đổi các Shard khi cần thiết.
Có 2 loại Shard là : primary shard và replica shard.

- Primary shard
Primary Shard là sẽ lưu trữ dữ liệu và đánh index. Sau khi đánh xong dữ liệu sẽ được vận chuyển tới các replica shard.
Mặc định của Elasticsearch là mỗi index sẽ có 5 primary shard và với mỗi primary shard thì sẽ đi kèm với 1 replica Shard.

- Replica shard
Replica shard đúng như cái tên của nó, nó là nơi lưu trữ dữ liệu nhân bản của primary shard.

Replica shard có vai trò đảm bảo tính toàn vẹn của dữ liệu khi Primary Shard xảy ra vấn đề.

5. Index

Index là tên logic ánh xạ đến một hoặc nhiều primary shard.

Trong Elasticsearch, sử dụng một cấu trúc được gọi là inverted index . Nó được thiết kế để cho phép tìm kiếm full-text search. Cách thức của nó khá đơn giản, các văn bản được phân tách ra thành từng từ có nghĩa sau đó sẽ được map xem thuộc văn bản nào. Khi search tùy thuộc vào loại search sẽ đưa ra kết quả cụ thể.

VÍ dụ : Chúng ta có 2 văn bản cụ thể như sau :

```sh
1,The quick brown fox jumped over the lazy dog
2,Quick brown foxes leap over lazy dogs in summer
```

Để tạo ra một [inverted index](https://www.elastic.co/guide/en/elasticsearch/guide/current/inverted-index.html), trước hết chúng ta sẽ phân chia nội dung của từng tài liệu thành các từ riêng biệt (chúng gọi là terms), tạo một danh sách được sắp xếp của tất cả terms duy nhất, sau đó liệt kê tài liệu nào mà mỗi thuật ngữ xuất hiện. Kết quả như sau:

```sh
Term      Doc_1  Doc_2
-------------------------
Quick   |       |  X
The     |   X   |
brown   |   X   |  X
dog     |   X   |
dogs    |       |  X
fox     |   X   |
foxes   |       |  X
in      |       |  X
jumped  |   X   |
lazy    |   X   |  X
leap    |       |  X
over    |   X   |  X
quick   |   X   |
summer  |       |  X
the     |   X   |
------------------------
```

Khi muốn tìm kiếm màu quick brown, chúng ta chỉ cần tìm trong các document trong đó mỗi term có xuất hiện hay không.

```sh
Term      Doc_1  Doc_2
-------------------------
brown   |   X   |  X
quick   |   X   |
------------------------
Total   |   2   |  1
```

Ở đây cả 2 documents đều match với từ khóa. Tuy nhiên có thể  thấy Doc_1 match nhiều hơn, kết quả tìm kiếm sẽ đưa đến nội dung của Doc_1.

6. Node

Là trung tâm hoạt động của Elasticsearch. Là nơi lưu trữ dữ liễu ,tham gia thực hiện đánh index của cluster cũng như thực hiện các thao tác tìm kiếm.Mỗi node được định danh bằng 1 unique name

7. Cluster

Tập hợp các nodes hoạt động cùng với nhau, chia sẽ cùng thuộc tính cluster.name. Chính vì thế Cluster sẽ được xác định bằng 1 ‘unique name’.

Mỗi cluster có một node chính (master), được lựa chọn một cách tự động và có thể thay thế nếu sự cố xảy ra. Một cluster có thể gồm 1 hoặc nhiều nodes. Các nodes có thể hoạt động trên cùng 1 server . Tuy nhiên trong thực tế , một cluster sẽ gồm nhiều nodes hoạt động trên các server khác nhau để đảm bảo nếu 1 server gặp sự cố thì server khác (node khác) có thể hoạt động đầy đủ chức năng so với khi có 2 servers. Các node có thể tìm thấy nhau để hoạt động trên cùng 1 cluster qua giao thức unicast.

Chức năng chính của Cluster đó chính là quyết định xem shards nào được phân bổ cho node nào và khi nào thì di chuyển các Cluster để cân bằng lại Cluster.

## Install

Docker: `docker pull docker.elastic.co/elasticsearch/elasticsearch:7.8.0`

Cài đặt multi-node cluster sử dụng docker-compose

```sh
version: '3'
services:
  es01:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.8.0
    container_name: es01
    environment:
      - node.name=es01
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es02,es03
      - cluster.initial_master_nodes=es01,es02,es03
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data01:/usr/share/elasticsearch/data
    ports:
      - 9200:9200

  es02:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.8.0
    container_name: es02
    environment:
      - node.name=es02
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es01,es03
      - cluster.initial_master_nodes=es01,es02,es03
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data02:/usr/share/elasticsearch/data

  es03:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.8.0
    container_name: es03
    environment:
      - node.name=es03
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es01,es02
      - cluster.initial_master_nodes=es01,es02,es03
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data03:/usr/share/elasticsearch/data

volumes:
  data01:
    driver: local
  data02:
    driver: local
  data03:
    driver: local

```

Sử  dụng: `curl -X GET "localhost:9200/_cat/nodes?v&pretty"` để kiểm tra các node up and running.

## REST API

### Index

1. Tạo 1 index

```sh
curl -X PUT "localhost:9200/twitter?pretty"
```

hoặc chỉ định các [tham số](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html#indices-create-api-path-params) cụ thể cho index như settings, mappings, aliases

```sh
curl -X PUT "localhost:9200/test" -H 'Content-Type: application/json' -d'
{
    "settings" : {
        "number_of_shards" : 1
    },
    "mappings" : {
        "properties" : {
            "field1" : { "type" : "text" }
        }
    }
}
'
```

Sau khi index được tạo sẽ trả về :

```sh
{
    "acknowledged": true,
    "shards_acknowledged": true,
    "index": "test"
}
```

2. Liệt kê các index

```sh
curl -X GET "localhost:9200/_cat/indices"
```

3. Kiểm tra index

```sh
curl -X GET "localhost:9200/test"
```

4. Xóa index

```sh
curl -X DELETE "localhost:9200/test"
```

5. **Request body**

```sh
curl -X GET "localhost:9200/test?q=abc=xyz"
```

### Search

Khi muốn tìm một số dữ liệu  đã nhập vào một index của Elasticsearch thì gửi yêu cầu đến `_search` endpoint:

```sh
curl -X GET "localhost:9200/test/_search"
```

**Request body**:

```sh
curl -X GET "localhost:9200/_search?q=user:kimchy"
```

```sh
curl -X GET "localhost:9200/_search?q=message:number&size=0&terminate_after=1"
```
