# elk-docker-ssl

作業系統 : Ubuntu 20.04
並先行安裝 $: sudo apt install docker-compose

此專案提供elk內部ssl連線及CA設定
並使用docker container運行

專案抓下來後
1.需先執行下列指令安裝CA於 docker的volume內
$: docker-compose -f create-certs.yml run --rm create_certs

2.執行docker compose進行 docker安裝
$: docker-compose up -d

3.複製 docker volume 內的ca至機器上
$: docker cp elk-es01:/usr/share/elasticsearch/config/certificates/ca/ca.crt /tmp

4.檢查 elasticsearch是否能運行
$: curl --cacert /tmp/ca.crt -u elastic:changeme https://localhost:9200?pretty

5.kibana_system 換密碼成 changeme
$: 
curl --cacert /tmp/ca.crt -u elastic:changeme -X POST "https://localhost:9200/_security/user/kibana_system/_password?pretty" -H 'Content-Type: application/json' -d'
{
  "password" : "changeme"
}
'

6.logstash_system 換密碼成 changeme
$:
curl --cacert /tmp/ca.crt -u elastic:changeme -X POST "https://localhost:9200/_security/user/logstash_system/_password?pretty" -H 'Content-Type: application/json' -d'
{
  "password" : "changeme"
}
'

以上完成後即可運行~!

參考 docker compose設定:
    yt :  https://www.youtube.com/watch?v=v982iirPsw4&ab_channel=RaphaelDeLio
    git : https://github.com/raphaeldelio/ElasticSearchTutorial/tree/master/docker/start-elasticsearch-with-encryption