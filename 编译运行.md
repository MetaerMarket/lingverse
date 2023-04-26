#### git 仓库  
https://github.com/MetaerMarket/lingverse.git  
#### 依赖安装  
Erlang/OTP24 Elixir 1.12.x  
wget https://packages.erlang-solutions.com/erlang-solutions_2.0_all.deb  
dpkg -i erlang-solutions_2.0_all.deb  
apt update  
apt install esl-erlang  
apt install elixir  
执行 erl --version 输出 Erlang/OTP 24 ......  
执行 elixir --version 输出 Elixir 1.13.0 (compiled with Erlang/OTP 24)  
则表示 erlang 与 elixir安装成功。  
#### 其他依赖  
apt install automake autoconf 执行 automake --version 有输出。  
apt install libtool 执行 libtoolize --version 有输出。  
apt install inotify-tools  
apt install gcc  
apt install g++  
apt install make  
apt install cargo  
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh 执行 rustc --version 有输出  
wget https://gmplib.org/download/gmp/gmp-6.2.1.tar.zst && apt install zstd && tar -I zstd -xvf gmp-6.2.1.tar.zst 到目录下面执行 ./configure && make && make install 会将库安装到目录 /usr/local/lib/ 下面。  
curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash - && apt install -y nodejs 执行 node --version 有输出。  
Postgres安装与启动，我使用的docker形式安装。安装之前，请确认你已经安装好docker环境。  
docker volume create postgres-data  
docker run -d --name postgres-server -p 54321:5432 -v postgres-data:/var/lib/postgresql/data -e "POSTGRES_PASSWORD=123456" postgres  
#### 准备好环境变量  
export ETHEREUM_JSONRPC_VARIANT=geth  
export BLOCK_TRANSFORMER=clique  
export ETHEREUM_JSONRPC_HTTP_URL=http://172.17.74.254:7545  
export ETHEREUM_JSONRPC_WS_URL=ws://172.17.74.254:7545  
export DATABASE_URL=postgresql://postgres:123456@127.0.0.1:54321/blockscout  
export ECTO_USE_SSL=false  
export PORT=2000  
export CHAIN_SPEC_PATH=https://b.lucq.fun/api/noteShare/?id=1223&json=true  
export CHAIN_ID=1337  
export SECRET_KEY_BASE=VTIB3uHDNbvrY0+60ZWgUoUBKDn9p  
如果 SECRET_KEY_BASE 想更换一个，执行命令mix phx.gen.secret重新生成一个。  
#### 安装Mix依赖与编译  
mix do deps.get  
mix do local.rebar --force  
mix do deps.compile  
mix do compile  
如果需要清理以前的静态资源，执行命令 mix phx.digest.clean  
#### 创建数据库  
mix do ecto.create, ecto.migrate  
如果想要清理以前的数据，执行命令mix do ecto.drop  
#### 安装 Node.js 依赖与编译  
cd apps/block_scout_web/assets && npm i && ./node_modules/.bin/webpack --mode production  
cd apps/explorer && npm i  
#### 建立用于部署的静态资产  
cd apps/block_scout_web/ && mix phx.digest  
#### 启用HTTPS  
cd apps/block_scout_web/ && mix phx.gen.cert blockscout blockscout.local  
将 blockscout 和 blockscout.local 配置到 /etc/hosts  
127.0.0.1       localhost blockscout blockscout.local  
#### 启动Blockscout  
mix phx.server  


