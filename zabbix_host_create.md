通用版本- 脚本与例子
```
=======================================================
#!/bin/bash
work_space="$(cd $(dirname "${BASH_SOURCE[0]}") && pwd)"
cd $work_space
declare -A myMap

function showhelp() {
    echo -e '说明：
############################################
## zabbix生成主机脚本
## 需要调用多配置文件，文件不存在不执行
## 需优先在zabbix服务端配置好指定agent代理程序
## 问题联系：OC林锦华  2021-08-27
###########################################     
'
    echo "格式:$0 [可选参数]"
    echo -e '
    可选参数为:
            -f             调用的服务配置，比如mysql=1.2.1.1 1.2.1.2
            -s             调用的IP主机名，比如1.2.1.1  test-tw-db-01-01
            -r             调用的服务-模板规则，需与服务配置的服务名一一对应
                             例如 mysqltpl:Template App MySQL
                             多个模板规则以逗号做为分隔符
            -o             生成的xml文件名
            -a             产品地区，比如test-tw
            -p             指定zabbix proxy服务端名
            -h             使用帮助'
    echo "        例如: $0 -a test-tw -p test-tw-proxy -s hostname -f services.conf -r servicestpl.conf -o test-tw-hosts.xml"
    exit 0
}

function parseargs() {
    if [[ -z "${12}" ]] || [[ -n "${13}" ]]; then
        showhelp
    fi
    echo $@ | grep '\-f' | grep '\-o' | grep '\-s' | grep '\-r'| grep '\-p' | grep '\-a' >/dev/null || showhelp

    myMap["-a"]=$(echo $@ | awk '{for (i = 1; i <= 11; i=i+2){if($i == "-a"){print $(i+1)}}}')
    myMap["-f"]=$(echo $@ | awk '{for (i = 1; i <= 11; i=i+2){if($i == "-f"){print $(i+1)}}}')
    myMap["-p"]=$(echo $@ | awk '{for (i = 1; i <= 11; i=i+2){if($i == "-p"){print $(i+1)}}}')
    myMap["-s"]=$(echo $@ | awk '{for (i = 1; i <= 11; i=i+2){if($i == "-s"){print $(i+1)}}}')
    myMap["-o"]=$(echo $@ | awk '{for (i = 1; i <= 11; i=i+2){if($i == "-o"){print $(i+1)}}}')
    myMap["-r"]=$(echo $@ | awk '{for (i = 1; i <= 11; i=i+2){if($i == "-r"){print $(i+1)}}}')
    for key in "${!myMap[@]}"; do
        if [[ "${myMap[$key]}" == '-a' ]] || [[ "${myMap[$key]}" == '-p' ]] || [[ "${myMap[$key]}" == '-s' ]] || [[ "${myMap[$key]}" == '-f' ]] || [[ "${myMap[$key]}" == '-o' ]] || [[ "${myMap[$key]}" == '' ]]; then
            echo "== check args $key value =="
            echo "$key value error"
            exit 0
        fi
    done

}



function createxml() {

    parseargs "$@"
    ###############################################################################
    echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>
<zabbix_export>
    <version>5.0</version>
    <date>$(date "+%FT%T%N" | sed -r 's/[[:digit:]]{9}$/Z/')</date>
    <groups>
        <group>
            <name>Discovered hosts</name>
        </group>
" >${myMap["-o"]}
    ################################################################################
    awk -v awkservicename=${myMap[-a]} -F'[_|=]' '{print awkservicename"-"$1}' ${myMap[-f]} | sort | sort -rn | uniq | while read line; do
        echo "
        <group>
            <name>$line</name>
        </group>
" >>${myMap["-o"]}
    done
    ################################################################################
    echo "
    </groups>
    <hosts>
" >>${myMap["-o"]}
    ################################################################################
    cat ${myMap["-f"]} | grep -oE "(2[0-4][0-9]|25[0-5]|1[0-9][0-9]|[1-9]?[0-9])(\.(2[0-4][0-9]|25[0-5]|1[0-9][0-9]|[1-9]?[0-9])){3}" | sort | sort -rn | uniq | while read line; do
        servername=$(grep $line ${myMap["-s"]} | awk '{print $2}')
        echo "
        <host>
            <host>${servername}</host>
            <name>${servername}</name>
" >>${myMap["-o"]}
        echo ${servername} | grep zabbix >/dev/null 2>&1 ||
            echo "
            <proxy>
                <name>${myMap["-p"]}</name>
            </proxy>
" >>${myMap["-o"]}
        echo "
            <templates>
" >>${myMap["-o"]}
        ########################################################################################
        num_line=$(grep $line ${myMap["-f"]} | wc -l)
        for num in $(seq ${num_line}); do
            grep "$(grep $line ${myMap["-f"]} | awk -v linenum=$num 'NR==linenum{print $0}' | awk -F'=' '{print $1}')tpl" ${myMap["-r"]} | uniq | awk -F':' '{print $2}' | sed 's/,/\n/g' | while read template; do
                [[ -n ${template} ]] && echo "
                <template>
                    <name>${template}</name>
                </template>
" >>${myMap["-o"]}
            done

        done
        echo "
                <template>
                    <name>Template OS Linux by Zabbix agent</name>
                </template>
" >>${myMap["-o"]}
        ########################################################################################

        servicegroup=$(grep $line ${myMap["-f"]} | awk -F'[_|=]' '{print $1}' | sort|sort -rn|uniq | head -1)
        echo "
           </templates>
            <groups>
                <group>
                    <name>Discovered hosts</name>
                </group>
                <group>
                    <name>${myMap[-a]}-${servicegroup}</name>
                </group>
            </groups>
            <interfaces>
                <interface>
                    <ip>$line</ip>
                    <dns>${myMap[-a]}.$line.nip.io</dns>
                    <interface_ref>if1</interface_ref>
                </interface>
            </interfaces>
            <inventory_mode>DISABLED</inventory_mode>
        </host>
        " >>${myMap["-o"]}
    done
    ########################################################################################
    echo "
    </hosts>
</zabbix_export>
" >>${myMap["-o"]}

}

function main() {
    createxml "$@"
}

main "$@"

```






例子
```
hostname
10.75.12.10 cdc-de-k8s-bas-01-01
10.75.12.11 cdc-de-k8s-bas-01-02
10.75.12.12 cdc-de-k8s-bas-01-03
10.75.12.20 cdc-de-k8s-db-01-01
10.75.12.21 cdc-de-k8s-db-01-02
services.conf
mysql=10.75.12.20 10.75.12.21
redis=10.75.12.10 10.75.12.11 10.75.12.12
rabbitmq=10.75.12.10 10.75.12.11 10.75.12.12
servicestpl.conf
redistpl:redis-port
rabbitmqtpl:rabbitmq-port,rabbitmq-Template
mysqltpl:mysql_slave_status,Template App MySQL
```
运行
```
run.sh
bash zbxgen.sh -a cdc-de-k8s -p cdc-de-k8s-proxy -s hostname -f services.conf -r servicestpl.conf -o cdc-de-k8s-hosts.xml
```
