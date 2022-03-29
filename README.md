# FLiP-PulsarDevPython101
Apache Pulsar Development 101 with Python



### Setup Python Libraries 3.6++

````

pip3 install requests
pip3 install paho-mqtt
pip3 install kafka-python
pip3 install websocket-client
pip3 install pulsar-client=='2.9.1[all]'

````

### Example

````
    # MQTT
    import paho.mqtt.client as mqtt
    client = mqtt.Client("rpi4-iot")
    client.connect("pulsar1", 1883, 180)
    client.publish("persistent://public/default/mqtt-2", payload=json_string, qos=0, retain=True)
    print("sent mqtt: " + json_string) 

    # Apache Kafka
    from kafka import KafkaProducer
    producer = KafkaProducer(bootstrap_servers='pulsar1:9092',retries=3)
    producer.send('rp4-kafka-1', json.dumps(row).encode('utf-8'))
    producer.flush()
    print("sent kafka")
    
    # Web Sockets
    import socket
    import requests
    import websocket, base64, json
    scheme = 'ws' # or wss
    TOPIC = scheme + '://pulsar1:8080/ws/v2/producer/persistent/public/default/energy'
    ws = websocket.create_connection(TOPIC)
    message = str(json.dumps(row) )
    message_bytes = message.encode('ascii')
    base64_bytes = base64.b64encode(message_bytes)
    base64_message = base64_bytes.decode('ascii')
    ws.send(json.dumps({ 'payload' : base64_message, 'properties': { 'device' : 'jetson2gb'},'key': str(uuid2), 'context' : 5 }))
    response =  json.loads(ws.recv())
    if response['result'] == 'ok':
            print ('Message published successfully')
    else:
            print ('Failed to publish message:', response)
    ws.close()
    
    # Native Pulsar Protocol
    import pulsar
    from pulsar.schema import *
    class enviroplus(Record):
        adjtemp = String()
    
    client = pulsar.Client('pulsar://pulsar1:6650')
    producer = client.create_producer(topic='persistent://public/default/rp4enviroplus' ,schema=JsonSchema(enviroplus),properties={"producer-name": 
               "enviroplus-py-sensor","producer-id": "enviroplus-sensor" })
    enviroRec = enviroplus()
    producer.send(enviroRec,partition_key=str(uniqueid))
    
````


### Communication to Apache Pulsar

* Via Pulsar Native Protocol
* Via Apache Kafka Protocol: (Kafka-on-Pulsar: KoP)
* Via MQTT Protocol (MQTT-on-Pulsar: MoP)
* via Web Sockets
* Via AMQP Protocol (AMQP-on-Pulsar: AoP) ie. RabbitMQ
* Via HTTP Post REST

### Pulsar Python Examples

* https://github.com/tspannhw/FLiP-Pi-BreakoutGarden
* https://github.com/tspannhw/pulsar-pychat-function
* https://github.com/tspannhw/FLiP-Py-Pi-GasThermal
* https://github.com/tspannhw/FLiP-PY-FakeDataPulsar
* https://github.com/tspannhw/FLiP-Py-Pi-EnviroPlus
* https://github.com/tspannhw/FLiP-Pi-Thermal
* https://github.com/tspannhw/FLiP-RP400
* https://github.com/tspannhw/PythonPulsarExamples
* https://github.com/tspannhw/FLiP-Pi-Weather
* https://github.com/tspannhw/pulsar-adafruit-funhouse
* https://github.com/tspannhw/FLiP-InfluxDB
* https://github.com/tspannhw/FLiP-Py-Pi-KoP
* https://github.com/tspannhw/FLiP-Energy/


### AoP, MoP, KoP, RoP

* https://github.com/streamnative/aop
* https://github.com/streamnative/rop
* https://github.com/streamnative/kop
* https://github.com/streamnative/mop


### References

* https://pulsar.apache.org/docs/en/client-libraries-python/
* https://www.wearedevelopers.com/world-congress/topics#/
* https://2022.pythonwebconf.com/presentations/apache-pulsar-development-101-with-python
* https://www.youtube.com/watch?v=qHyzF2MTpLg&t=1s
* https://www.slideshare.net/bunkertor/python-web-conference-2022-apache-pulsar-development-101-with-python-f-lippy
* https://websockets.readthedocs.io/en/stable/intro/index.html
* https://github.com/tspannhw/FLiP-IoT/blob/main/wsreader.py
