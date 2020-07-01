# [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)

Elasticsearch là một công cụ tìm kiếm dựa trên nền tảng Apache Lucene. Nó cung cấp một bộ máy tìm kiếm dạng phân tán, có đầy đủ công cụ với một giao diện web HTTP có hỗ trợ dữ liệu JSON. Elasticsearch được phát triển bằng Java và được phát hành dạng nguồn mở theo giấy phép Apache.

## Usages

Elasticsearch cung cấp tìm kiếm `near real-time full-text` và phân tích được tất cả loại dữ liệu dù là dạng văn bản có cấu trúc hay không có cấu trúc, dữ liệu số hay dữ liệu không gian địa lý đều được Elasticsearch lưu trữ và set chỉ số cho nó một cách hiệu quả hỗ trợ việc tìm kiếm nhanh chóng hơn. Nhờ đó, nó được sử dụng trong nhiều trường hợp như:

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
- Elasticsearch chạy trên server riêng và đồng thời giao tiếp thông qua RESTful do vậy nên nó không phụ thuộc vào client viết bằng gì hay hệ thống hiện tại viết bằng gì nên việc tích hợp nó vào hệ thống là dễ dàng, chỉ cần gửi request http sau nó trả về kết quả.
- Elasticsearch là 1 hệ thống phân tán và có khả năng mở rộng tuyệt vời (horizontal scalability) bằng cách thêm node cho nó, nó sẽ tự động auto mở rộng cho bạn.

## Glossary

1. Near real-time:

Elasticsearch cho phép tìm kiếm nhanh chóng tuy không phải là ngay lập tức nhưng chỉ trễ  hơn không đến 1s.

2. Full-text:

Elasticsearch cho phép tìm kiếm bất cứ đâu trong trường mong muốn của Document.

3. Document

Document là một JSON object với một số dữ liệu, đây là đơn vị nhỏ nhất để lưu trữ dữ liệu trong Elasticsearch. Mỗi document được lưu trữ và đánh index.

4. Index

Index là một tập hợp documents có một số điểm giống nhau.

Mỗi index được xác định bằng một tên (chữ thường) và tên này được dùng khi thực hiện tìm kiếm, cập nhật và xóa các hoạt động đối với documents trong đó.

Thêm index là bước quan trọng trong việc chạy một công cụ tìm kiếm. Nếu không lập index cho nội dung, bạn sẽ không thể truy vấn nội dung đó bằng cách sử dụng Elaticsearch hoặc tận dụng bất kỳ tính năng tìm kiếm mạnh mẽ nào mà Elaticsearch cung cấp.

5. Mapping

Mapping là một định nghĩa lược đồ (schema) cho index.

Một mapping có thể được xác định cụ thể hoặc nó sẽ được tạo tự động khi một documents được thêm index.

5. Shard

Shard là đối tượng của Lucene, là tập con các document của 1 Index. Một Index có thể được chia thành nhiều shard.

Mỗi node bao gồm nhiều Shard. Chính vì thế Shard mà là đối tượng nhỏ nhất, hoạt động ở mức thấp nhất, đóng vai trò lưu trữ dữ liệu.

Shard là đối tượng quan trọng trong Elasticsearch vì nó cho phép bạn:

- Phân chia chiều ngang, tỷ lệ khối lượng nội dung của bạn.

- Phân phối và song song hóa các hoạt động trên các shard (có khả năng trên nhiều nút), do đó làm tăng hiệu suất và thông lượng.

Có 2 loại Shard là: primary shard và replica shard.

- Primary shard
  - Primary Shard là sẽ lưu trữ dữ liệu và đánh index. Sau khi đánh xong dữ liệu sẽ được vận chuyển tới các replica shard.
  - Mặc định của Elasticsearch là mỗi index sẽ có 5 primary shard và với mỗi primary shard thì sẽ đi kèm với 1 replica Shard.

- Replica shard

  - Replica shard đúng như cái tên của nó, nó là nơi lưu trữ bản sao của primary shard.

  - Replica shard có vai trò đảm bảo tính toàn vẹn của dữ liệu khi Primary Shard xảy ra vấn đề.

6. Node

Node là trung tâm hoạt động của Elasticsearch cũng là một phần của cluster, nơi lưu trữ dữ liệu, tham gia thực hiện đánh index của cluster cũng như thực hiện các thao tác tìm kiếm. Mỗi node được định danh bằng 1 unique name.

Các loại node:

- Master node: Điều khiển cluster
- Data node: Lưu dữ liệu và thực hiện các hoạt động liên quan đến dữ liệu như CRUD, tìm kiếm và tập hợp.
- Ingest node: Có khả năng áp dụng một `ingest pipeline` vào document để chuyển đổi và thêm thông tin cho trước khi thêm index.
- Machine learning node: Được sử dụng trong chức năng machine learning.
- Transform node: Sử dụng khi bạn muốn chuyển đổi dữ liệu.

7. Cluster

Một Cluster bao gồm một hoăc tập hợp các nodes hoạt động cùng với nhau, chia sẽ cùng thuộc tính `cluster.name`. Chính vì thế Cluster sẽ được xác định bằng 1 `unique name`.

Mỗi cluster có một node chính (master), được lựa chọn một cách tự động và có thể thay thế nếu sự cố xảy ra. Trong một cluster các node cùng nhau lưu giữ toàn bộ dữ liệu và cung cấp khả năng liên kết và tìm kiếm dữ liệu qua tất cả các nodes.

## Install

Docker:

```sh
docker pull docker.elastic.co/elasticsearch/elasticsearch:7.8.0
```

Cài đặt multi-node cluster sử dụng docker-compose:

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

Kiểm tra các node up and running:

```sh
curl -X GET "localhost:9200/_cat/nodes?v&pretty"
```

## [Architecture](https://www.elastic.co/blog/sizing-hot-warm-architectures-for-logging-and-metrics-in-the-elasticsearch-service-on-elastic-cloud)

### Basic architecture

![ ](https://images.contentstack.io/v3/assets/bltefdd0b53724fa2ce/blt5365b41d3c2bc21c/5c57e2988d25f6030ca0437d/uniform_cluster.png)

Trong ES cluster tất cả data nodes có cùng đặc điểm kỹ thuật(specification), đảm nhiệm tất cả vai trò và chia sẻ index và tải truy vấn cuối cùng.

### Hot-warm architecture

![ ](https://images.contentstack.io/v3/assets/bltefdd0b53724fa2ce/blte59c4541ff94d917/5c57e292a2b68bd90b9fee90/hot-warm_cluster.png)

Kiến trúc có 2 loại data nodes khác nhau:

- Hot data node: Chứa tất cả các index gần đây nhất, do đó xử lý tất cả index load trong cụm.

  Vì dữ liệu gần đây nhất cũng thường được truy vấn thường xuyên nhất, các node này có xu hướng rất bận rộn. Thêm index vào Elaticsearch có thể rất tốn CPU và I/O, các nút này cần phải hoạt động tích cực và có lưu trữ nhanh -> ổ SSDs gắn liền cục bộ.

- Warm data node: Xử lý lưu trữ dài hạn read-only indices trong cụm theo cách hiệu quả về chi phí. Chúng cần số lượng CPU và RAM tốt, disk quay or SAN thay cho SSDs

Khi indices trên các hot data node vượt quá thời gian lưu cho các nút đó sẽ được chuyển đến các warm data node.
