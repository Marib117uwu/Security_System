# Secure Tenant

## Integrantes
- Maribel Ramírez Torres  1222100479

## Carta de liberación del proyecto
![image](https://github.com/Marib117uwu/Security_System/assets/135056294/7dc6fe49-b3a6-4fa6-ae2e-fd06af977968)


## Lista del Hardware utilizado
| Id | Componente   |             Descripción                          | Imagen                                                                                                       | Cantidad | Costo total |
|----|--------------|--------------------------------------------------|--------------------------------------------------------------------------------------------------------------|----------|-------------|
|  1 |    ESP32     |Placa de desarrollo para uso de IoT.              |![image](https://github.com/Marib117uwu/Security_System/assets/135056294/708ac727-2b28-41b0-9c38-a776de545c51)|     1    |     $200    |
|  2 | Botón        |Botón sencillo programable color negro.           |![image](https://github.com/Marib117uwu/Security_System/assets/135056294/17ded99e-7e5e-4858-8269-f94d1e611e49)|     1    |      $15    |
|  3 |Sensor Buzzer |Transductor electroacústico que produce un sonido.|![image](https://github.com/Marib117uwu/Security_System/assets/135056294/eb36929c-db29-427b-9e8c-19d40a92ff96)|     1    |      $25    |
|  4 | LED          |Emisor de luz color rojo.                         |![image](https://github.com/Marib117uwu/Security_System/assets/135056294/88151dbb-041b-41e6-8450-34b507630de1)|     1    |      $3     |


## Lista de Software utilizado
| Id | Software | Version | 
|----|----------|---------|
|  1 |  Thonny  | 2.7.18  |         
|  2 | Node-Red | 3.1.9   |         
|  3 |PostgreSQL| 16.1    | 
|  4 |  Docker  | 26.1.4  |

## Visión del producto
Visión:

    Para inquilinos de departamentos o cuartos, quienes tienen la necesidad de sentirse seguros sin que personas ajenas invadan su privacidad 
    o que se sientan atacados, el producto Secure Tenant es un mecanismo de seguridad especialmente para personas que viven en constante inseguridad. 
    El producto Secure Tenant permite el control de una alarma que es activada mediante un botón en cuanto se tenga una emergencia. 

## Conexiones
- Imagenes de Wokwi, Fritzing, o de otro software que me permita mostrar las conexiones del circuito.
- Raspberry Pi
- ESP 32
- ![image](https://github.com/Marib117uwu/Security_System/assets/135056294/be904088-81b8-47f8-9eaf-8764610723f6)

## Funcionalidades

| Id |                             Historia de usuario                                           | Prioridad | Estimación |              Como probarlo                           |     Responsable        |
|----|-------------------------------------------------------------------------------------------|-----------|------------|------------------------------------------------------|------------------------|
|  1 | Como inquilino quiero que el mecanismo me permita encender la alarma con un botón.        |   Alta    |   2 días   |Poniendo a prueba el funcionamiento del código.       | Maribel Ramírez Torres |
|  2 | Como inquilino quiero que el sistema de seguridad sea sencillo de manipular.              |   Alta    |   2 horas  |Haciendo pruebas de uso del prototipo.                | Maribel Ramírez Torres |
|  3 | Como inquilino quiero que se registre fecha y hora cuando haya emergencias.               |   Alta    |   1 día    |Dar click en el botón y verificar si se hizo registro.| Maribel Ramírez Torres |

## Prototipo en dibujo
- ![image](https://github.com/Marib117uwu/Security_System/assets/135056294/95a5789d-87df-4374-b587-ce96f2cd02d9)


# Evidencias de funcionamiento
- Captura de pantalla de flujos de Node RED
  ![image](https://github.com/Marib117uwu/Security_System/assets/135056294/19a9f9e1-904c-4d36-aa29-00eca8377e8f)

- Captura de las pantallas del proyecto DASHBOARD y Pantalla de la ESP32
  DASHBOARD
  ![image](https://github.com/Marib117uwu/Security_System/assets/135056294/bf307d3f-a2c5-4288-a8eb-c746cc3f88dc)
  Pantalla ESP32
  ![image](https://github.com/Marib117uwu/Security_System/assets/135056294/d3e1ec5d-a5af-44ba-bb2e-905f0463ff04)

- Video demostrativo de las funcionalidades del proyecto
  ENLACE
  https://mega.nz/file/lSt2QRxJ#v3ZzwyyO-y8jYfQYM7lAeV7ug6pEpqK6qQ_VOAj0Ah4
  
- Código fuente (PROHIBIDO PONER COMPRIMIDOS)
  
        import time
        from machine import Pin, PWM
        from umqtt.simple import MQTTClient
        import json
        
        ssid = "OPPOA17"
        password = "z99sksdu"
        mqtt_server = "192.168.221.21"
        topicboton = "puerta/estado"
        topicvirtual = "estado/virtual"
        client_id = "ESP32"
        
        # Configuración del botón
        boton_pin = 12
        boton = Pin(boton_pin, Pin.IN, Pin.PULL_UP)
        
        # Configuración de led
        led = Pin(4, Pin.OUT)
        
        # Configuración de buzzer
        buzzer = PWM(Pin(13), freq=440, duty=0)
        
        # Función para emitir sonidos
        def sonido(frecuencia, duracion):
            buzzer.freq(frecuencia)
            buzzer.duty(512)
            time.sleep(duracion)
            buzzer.duty(0)
        
        # Conectar a la red WiFi
        def connect_wifi():
            wlan = network.WLAN(network.STA_IF)
            wlan.active(True)
            wlan.connect(ssid, password)
            while not wlan.isconnected():
                print('Conectando a la red WiFi...')
                time.sleep(1)
            print('Conectado a la red WiFi:', wlan.ifconfig())
        
        # Función de callback para manejar los mensajes MQTT entrantes
        def mqtt_callback(topic, msg):
            print("Mensaje recibido en el tópico {}: {}".format(topic, msg))
            if topic == b'estado/virtual':
                print("Mensaje en el tópico estado/virtual: {}".format(msg))
                if msg == b'1' or msg == '1':  # Añade esta condición para manejar bytes o cadenas
                    print("Encendiendo LED desde el dashboard")
                    led.value(1)
                    sonido(1915, 0.2)
                    sonido(1700, 0.2)
                    sonido(1915, 0.2)
                    sonido(1700, 0.2)
                    sonido(1915, 0.2)
                    sonido(1700, 0.2)
                    sonido(1915, 0.2)
                    sonido(1700, 0.2)
                    sonido(1915, 0.2)
                    sonido(1700, 0.2)
                    sonido(1915, 0.2)
                    sonido(1700, 0.2)
                elif msg == b'0' or msg == '0':  # Añade esta condición para manejar bytes o cadenas
                    print("Apagando LED desde el dashboard")
                    buzzer.duty(0)
                    led.value(0)
        
        
        # Conectar al servidor MQTT y suscribirse a un tema
        def connect_mqtt():
            client = MQTTClient(client_id, mqtt_server)
            client.set_callback(mqtt_callback)
            client.connect()
            client.subscribe(topicvirtual)
            print('Conectado al servidor MQTT y suscrito a', topicvirtual)
            return client
        
        def alarma(hour):
            if 1 <= hour <= 16:
                led.value(1)
                sonido(1915, 0.2)
                sonido(1700, 0.2)
                sonido(1915, 0.2)
                sonido(1700, 0.2)
                sonido(1915, 0.2)
                sonido(1700, 0.2)
                sonido(1915, 0.2)
                sonido(1700, 0.2)
                sonido(1915, 0.2)
                sonido(1700, 0.2)
                sonido(1915, 0.2)
                sonido(1700, 0.2)
                time.sleep(6)
                buzzer.duty(0)
                led.value(0)
        
        # Mandar el estado del boton
        
        # Mandar el estado del botón
        def publicarPuerta(client):
            estadoPrev = boton.value()
            while True:
                estadoAct = boton.value()
                if estadoPrev != estadoAct:
                    estado = "abierta" if estadoAct == 0 else "cerrada"
                    current_time = time.localtime()
                    year = current_time[0]
                    month = current_time[1]
                    day = current_time[2]
                    hour = current_time[3]
                    minute = current_time[4]
                    alarma(hour)
                    fecha_formateada = "{:04d}-{:02d}-{:02d}".format(year, month, day)
                    hora_formateada = "{:02d}:{:02d}".format(hour, minute)
                    msg = {'estado': estado, 'fecha': fecha_formateada, 'hora': hora_formateada}
                    client.publish(topicboton, json.dumps(msg))
                    print(msg)
                    estadoPrev = estadoAct
                    # Espera a que cambie el estado del botón para volver a comprobar
                    while boton.value() == estadoAct:
                        time.sleep(0.1)
                else:
                    time.sleep(0.1)
                time.sleep(0.1)
        
        # Main loop
        def main():
            connect_wifi()
            client = connect_mqtt()
            try:
                while True:
                    client.check_msg()
                    publicarPuerta(client)
                    time.sleep(0.1) 
            finally:
                client.disconnect()
        
        if __name__ == "__main__":
            main()



