Linux instructions have been provided [ahoiahoi](https://github.com/ahoiahoi), no support outside of this page will be given at this time.  
More information is provided on the [Windows](https://github.com/Rawaho/NexusForever/wiki/Installation) installation page.

### Server Setup
```
mkdir ${HOME}/git
cd ${HOME}/git
git clone https://github.com/Rawaho/NexusForever.git
cd NexusForever/Source

for server in WorldServer StsServer AuthServer; do 
        cd NexusForever.${server}
        cat ${server}.example.json > ${server}.json
        ln -s ${PWD}/${server}.json ${PWD}/bin/Debug/netcoreapp2.1/${server}.json
        sed -i -e 's/"Database": ".*/"Database": "nexus"/' ${server}.json
        cd ..
done

cd NexusForever.WorldServer/
cp -a /path/to/tbl/files .
ln -s ${PWD}/tbl/ ${PWD}/bin/Debug/netcoreapp2.1/
```

### Database Setup
```
cd ${HOME/git/NexusForever/Database/

mysql -e "create database nexus;"
mysql -D mysql -e "grant all on nexus.* to 'nexusforever'@'127.0.0.1' identified by 'nexusforever';"
mysql -D nexus < Base/*.sql
mysql -D nexus < Updates/*/*.sql

mysql -D nexus -e "INSERT INTO server(name, host, port, type) VALUES ('NexusForever', '--insert-server-ip--', 24000, 0);"
```

### Launching the Server
```
export TERM=xterm
cd ${HOME}/git/NexusForever/Source
for server in StsServer AuthServer; do
        cd NexusForever.${server}
        dotnet run&
        cd ..
done
cd NexusForever.WorldServer
dotnet run
```