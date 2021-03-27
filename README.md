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

# Thực hiện bằng Docker

# Chạy các lệnh trong fle docker-cammand và kiểm tra kết quả

```
docker run -it --net=cluster mysql/mysql-cluster ndb_mgm
[Entrypoint] MySQL Docker Image 8.0.23-1.1.19-cluster
[Entrypoint] Starting ndb_mgm
-- NDB Cluster -- Management Client --
ndb_mgm> 
ndb_mgm> show
Connected to Management Server at: 192.168.0.2:1186
Cluster Configuration
---------------------
[ndbd(NDB)]	3 node(s)
id=2	@192.168.0.3  (mysql-8.0.23 ndb-8.0.23, Nodegroup: 0, *)
id=3	@192.168.0.4  (mysql-8.0.23 ndb-8.0.23, Nodegroup: 0)
id=5	@192.168.0.5  (mysql-8.0.23 ndb-8.0.23, Nodegroup: 0)

[ndb_mgmd(MGM)]	1 node(s)
id=1	@192.168.0.2  (mysql-8.0.23 ndb-8.0.23)

[mysqld(API)]	2 node(s)
id=4	@192.168.0.10  (mysql-8.0.23 ndb-8.0.23)
id=6	@192.168.0.11  (mysql-8.0.23 ndb-8.0.23)
