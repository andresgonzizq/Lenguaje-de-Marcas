# XQuery

_Actividad XML - XQuery 1._

### Ejercicio 1

Dado el siguiente documento XML realiza las siguientes consultas con XQuery:

```
<?xml version="1.0" encoding="UTF-8"?>
<bookstore>
  <book category="COOKING">
    <title lang="en">Everyday Italian</title>
    <author>Giada De Laurentiis</author>
    <year>2005</year>
    <price>30.00</price>
  </book>
  <book category="CHILDREN">
    <title lang="en">Harry Potter</title>
    <author>J K. Rowling</author>
    <year>2005</year>
    <price>29.99</price>
  </book>
  <book category="WEB">
    <title lang="en">XQuery Kick Start</title>
    <author>James McGovern</author>
    <author>Per Bothner</author>
    <author>Kurt Cagle</author>
    <author>James Linn</author>
    <author>Vaidyanathan Nagarajan</author>
    <year>2003</year>
    <price>49.99</price>
  </book>
  <book category="WEB">
    <title lang="en">Learning XML</title>
    <author>Erik T. Ray</author>
    <year>2003</year>
    <price>39.95</price>
  </book>
</bookstore> 
```
### 1.	Mostrar los títulos de los libros con la etiqueta "titulo".
```
for $book in //book/title/text()
return <titulo>{$book}</titulo>
```
</br>

```
<titulo>Everyday Italian</titulo>
<titulo>Harry Potter</titulo>
<titulo>XQuery Kick Start</titulo>
<titulo>Learning XML</titulo>
```
### 2.	Mostrar los libros cuyo precio sea menor o igual a 30. Primero incluyendo la condición en la cláusula "where" y luego en la ruta del XPath.
```
for $book in /bookstore/book
where $book/price<=30
return $book
```
</br>

```
<book category="COOKING">
  <title lang="en">Everyday Italian</title>
  <author>Giada De Laurentiis</author>
  <year>2005</year>
  <price>30.00</price>
</book>
<book category="CHILDREN">
  <title lang="en">Harry Potter</title>
  <author>J K. Rowling</author>
  <year>2005</year>
  <price>29.99</price>
</book>
```
### 3.	Mostrar sólo el título de los libros cuyo precio sea menor o igual a 30.
```
for $book in /bookstore/book
where $book/price<=30
return $book/title
```
</br>

```
<title lang="en">Everyday Italian</title>
<title lang="en">Harry Potter</title>
```
### 4.	Mostrar sólo el título sin atributos de los libros cuyo precio sea menor o igual a 30.
```
for $book in /bookstore/book[price<=30]
return <title>{$book/title/text()}</title>
```
</br>

```
<title>Everyday Italian</title>
<title>Harry Potter</title>
```
### 5.	Mostrar el título y el autor de los libros del año 2005, y etiquetar cada uno de ellos con "lib2005".
```
for $book in /bookstore/book
where $book/year=2005
return <lib2005>{$book/title,$book/author}</lib2005>
```
</br>

```
<lib2005>
  <title lang="en">Everyday Italian</title>
  <author>Giada De Laurentiis</author>
</lib2005>
<lib2005>
  <title lang="en">Harry Potter</title>
  <author>J K. Rowling</author>
</lib2005>
```
### 6.	Mostrar los años de publicación, primero con "for" y luego con "let" para comprobar la diferencia entre ellos. Etiquetar la salida con "publicacion".
```
for $book in /bookstore/book/year
return <publicacion>{$book}</publicacion>
```
</br>

```
<publicacion>
  <year>2005</year>
</publicacion>
<publicacion>
  <year>2005</year>
</publicacion>
<publicacion>
  <year>2003</year>
</publicacion>
<publicacion>
  <year>2003</year>
</publicacion>
```
### 7.	Mostrar los libros ordenados primero por "category" y luego por "title" en una sola consulta.
```
for $book in /bookstore/book
order by $book/@category,$book/title
return $book
```
</br>

```
<book category="CHILDREN">
  <title lang="en">Harry Potter</title>
  <author>J K. Rowling</author>
  <year>2005</year>
  <price>29.99</price>
</book>
<book category="COOKING">
  <title lang="en">Everyday Italian</title>
  <author>Giada De Laurentiis</author>
  <year>2005</year>
  <price>30.00</price>
</book>
<book category="WEB">
  <title lang="en">Learning XML</title>
  <author>Erik T. Ray</author>
  <year>2003</year>
  <price>39.95</price>
</book>
<book category="WEB">
  <title lang="en">XQuery Kick Start</title>
  <author>James McGovern</author>
  <author>Per Bothner</author>
  <author>Kurt Cagle</author>
  <author>James Linn</author>
  <author>Vaidyanathan Nagarajan</author>
  <year>2003</year>
  <price>49.99</price>
</book>
```
### 8.	Mostrar cuántos libros hay, y etiquetarlo con "total".
```
let $cantidad_libro:=count(/bookstore/book)
return <total>{$cantidad_libro}</total>
```
</br>

```
<total>4</total>
```
### 9.	Mostrar los títulos de los libros y al final una etiqueta con el número total de libros.
```
let $cantidad:=count(/bookstore/book),
    $title:=(for $book in /bookstore/book/title 
    return <titulo>{$book/text()}</titulo>) 
return 
      <resultado>{$title}<total_libros>{$cantidad}</total_libros></resultado>
```
</br>

```
<resultado>
  <titulo>Everyday Italian</titulo>
  <titulo>Harry Potter</titulo>
  <titulo>XQuery Kick Start</titulo>
  <titulo>Learning XML</titulo>
  <total_libros>4</total_libros>
</resultado>
```
### 10.	Mostrar el precio mínimo y máximo de los libros.
```
let $cantidad_maxima := max(/bookstore/book/price), 
    $cantidad_minima := min(/bookstore/book/price)
return
<resultado>
  <maximo>{$cantidad_maxima}</maximo>
  <minimo>{$cantidad_minima}</minimo>
</resultado>
```
</br>

```
<resultado>
  <maximo>49.99</maximo>
  <minimo>29.99</minimo>
</resultado>
```
### 11.	Mostrar el título del libro, su precio y su precio con el IVA incluido, cada uno con su propia etiqueta. Ordénalos por precio con IVA.
```
for $book in /bookstore/book
let $iva := ($book/price * 1.21)
order by $precio_iva
return 
<libro><titulo>{$book/title/text()}</titulo><precio>{$book/price/text()} €</precio><precio_iva>{$iva} €</precio_iva></libro>
```
</br>

```
<libro>
  <titulo>Harry Potter</titulo>
  <precio>29.99 €</precio>
  <precio_iva>36.2879 €</precio_iva>
</libro>
<libro>
  <titulo>Everyday Italian</titulo>
  <precio>30.00 €</precio>
  <precio_iva>36.3 €</precio_iva>
</libro>
<libro>
  <titulo>Learning XML</titulo>
  <precio>39.95 €</precio>
  <precio_iva>48.3395 €</precio_iva>
</libro>
<libro>
  <titulo>XQuery Kick Start</titulo>
  <precio>49.99 €</precio>
  <precio_iva>60.4879 €</precio_iva>
</libro>
```
### 12.	Mostrar la suma total de los precios de los libros con la etiqueta "total".
```
let $book := /bookstore/book
return <total>{sum($book/price)}</total>
```
</br>

```
<total>149.93</total>
```
### 13.	Mostrar cada uno de los precios de los libros, y al final una nueva etiqueta con la suma de los precios.
```
<libros>
{
  for $book in /bookstore/book
  return $book/price
}
{
  let $suma := /bookstore/book
  return <total>{sum($suma/price)}</total>
}
</libros>
```
</br>

```
<libros>
  <price>30.00</price>
  <price>29.99</price>
  <price>49.99</price>
  <price>39.95</price>
  <total>149.93</total>
</libros>
```
### 14.	Mostrar el título y el número de autores que tiene cada título en etiquetas diferentes.
```
for $book in /bookstore/book
return 
  <libro>{$book/title}<autores>{count($book/author)}</autores></libro>
```
</br>

```
<libro>
  <title lang="en">Everyday Italian</title>
  <autores>1</autores>
</libro>
<libro>
  <title lang="en">Harry Potter</title>
  <autores>1</autores>
</libro>
<libro>
  <title lang="en">XQuery Kick Start</title>
  <autores>5</autores>
</libro>
<libro>
  <title lang="en">Learning XML</title>
  <autores>1</autores>
</libro>
```
### 15.	Mostrar en la misma etiqueta el título y entre paréntesis el número de autores que tiene ese título.
```
for $book in /bookstore/book
return <libro>{$book/title/text()}({count($book/author)})</libro>
```
</br>

```
<libro>Everyday Italian (1)</libro>
<libro>Harry Potter (1)</libro>
<libro>XQuery Kick Start (5)</libro>
<libro>Learning XML (1)</libro>
```
### 16.	Mostrar los libros escritos en años que terminen en "3".
```
for $book in /bookstore/book
where ends-with($book/year, "3")
return $book
```
</br>

```
<book category="WEB">
  <title lang="en">XQuery Kick Start</title>
  <author>James McGovern</author>
  <author>Per Bothner</author>
  <author>Kurt Cagle</author>
  <author>James Linn</author>
  <author>Vaidyanathan Nagarajan</author>
  <year>2003</year>
  <price>49.99</price>
</book>
<book category="WEB">
  <title lang="en">Learning XML</title>
  <author>Erik T. Ray</author>
  <year>2003</year>
  <price>39.95</price>
</book>
```
### 17.	Mostrar los libros cuya categoría empiece por "C".
```
for $book in /bookstore/book
where starts-with($book/@category, "C")
return $book
```
</br>

```
<book category="COOKING">
  <title lang="en">Everyday Italian</title>
  <author>Giada De Laurentiis</author>
  <year>2005</year>
  <price>30.00</price>
</book>
<book category="CHILDREN">
  <title lang="en">Harry Potter</title>
  <author>J K. Rowling</author>
  <year>2005</year>
  <price>29.99</price>
</book>
```
### 18.	Mostrar los libros que tengan una "X" mayúscula o minúscula en el título.
```
for $book in /bookstore/book
where contains($book/title, "x") or contains($book/title, "X") order by $book/title descending
return $book
```
</br>

```
<book category="WEB">
  <title lang="en">XQuery Kick Start</title>
  <author>James McGovern</author>
  <author>Per Bothner</author>
  <author>Kurt Cagle</author>
  <author>James Linn</author>
  <author>Vaidyanathan Nagarajan</author>
  <year>2003</year>
  <price>49.99</price>
</book>
<book category="WEB">
  <title lang="en">Learning XML</title>
  <author>Erik T. Ray</author>
  <year>2003</year>
  <price>39.95</price>
</book>
```
### 19.	Mostrar el título y el número de caracteres que tiene cada título, cada uno con su propia etiqueta.
```
for $book in /bookstore/book
return 
  <libro>{$book/title}<length>{string-length($book/title)}</length></libro>
```
</br>

```
<libro>
  <title lang="en">Everyday Italian</title>
  <length>16</length>
</libro>
<libro>
  <title lang="en">Harry Potter</title>
  <length>12</length>
</libro>
<libro>
  <title lang="en">XQuery Kick Start</title>
  <length>17</length>
</libro>
<libro>
  <title lang="en">Learning XML</title>
  <length>12</length>
</libro>
```
### 20.	Mostrar todos los años en los que se ha publicado un libro eliminando los repetidos. Etiquétalos con "año".
```
for $año in distinct-values(/bookstore/book/year)
return <año>{$año}</año>
```
</br>

```
<año>2005</año>
<año>2003</año>
```
### 21.	Mostrar todos los autores eliminando los que se repiten y ordenados por el número de caracteres que tiene cada autor.
```
for $author in distinct-values(/bookstore/book/author)
order by string-length($author)
return <autor>{$author}</autor>
```
</br>

```
<autor>Kurt Cagle</autor>
<autor>James Linn</autor>
<autor>Per Bothner</autor>
<autor>Erik T. Ray</autor>
<autor>J K. Rowling</autor>
<autor>James McGovern</autor>
<autor>Giada De Laurentiis</autor>
<autor>Vaidyanathan Nagarajan</autor>
```
### 22.	Mostrar los títulos en una tabla de HTML.
```
<table>
{for $book in /bookstore/book return <tr><td>{$book/title/text()}</td></tr>}
</table>
```
</br>

```
<table>
  <tr>
    <td>Everyday Italian</td>
  </tr>
  <tr>
    <td>Harry Potter</td>
  </tr>
  <tr>
    <td>XQuery Kick Start</td>
  </tr>
  <tr>
    <td>Learning XML</td>
  </tr>
</table>
```
