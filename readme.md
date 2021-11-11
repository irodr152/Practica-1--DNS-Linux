
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
    
**Registros a configurar: NS, A, CNAME, TXT, SOA**
    
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
