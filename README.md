# Mysql_Sharing_Cluster
Tham khảo: https://dev.mysql.com/doc/refman/8.0/en/mysql-cluster-overview.html

<img src="https://dev.mysql.com/doc/refman/8.0/en/images/cluster-components-1.png" title="cluster-components"><br/>
# Ý tưởng.
Một ứng dụng có lượng lơn dữ liệu (100GB) dữ liệu, thực hiện nhiều lệnh truy vấn cùng lúc (1 triệu truy vấn / Giây).

# Vấn đề.
Một server chạy thông thường sẽ không dủ để đáp ứng được một lượng dữ liệu lớn và nhiều truy vấn như vậy.

Khi server này down thì ứng dụng cũng down ngay lập tức vì không có giải pháp backup.
# Giải pháp.
Cần phải chia nhỏ dữ liệu ra nhiều server để quản lý.

Có nhiều Server để nhận truy vấn từ ứng dụng.

Mysql cung cấp giải pháp cluster để đáp ứng nhu cầu trên: InnoDB Cluster và NBD Cluster.

- InnoDB Cluster thì không có nhiều Node Sql để nhận truy vấn từ ứng dụng, NBD Cluster có thể mở rộng được.

- NBD Cluster dữ liệu của bảng sẽ được chia ra lưu trữ trên nhiều ndbd được quản lý bởi ndb_mngd.

# Docker-compose

# TL;DR

To start this cluster, run:

docker-compose up -d
```

Wait a few seconds, and check the status of the cluster:

```
docker exec -it 7f4c76b6e1913a11154867149d9cf3cc_mysql-tester_1 ndb_mgm
ndb_mgm> show
Cluster Configuration
---------------------
[ndbd(NDB)]	2 node(s)
id=11	@172.25.0.8  (mysql-5.7.25 ndb-7.6.9, Nodegroup: 0, *)
id=12	@172.25.0.7  (mysql-5.7.25 ndb-7.6.9, Nodegroup: 0)

[ndb_mgmd(MGM)]	2 node(s)
id=1	@172.25.0.6  (mysql-5.7.25 ndb-7.6.9)
id=2	@172.25.0.3  (mysql-5.7.25 ndb-7.6.9)

[mysqld(API)]	2 node(s)
id=21	@172.25.0.2  (mysql-5.7.25 ndb-7.6.9)
id=22	@172.25.0.4  (mysql-5.7.25 ndb-7.6.9)

ndb_mgm>
```
