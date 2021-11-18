
**Volumen por separado de la configuración**
En el docker compose ponemos esto para crear el volumen por separado de la configuracion

volumes:
  dns_config:
    external: true


**Red propia interna para todos los contenedores**

networks:
       br02:

**ip fija en el servidor**

networks:
       br02:
        ipv4_address: 10.1.0.254

**Configurar Forwarders**
En el fichero named.conf.option porner en la parte de fordwarders lo siguiente

 forwarders {
 	8.8.4.4;
	 };

**Crear Zona propia**

//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";

zone "practica_dns.com" {
    type master;
    file "/volume/db.practica_dns.com";
};




**Registros a configurar: NS, A, CNAME, TXT, SOA**
    
    
; BIND data file for example.com
;
$TTL    604800
@       IN      SOA     practica_dns.com. root.practica_dns.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL

@       IN      NS      ns.practica_dns.com.
@       IN      A       10.1.0.254
@       IN      AAAA    ::1
practica IN	    A	    10.0.1.254
www IN  CNAME   practica_dns.com     
@   IN  TXT "QUE ACABE EL SUFRIMIENTO"



**Cliente con herramientas de red**

Para instalar las herramientas de red, ejecutamos el siguiente comando:

sudo apt-get install -y net-tools

Para hacer la imgen del server dns con las tools de red, ponemos en la consola(fuera de la imagen), el siguiente comando:

docker commit -m "imagen dns net tools" asind_bind9vv dns_net

Si quisieramos crear un contenedor con esa imagen, en el docker-compose añadiriamos lo siguiente

asir_cliente:
    image: dns_net
    networks: 
      - br02 
    dns:
      - 10.1.0.254



**Probamos el dig**

Primero instalamos los comandos del DNS
apt-get install DNS-utils

acto seguido hacemos  lo siguiente:

dig @10.0.1.254 www.practica_dns.com

root@0d03c5d7959e:/# dig @10.0.1.254 www.practica_dns.com    

; <<>> DiG 9.16.1-Ubuntu <<>> @10.0.1.254 www.practica_dns.com
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 8419
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;www.practica_dns.com.          IN      A

;; Query time: 4 msec
;; SERVER: 10.0.1.254#53(10.0.1.254)
;; WHEN: Wed Nov 17 17:31:10 UTC 2021
;; MSG SIZE  rcvd: 49

root@0d03c5d7959e:/# 

