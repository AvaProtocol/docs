# 搭建Subscan

1. 环境：

   - Linux / Mac OSX
   - Git
   - Golang 1.12.4+
   - Redis 3.0.4+
   - MySQL 5.6+
   - Node 8.9.0+

   注：Mac OSX未试验成功



2. 配置

   1. 在mysql-server里创建名为subscan的数据库。

   2. 设置配置文件
      ```
      cp configs/redis.toml.example configs/redis.toml && cp configs/mysql.toml.example configs/mysql.toml && cp configs/http.toml.example configs/http.toml
      ```

      1. Redis configs/redis.toml

      > addr： redis host and port (default: 127.0.0.1:6379)

      2. Mysql configs/mysql.toml

      > host: mysql host (default: 127.0.0.1) user: mysql user (default: root) pass: mysql user passwd (default: "") db: mysql db name (default: "subscan")

      3. Http configs/http.toml

      > addr: local http server port (default: 0.0.0.0:4399)

      4. 在ui/package.json中加入

         ```json
         "config": {
           "nuxt": {
             "host": "0.0.0.0",
             "port": "3333"
           }
         },
         ```

   3. 复制acala配置文件

      将https://github.com/itering/scale.go/blob/master/network/acala.json 复制到 configs/source/acala.json




3. 启动
   1. 启动Acala网络

      ```
      ./acala --base-path /tmp/alice --chain local --alice --port 30333 --ws-port 9944 --rpc-port 9933  --validator --rpc-methods=Unsafe --ws-external --rpc-external --ws-max-connections 1000 --rpc-cors=all --unsafe-ws-external --unsafe-rpc-external
      
      ./acala --base-path /tmp/bob --chain local --bob --port 30334 --ws-port 9945 --rpc-port 9934  --validator --bootnodes /ip4/127.0.0.1/tcp/30333/p2p/12D3KooWNHQzppSeTxsjNjiX6NFW1VCXSJyMBHS48QBmmGs4B3B9
      ```

   1. 启动Subscan服务
     
      1. 启动Subscan Daemon

      ```
      cd cmd
      export CHAIN_WS_ENDPOINT=ws://127.0.0.1:9944
      export NETWORK_NODE=acala
      ./subscan start substrate
      ```

      2. 启动Api Server

      ```
      cd cmd
      export CHAIN_WS_ENDPOINT=ws://127.0.0.1:9944
      export NETWORK_NODE=acala
      ./subscan
      ```
   3. 启动网页UI

      ```
      cd ui && yarn
      yarn dev
      ```
      
      注： 这一步，第一次启动可能会出现浏览器加载很久的情况，多等待一会，多刷新一下。

4. 查看Subscan Daemon日志

   ```
   tail -f log/substrate_log
   ```

   