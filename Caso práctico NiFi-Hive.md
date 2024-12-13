# Caso práctico NiFi - Hive

Este caso practico vamos a crear una tabla en Hive con los datos de un topic de kafka

## Caso 1 - Enviar datos del dataset de ventas a kafka

En primer lugar vamos a crear un topic donde poder enviar los datos del fichero csv que vimos en el caso práctico.
En mi caso se va a llamar ventas_mbdw

Desde el nodo edge ejecutamos:
```
kafka-topics --bootstrap-server  master01.bigdata.alumnos.upcont.es:9092 --topic ventas_mbdw --create --partitions 1 --replication-factor 1
```

Ahora continuamos desde la aplicacion nifi.

1. Añadimos un procesador a nuestro canvas de tipo GetFile y añadimos la ruta desde donde queremos que lea el fichero. Recordar que si no queremos que el procesador lea recurrentemente el mismo archivo tenemos que poner la opcion _Keep Source File_ a false

	![](images/keepSourceFile.png)

2. Ahora añadimos un procesador de tipo SplitRecord para generar un Flow File por cada fila del fichero csv

	![](images/SplitRecord.png)

3. Añadimos un procesador para publicar en kafka con las siguientes propiedades:

   	![](images/PublishKafka.png)

4. Por ultimo unimos los procesadores GetFile y SplitRecords para los casos de exito y los procesadores SplitRecord y PublishKafka para los casos splits. Y autoterminamos el resto de relaciones para cada uno de los procesadores.

	![](images/NifiKafka.png)

6. Finalmente copiamos el fichero csv en la ruta indicada en el punto 1 y podremos comprobar que los eventos están llegando al topic kafka con el siguiente comando lanzado desde el edge:
```
kafka-console-consumer --bootstrap-server  master01.bigdata.alumnos.upcont.es:9092 --topic  ventas_mbdw  --from-beginning
```

## Caso 2 - Enviar datos desde Kafka a NiFi

A continuación detallamos los pasos a realizar:

1. Añadimos el procesador ConsumerKafkaRecord_2_6 al flujo de trabajo.
2. Configuramos las propiedades del procesador de la siguiente forma:
	
	![](images/Captura.PNG)
	![](images/Captura2.PNG)
   
   _Propiedades Consumer Kafka_

3. Configuramos también el CSVReader

	![](images/Captura3.PNG)
   
   _Propiedades CSVReader_

4. Configuramos el AvroRecordSetWriter

	![](images/Captura9.PNG)

	_Propiedades Writer Avro_
   
5. Activamos los Reader/Writer

    ![](images/Captura6.PNG)
	
   _Activar Writer_
   
6. Marcamos la relacion parse.failure como terminada

	![](images/Captura7.PNG)
	
   _Activar Writer_

7. Ahora nos vamos a crear una ruta en hdfs donde enviar los datos. En mi caso es /user/jrfacosta/ventas_mbdw

   	![](images/RutaHdfs.PNG)

8. Le damos permisos para que pueda escribir nifi en el directorio

	![](images/Permisos.png)
	
7. Por ultimo añadimos el procesador PutHDFS

 	![](images/PutHdfs.PNG)
	
   _Propiedades PutHDFS_

   Para usar este procesador tenemos que descargarnos los archivos de core-site.xml y hdfs-site.xml de la ruta /tmp/jrfa del edge.
   Y copiarlos en alguna ruta de nuestro pc que tendremos que indicarle al procesador.
   

  	![](images/EnvioHdfs.PNG)

