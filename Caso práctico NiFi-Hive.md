# Caso práctico NiFi - Hive

Este caso practico vamos a crear una tabla en Hive con los datos de un topic de kafka

## Caso 1 - Enviar datos desde Kafka a NiFi

A continuación detallamos los pasos a realizar:

1. Seleccionamos un procesador (primer icono grande) y lo arrastramos en nuestra área de trabajo.
2. Así pues, buscamos el procesador ConsumerKafkaRecord_2_6 y lo añadimos al flujo.
3. Configuramos las propiedades del procesador de la siguiente forma:
	
	![](images/Captura.PNG)
	![](images/Captura2.PNG)
   
   _Propiedades Consumer Kafka_

4. Configuramos también el CSVReader

	![](images/Captura3.PNG)
   
   _Propiedades CSVReader_

5. Y el CSVWriter

	![](images/Captura4.PNG)
    ![](images/Captura5.PNG)
	
   _Propiedades CSVWriter_
   
6. Activamos los Reader/Writer

    ![](images/Captura6.PNG)
	
   _Activar CSVWriter_
   
7. Por último marcamos la relacion parse.failure como terminada

	![](images/Captura7.PNG)
	
   _Activar CSVWriter_

   
## Caso 2 - Split y Avro

Vamos a leer hacer split de los mensajes anteriores y vamos a convertir los mensajes en avro para enviarlos a HDFS

1. Seleccionamos el procesador SplitRecord y lo arrastramos en nuestra área de trabajo.

2. Configuramos las propiedades del procesador de la siguiente forma:
	
	![](images/Captura8.PNG)

	_Propiedades SplitRecord_
	
3. Configuramos el AvroRecordSetWriter

	![](images/Captura9.PNG)

	_Propiedades Writer Avro_

4. Terminamos las relaciones que no son spliteadas

	![](images/Captura10.PNG)

	_Terminar relaciones_
	
5. Unimos los dos procesadores con la relacion succes
