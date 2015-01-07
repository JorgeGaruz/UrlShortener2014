

# Project "MediumCandy"

The color medium candy apple red, applied with a metallic sheen, is popular among car owners who customize their cars.

# Team Members

* Carlos Mart�nez Castillero (leader)
* Daniel Garc�a Paez
* Alberto Berbel Aznar
* Guillermo Ginestra D�az

# Funcionalidades implementadas

A continuaci�n, se dar� una descripci�n de cada una de las funcionalidades que se han implementado en este proyecto.

## Servicio que almacena URLs alcanzables

Se ha implementado un Servicio que se encarga de comprobar que una URL es alcanzable previamente a ser acortada y almacenada en el sistema. Tal y como se acord�, el Servicio implementado se corresponde con el **Tipo 2 de implementaci�n**.

El servicio principal que se encarga de responder a las peticiones se encuentra en el `MediumCandyController` y es accesible a trav�s de la uri mostrada a continuaci�n (siendo el par�metro `url` la URL que se desea acortar): 

- `http://URI_DEL_SERVIDOR/mediumcandy/linkreachable?url=URL` 

El servicio responder� con: 

- `CREATED (201)` en el caso de que la URL proporcionada sea alcanzable (y correctamente formada, por supuesto).
- `BAD REQUEST (400)` en el caso de que la URL que se desea acortar no sea alcanzable (o en su defecto, no se trate de una URL correctamente formada).
 
La funcionalidad m�s importante se encuentra implementada en el `UrlShortenerControllerWithLogs`, Servicio al que accede el `MediumCandyController` para poder dar respuesta a todas las peticiones que le llegan en `/mediumcandy/linkreachable`. Cabe destacar la funci�n `private static boolean ping(String urlIn)` que determina si `urlIn` es una URL alcanzable. Aspectos destacables acerca de su implementaci�n son:

- La utilizaci�n de la clase `HttpURLConnection` que permite realizar peticiones *http*.
- La existencia de un *timeout* de varios segundos, tras el cual si no hemos recibido respuesta tras realizar una petici�n cierra la conexi�n y determina que la URL dada no es alzanzable.
- En el caso de obtener respuesta accedemos a sus cabeceras *http* y comprobamos que el c�digo recibido es v�lido y se trata por tanto de de una URL alcanzable.



## Acortamiento masivo de URLs mediante fichero CSV

## Servicio de personalizaci�n de URLs acortadas

## Informaci�n y estad�sticas de URLs

Funcionalidad que consiste en mostrar estad�sticias de las URLs almacenadas en la Base de Datos con su URL corta implementado con **nivel tecnol�gico Tipo 2** como acordamos. 

Cada vez que alguien accede a una direcci�n con una URL corta, se almacena en la Base de Datos. Para conocer las estad�sticas de una direcci�n basta con hacer una petici�n get a la siguiente direcci�n del servidor, donde URL es la direcci�n del cual se quieren obtener las estad�sticias e informaci�n: 

- `http://URI_DEL_SERVIDOR/mediumcandy/linkstats?url=URL`  

Esta direcci�n accede al `MediumCandyController` la cual a su vez llama a `UrlShortenerControllerWithLogs`. Este controller se encarga de acceder a la Base de Datos para buscar el objeto ShortURL, y con el hash de este recuperar de la Base de Datos el n�mero de clicks (veces que se a accedido a la direcci�n dada por par�metro). 

Este m�todo devuelve lo siguiente: 

- `OK (200)` junto con la lista de las estad�sticias, en el caso de que todo haya ido bien y haya en la Base de Datos una ShortURL con la direcci�n dada.  
- `BAD REQUEST (400)` en el caso de que la URL dada no este en la Base de Datos
