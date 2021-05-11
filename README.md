# SSL-Verify  
Cliente HTTP para conectarse a una url e intentar consumir.  
Los casos de uso de esta herramienta son:  
* Cuando no se sabe que la ruta de los cacert que la aplicación o el sistema está utilizando. 
* Cuando se tiene error al consumir un servicio web con HTTPS y se generan error **Received fatal alert: protocol_version** y no se sabe la causa

## Argumentos JAVA  
### Djdk.tls.client.protocols  
Argumento que sirve para pasarle el protocolo TLS a usar para la conexion. Ejemplo

    java -jar  -Djdk.tls.client.protocols=TLSv1.2 ssl-verify.jar 

Los posibles valores son: `TLSv1`, `TLSv1.1`, `TLSv1.2` y `TLSv1.3`
### Djavax.net.debug 
Argumento que sirve para indicarle a java que nos muestre información acerca de los detalles del establecimiento de conexion https. Ejemplo. 
    
    java -jar  -Djdk.tls.client.protocols=TLSv1.2 -Djavax.net.debug=ssl:record:plaintext ssl-verify.jar. 
        
Los posibles valores pueden ser mucho, pero se recomienda `-Djavax.net.debug=ssl` que muestra el detalle del establecimiento de las conexion; entre los puntos que se muestran está la ruta excacta de los cacer de java que usa y donde se debe instalar los certificados autofirmado, y los protocolos TLS y chipher suite usados en la conexion

    Inaccessible trust store: /[JAVA_HOME]/jre/lib/security/jssecacerts
    trustStore is: /[JAVA_HOME]/jre/lib/security/cacerts
    trustStore type is: jks
    trustStore provider is: 
    .
    .
    .
    update handshake state: finished[20]
    upcoming handshake states: server change_cipher_spec[-1]
    upcoming handshake states: server finished[20]
    main, WRITE: TLSv1.2 Handshake, length = 40
    main, READ: TLSv1.2 Change Cipher Spec, length = 1
    update handshake state: change_cipher_spec
    upcoming handshake states: server finished[20]
    main, READ: TLSv1.2 Handshake, length = 40
    check handshake state: finished[20]
    update handshake state: finished[20]
    *** Finished
    verify_data:  { 160, 35, 99, 226, 174, 100, 133, 23, 162, 121, 212, 70 }
    ***
    %% Cached client session: [Session-1, TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384]
    main, WRITE: TLSv1.2 Application Data, length = 226
    main, READ: TLSv1.2 Application Data, length = 1412
    
En caso de error
    
    Compression Methods:  { 0 }
    Extension elliptic_curves, curve names: {secp256r1, secp384r1, secp521r1, sect283k1, sect283r1, sect409k1, sect409r1, sect571k1, sect571r1, secp256k1}
    Extension ec_point_formats, formats: [uncompressed]
    Extension extended_master_secret
    Extension server_name, server_name: [type=host_name (0), value=pilotosiatservicios.impuestos.gob.bo]
    ***
    main, WRITE: TLSv1 Handshake, length = 156
    main, READ: TLSv1 Alert, length = 2
    main, RECV TLSv1.2 ALERT:  fatal, protocol_version
    main, called closeSocket()

