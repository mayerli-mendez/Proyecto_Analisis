# Proyecto_Analisis

## Integrantes : 

* Manuel Auqui
* Maria Jose Chala
* Leoni Guambo
* Mayerli Mendez
* Jorge Ortiz

# Desarrollo
### Fuente 1 

![Esta es una imagen](https://southcentralus1-mediap.svc.ms/transform/thumbnail?provider=spo&inputFormat=png&cs=fFNQTw&docid=https%3A%2F%2Fepnecuador-my.sharepoint.com%3A443%2F_api%2Fv2.0%2Fdrives%2Fb!YP5u9sklC0iywPgRepMQVOdg8BAkAQlLoHr_GSobHaPNJuBAAf_VQKAw4x81bXaz%2Fitems%2F01PVDBQAZNOIM6JNFQKBBZCYJCRLEVI4LK%3Fversion%3DPublished&access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIwMDAwMDAwMy0wMDAwLTBmZjEtY2UwMC0wMDAwMDAwMDAwMDAvZXBuZWN1YWRvci1teS5zaGFyZXBvaW50LmNvbUA2ODJhNGU2YS1hNzdmLTQ5NTgtYTNhYy05ZTI2NmQxOGFhMzciLCJpc3MiOiIwMDAwMDAwMy0wMDAwLTBmZjEtY2UwMC0wMDAwMDAwMDAwMDAiLCJuYmYiOiIxNjQ2MDkyODAwIiwiZXhwIjoiMTY0NjExNDQwMCIsImVuZHBvaW50dXJsIjoiTWwyby9jRUxiSDVZQ0NNcy9JeVlTQjBNM1VnY0ZVVDZKZk5kd2hITUZBQT0iLCJlbmRwb2ludHVybExlbmd0aCI6IjEyMCIsImlzbG9vcGJhY2siOiJUcnVlIiwidmVyIjoiaGFzaGVkcHJvb2Z0b2tlbiIsInNpdGVpZCI6IlpqWTJaV1psTmpBdE1qVmpPUzAwT0RCaUxXSXlZekF0WmpneE1UZGhPVE14TURVMCIsInNpZ25pbl9zdGF0ZSI6IltcImttc2lcIl0iLCJuYW1laWQiOiIwIy5mfG1lbWJlcnNoaXB8bWF5ZXJsaS5tZW5kZXpAZXBuLmVkdS5lYyIsIm5paSI6Im1pY3Jvc29mdC5zaGFyZXBvaW50IiwiaXN1c2VyIjoidHJ1ZSIsImNhY2hla2V5IjoiMGguZnxtZW1iZXJzaGlwfDEwMDMyMDAwNzEyOGU5ZmNAbGl2ZS5jb20iLCJzZXNzaW9uaWQiOiI0NDQ2NjI5MC0yYmRjLTQyNjQtOTc0Yi1hMjM0NjNiYjI2MWEiLCJ0dCI6IjAiLCJ1c2VQZXJzaXN0ZW50Q29va2llIjoiMyIsImlwYWRkciI6IjE1Ny4xMDAuMTcwLjExOCJ9.LzZIR2FuM09jdTBKNlFvc3lFVlJ0NytOcUp0d3BMM1pTTUl6djlvZTNzWT0&cTag=%22c%3A%7BE419722D-B0B4-4350-9161-228AC954716A%7D%2C1%22&encodeFailures=1&width=1366&height=581&srcWidth=&srcHeight=)

La fuente 1 consite en obtener datos de la red social de Twitter , estos datos guardarlos en la base de datos CouchDb y una vez guardados , importarlos a otra base en este caso SQL Server y finalmente analizar la informacion mediante la herramienta power. 

Lo primero que se realizo es la importacion de librerias :

```
import couchdb
import tweepy
import json
```
A continuacion se coloco las API:

```
###API ########################
ckey = 'SKepyS4uZlyBmXV9xtxQP9hN8'
csecret = '02DIS0kb8qGJpRAd1YfFn89Pmw73gSUGBVPJMWu5B8CD46rYNp'
atoken = '115946548-UmysCzkNSMsbdUqad25NO2TlV6KodCfBjJ2RjMDJ'
asecret = 'Lx1VT6yVeC2b3eQPmfyBMhQwKjViObqYZ6tF8dnUcJQU7'
```

Se creo una clase la cual permite la extraccion de datos y mediante un mensaje indica que los datos se han guardado o si ya existen :
```
class listener(tweepy.Stream):
    
    def on_data(self, data):
        dictTweet = json.loads(data)
        try:
            
            dictTweet["_id"] = str(dictTweet['id'])
            doc = db.save(dictTweet)
            print ("SAVED" + str(doc) +"=>" + str(data))
        except:
            print ("Already exists")
            pass
        return True
    
    def on_error(self, status):
        print (status)
```
Despues se paso clas APIS como parametro para tenern acceso a la informacion , tambien se realizo la conexion con la base de datos localmente , en donde se coloca el nombre del usuario y contraseña : 

```
twitter_stream = listener(ckey, csecret,atoken,asecret)
server = couchdb.Server('http://admin:12345@127.0.0.1:5984/')

```

Una vez realizada la conexion con la base de datos , se procede a crear la base de datos : 
```
try:
    db = server.create('dbproyecto')
except:
    db = server['dbproyecto']

```

Finalmente se hace la obtencion de datos mediante filtro de palabras : 

```
twitter_stream.filter(track=['delincuencia','violencia','robos','Quito'])

```

Una vez realizado la obtencion de datos se verifico en la base de datos CouchDB. 
Despues se procedio a Importar estos datos a la otra base que es SQL server , este procedimiento se lo realizo manualmente es decir se exporto e importo por interfaz grafica.
Una vez importado los datos en la segunda base de datos se procedio a importar a la herramienta PowerBi y se realizo el analisis. 







