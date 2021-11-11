
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
