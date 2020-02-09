# KC_BDP
Entrega de la práctica del módulo de Big Data Processing
# Práctica BDProcessing KeepCoding

**Parte 1:**
Hay que crear el topic, en este caso le llamaremos "practica" y el producer. Se comprueba en el terminal el funcionamiento. 
- Comandos y capturas:

Creación del topic
```python 
root@debian:/home/kafka/kafka_2.11-2.4.0# bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic practica
```
Adjunto captura línea de código:

https://drive.google.com/open?id=1_upDVhWwtkMgW-COy3Og08bKt4TyqCTJ

Creación del Producer
```python
root@debian:/home/kafka/kafka_2.11-2.4.0# cat personal.json | bin/kafka-console-producer.sh --broker-list localhost:9092 --topic practica >/dev/null
```
Adjunto captura línea de código

https://drive.google.com/open?id=1eyhnx0q7fqEYnbxsQpX7oxe6Z8qMI-TB


**Parte 2**

Para la segunda parte de la práctica se crea un "Consumer" en Scala y se aplican los filtros "Bea" y "Willard" al archivo personal.json .
- Código Scala y Captura del resultado

```python
import java.util.Properties

import scala.collection.JavaConverters._

import org.apache.kafka.clients.consumer.KafkaConsumer

  

object practica {

def main(args: Array[String]): Unit = {

val props: Properties = new Properties()

props.put("group.id", "test")

props.put("bootstrap.servers", "localhost:9092")

props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer")

props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer")

props.put("acks", "all")

val consumidor = new KafkaConsumer[String, String](props)

val topics= List("practica")

try {

  

consumidor.subscribe(topics.asJava)

while (true){

val registro = consumidor.poll(100)

for (r<-registro.asScala.filter(v => v.value().indexOf( "Willard") == -1 && v.value().indexOf( "Bea") == -1 )) {

println(r.value())

} }

}catch{

case e: Exception => e.printStackTrace()

} finally{

consumidor.close() }

}
  
}
```` 

- Adjunto captura con el resultado con los nombres filtrados:

https://drive.google.com/open?id=1CtBxPt_qVKIzeQLvYpi-kmsG8L7vUFXh
