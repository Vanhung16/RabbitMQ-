![f2720e5c-cb23-4d23-9451-57a129823f9e.webp](..%2F..%2FAppData%2FLocal%2FTemp%2Ff2720e5c-cb23-4d23-9451-57a129823f9e.webp)
HAProxy:
- là 1 load balancing, reverse proxy
- Hỗ trợ cả HTTP và TCP
Default node that first create the cluster becomes the master node 
to set another node to master node we will use 'rabbitmqctl' to set leader node
NOTE: in the cluster any node must have the same erlang cookie
- in this case i set erland cookie is 'cluster cookie'
Tạo cluster RabbitMQ
- Kiểm tra status hiện tại của rabbitmq-node-1 bằng cách gửi lệnh vào container
  - docker exec -ti rabbitmq-node-1 bash -c "rabbitmqctl cluster_status"
- Stop apprabbitmq-node-2 bằng lệnh
  - docker exec -ti rabbitmq-node-2 bash -c "rabbitmqctl stop_app"
- Join rabbitmq-node-2 với rabbitmq-node-1
  - docker exec -ti rabbitmq-node-2 bash -c "rabbitmqctl join_cluster rabbit@rabbitmq-node-1"
- Cuối cùng start node 2
  - docker exec -ti rabbitmq-node-2 bash -c "rabbitmqctl start_app"
* Kiểm tra trạng thái
  * docker exec -ti rabbitmq-node-1 bash -c "rabbitmqctl cluster_status"
- ### Thực hiện lại các bước với node 3
  - $ docker exec -ti rabbitmq-node-3 bash -c "rabbitmqctl stop_app"
  - $ docker exec -ti rabbitmq-node-3 bash -c "rabbitmqctl join_cluster rabbit@rabbitmq-node-1"
  - $ docker exec -ti rabbitmq-node-3 bash -c "rabbitmqctl start_app"
- ### Kiểm tra kết quả: 
- docker exec -ti rabbitmq-node-1 bash -c "rabbitmqctl cluster_status