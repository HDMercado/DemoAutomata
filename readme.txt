--LEEME--
Integrantes: Luis Ancavil
			 Hugo Mercado

---Instrucciones---
Para ingresar al sitio web se debe abrir el archivo index.html que corresponde al menu principal de la pagina creada.

Al ingresar se puede apreciar en el costado derecho varios puntos los cuales corresponde a las consultas que hemos creado.

A la parte superior donde dice "Musica Electronica" al clickear se puede regresar al menu principal.

Al hacer click a una de las consultas se ingresara a la pagina donde aparecen los resultados de la consulta.

Al hacer click en uno de los titulos redirecciona al link de donde se saco la informacion

En caso de querer ver los codigos de las consultar estan todas en archivos distintos en .html con un nombre caracteristico de la consulta correspondiente.


---Descripcion de las consultas---

--Generos de la musica electronica--
En esta consulta se muestra los distintos generos de la musica electronica con su descripción respectiva.

En esta consulta ingresamos a la dbpedia con la finalidad de sacar los generos que subdivide a la musica electronica, para ellos hicimos la siguiente consulta.

En primer lugar nos direccionamos a electronic music en la dbpedia para buscar a todos los
tipo musica electronica para ello estas 2 primeras lineas: 

?tipo dct:subject <http://dbpedia.org/resource/Category:Electronic_music_genres>.
?tipo dct:subject dbc:Electronic_music_genres.—> nos busca generos de la musica electronica

luego de buscar a los generos buscamos nombre y descripción de cada uno:
?tipo rdfs:label ?nombre.
?tipo dbo:abstract ?desc.} 


?tipo dct:subject <http://dbpedia.org/resource/Category:Electronic_music_genres>.\
?tipo dct:subject dbc:Electronic_music_genres.}.


luego de especializar nuestra consulta fuimos guardando informacion de los generos en variables:
?nombre    —> Nos devuelve el nombre del genero 
?desc        —> Nos devuelve una descripción del genero 

--Referentes de la musica electronica--
La informacion que entrega es nombre y una breve descripción de los artistas
relacionados con la musica electronica.
En esta consulta ingresamos a la dbpedia con la finalidad de sacar los artistas que producen música electrónica, para ellos hicimos la siguiente consulta.
En primer lugar nos direccionamos a electronic music en la dbpedia para buscar a todos los
tipo persona para ello estas 2 primeras lineas: 

?tipo dbo:genre <http://dbpedia.org/resource/Electronic_music>. 
?tipo rdf:type dbo:Person.      —> nos busca todos los tipos persona 

luego de buscar a los tipos persona buscamos a todas las personas que fueran 
productor/a de musica: 

?tipo <http://purl.org/linguistics/gold/hypernym> dbr:Producer.
?tipo dbo:genre dbr:Electronic_music.

luego de especializar nuestra consulta fuimos guardando informacion de estos productores en variables
?nombre    —> Nos devuelve el nombre de la persona 
?imagen    —> Nos devuelve una imagen de la persona
?desc        —> Nos devuelve una descripción de la persona 
?res           —> Un resumen de la persona 


--Dubstep (un subgenero de la musica electronica) --
Genero musical Dubstep que tiene relación directamente con música electrónica

- En esta consulta ingresamos a la dbpedia con la finalidad de sacar los artistas que producen 
dubstep (genero de música electrónica) para ellos hicimos la siguiente consulta.
En primer lugar nos direccionamos a electronic music en la dbpedia para buscar a todos los
tipo persona para ello estas 2 primeras lineas: 

?tipo dbo:genre <http://dbpedia.org/resource/Electronic_music>. 
?tipo rdf:type dbo:Person.      —> nos busca todo los tipo persona 

luego de buscar a los tipo persona buscamos a todas las personas que fueran 
productor/a de musica: 

?tipo <http://purl.org/linguistics/gold/hypernym> dbr:Producer.

luego todos las persona que fueran productores de dubstep: 

?tipo dct:subject dbc:Dubstep_musicians.

luego de especializar nuestra consulta fuimos guardando informacion de estos productores en variables
?nombre    —> Nos devuelve el nombre de la persona 
?imagen    —> Nos devuelve una imagen de la persona
?desc        —> Nos devuelve una descripción de la persona 
?res           —> Un resumen de la persona 
?nace.       —> Fecha de nacimiento de la persona


--Festivales que usan musica electronica--
Por ultimo esta consulta muestra los tipo de festivales en los que se reproducen musica electronica, tanto con otros tipos de musica como solo musica electronica.

En esta consulta ingresamos a la dbpedia con la finalidad de sacar los festivales que reproducen música electrónica, para ellos hicimos la siguiente consulta.
En primer lugar nos direccionamos a electronic music en la dbpedia para buscar a todos los tipos de festivales de música electronica en estas 2 primeras lineas: 

?tipos dct:subject <http://dbpedia.org/resource/Category:Electronic_music_festivals>.
?tipos <http://purl.org/linguistics/gold/hypernym> dbr:Festival.

luego de buscar a los tipos persona buscamos a todas las personas que fueran 
productor/a de musica: 

?tipo <http://purl.org/linguistics/gold/hypernym> dbr:Producer.
?tipo dbo:genre dbr:Electronic_music.

luego de especializar nuestra consulta fuimos guardando informacion de estos productores en variables
?nombre    —> Nos devuelve el nombre del festival
?image    —> Nos devuelve una imagen del festival
?desc      —> Nos devuelve una descripción del festival 


--filtros y otros--
Para poder filtrar solo la busqueda en ingles y no se nos repitieran los mismas productores en diferentes idiomas
utilizamos un FILTER(LANG) para poder sacar solo los datos en ingles “en”: 

FILTER(LANG(?nombre) = "en" && LANG(?desc) = "en" && LANG(?res) = "en")
*Filtramos el nombre, la descripción, y el resumen de cada productor.

Los problemas que tuvimos en esta consulta fueron que los productores tenían doble fecha de nacimiento en la dbpedia (consult dubstep):
• 1988-09-26 
• 1988-9-25
para solucionar esto y que solo nos mostrara una fecha de nacimiento utilizamos la función
BIND(str) BIND(strlen) y un FILTER:

?nace ?len2 ?len3


BIND (str(?nace) AS ?len2).\
BIND (strlen(?len2) AS ?len3).\
FILTER(?len3 = 10)

Lo primero que hicimos fue convertir la fecha de nacimiento en un string, luego utlizamos el BIND(strlen) para calcular la
longitud del string, en el primer caso tenemos que la longitud del string es 10 en cambio la segunda tiene una longitud de 9,
luego de guardar ambos datos en sus respectivas variables (?len2 y ?len3), utilizamos el filter para mostrar solo los string que 
tuvieran una longitud de 10 por lo cual solo nos mostraría la primera fecha ya que esta tiene una longitud de 10.

---Consultas---
--Generos de musica electronica--
        select ?tipo ?nombre ?imagen ?desc ?res 
          where{
            {?tipo dbo:genre <http://dbpedia.org/resource/Electronic_music>.
            ?tipo rdf:type dbo:Person.
            ?tipo <http://purl.org/linguistics/gold/hypernym> dbr:Producer.
            ?tipo dbo:genre dbr:Electronic_music.
            ?tipo rdfs:label ?nombre.
            ?tipo dbo:thumbnail ?imagen.
            ?tipo dct:description ?desc.
            ?tipo dbo:abstract ?res.}
            UNION
            {?tipo dbo:genre <http://dbpedia.org/resource/Electronic_music>.
            ?tipo rdf:type dbo:Person.
            ?tipo <http://purl.org/linguistics/gold/hypernym> dbr:Producer.
            ?tipo dbo:genre dbr:Electronic_music.}
            FILTER(LANG(?nombre) = "en" && LANG(?res)= "en")
      }

--Referentes de la musica electronica--
        select ?tipo ?nombre ?imagen ?desc ?res 
          where{
            {?tipo dbo:genre <http://dbpedia.org/resource/Electronic_music>.
            ?tipo rdf:type dbo:Person.
            ?tipo <http://purl.org/linguistics/gold/hypernym> dbr:Producer.
            ?tipo dbo:genre dbr:Electronic_music.
            ?tipo rdfs:label ?nombre.
            ?tipo dbo:thumbnail ?imagen.
            ?tipo dct:description ?desc.
            ?tipo dbo:abstract ?res.}
            UNION
            {?tipo dbo:genre <http://dbpedia.org/resource/Electronic_music>.
            ?tipo rdf:type dbo:Person.
            ?tipo <http://purl.org/linguistics/gold/hypernym> dbr:Producer.
            ?tipo dbo:genre dbr:Electronic_music.}
            FILTER(LANG(?nombre) = "en" && LANG(?res)= "en")
      }

--Referentes de la musica electronica Dubstep--
        select ?tipo ?nombre ?imagen ?desc ?res ?nace ?len2 ?len3 
          where{
            {?tipo dbo:genre <http://dbpedia.org/resource/Electronic_music>.
            ?tipo rdf:type dbo:Person.
            ?tipo <http://purl.org/linguistics/gold/hypernym> dbr:Producer.
            ?tipo dct:subject dbc:Dubstep_musicians.
            ?tipo rdfs:label ?nombre.
            ?tipo dbo:thumbnail ?imagen.
            ?tipo dct:description ?desc.
            ?tipo dbo:abstract ?res.
            ?tipo dbo:birthDate ?nace.
            BIND (str(?nace) AS ?len2).
            BIND (strlen(?len2) AS ?len3).}
          UNION
          {?tipo dbo:genre <http://dbpedia.org/resource/Electronic_music>.
            ?tipo rdf:type dbo:Person.
            ?tipo <http://purl.org/linguistics/gold/hypernym> dbr:Producer.
            ?tipo dct:subject dbc:Dubstep_musicians.
            FILTER(LANG(?nombre) = "en" && LANG(?desc) = "en" && LANG(?res) = "en").
            BIND (str(?nace) AS ?len2).
            BIND (strlen(?len2) AS ?len3).}
            FILTER(LANG(?nombre) = "en" && LANG(?desc) = "en" && LANG(?res) = "en")
            FILTER(?len3 = 10)
          }

--Festivales que usan musica electronica--
        select ?tipos ?nombre ?desc ?image
         where{
          {?tipos dct:subject <http://dbpedia.org/resource/Category:Electronic_music_festivals>.
          ?tipos <http://purl.org/linguistics/gold/hypernym> dbr:Festival.
          ?tipos rdfs:label ?nombre.
          ?tipos rdfs:comment ?desc.
          ?tipos dbo:thumbnail ?image.}
          UNION
          {?tipos dct:subject <http://dbpedia.org/resource/Category:Electronic_music_festivals>.
          ?tipos <http://purl.org/linguistics/gold/hypernym> dbr:Festival.}
          FILTER(LANG(?nombre)= "en" && LANG(?desc)= "en")
      }



