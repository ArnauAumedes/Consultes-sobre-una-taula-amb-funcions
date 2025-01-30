## 1.2.3 Consultes sobre una taula amb funcions

1. Llista totes les columnes de la taula empleats.
```mysql
SELECT *
  FROM empleats;
```
2. Llista els cognoms de tots els empleats.
```mysql
SELECT cognoms
  FROM empleats;
```
3. Llista els cognoms dels empleats eliminant els cognoms que estiguin repetits.
```mysql
SELECT DISTINC cognoms
  FROM empleats;
```
4. Llista el nom i els cognoms de tots els empleats.
```mysql
SELECT nom, cognoms
  FROM empleats;
```
5. Mostra els cognoms i nom dels empleats concatenats amb una coma i un
espai en blanc.
```mysql
SELECT CONCAT(cognoms, ', ', nom) AS nom_complet
  FROM empleats;
```
6. Volem una columna on estigui tot en majúscules i l’altre tot en minúscules. Anomena les columnes com a "nom_majuscules" i "nom_minuscules" respectivament.
```mysql
SELECT
  UPPER(CONCAT(cognoms, ', ', nom)) AS nom_complet_majuscules
  LOWER(CONCAT(cognoms, ', ', nom)) AS nom_complet_minusculas
FROM empleats;
```
7. Mostra les 6 primeres lletres dels cognoms dels empleats
```mysql
SELECT LEFT(cognoms, 6) AS primeres_6_lletres
  FROM empleats;
```
8. Quins són els empleats que tenen la longitud del cognoms major a 6? (Mostra els cognoms i la longitud)
```mysql
SELECT cognoms, LENGTH(cognoms) AS longitud 
	FROM empleats
WHERE LENGTH(cognoms) > 6;
```
12. Substitueix totes les 'a' dels cognoms dels empleats per 'e'. Ordena pel nou valor dels cognoms
```mysql
SELECT cognoms 
	FROM empleats
WHERE SUBSTRING(LOWER(TRIM(cognoms)), 2, 1) = 'a';
````
13. Mostra tots els empleats que tenen en la segona posició dels cognoms una 'a'. (Sense utilitzar l’operador LIKE, ni REGEXP)
```mysql
SELECT empleat_id, cognoms, ROUND(salari * 1.15) AS nou_salari
	FROM empleats;
````
14. Per cada empleat mostra el codi d’empleat, cognom i el salari amb un augment del 15% expressat com un número enter, etiqueta la columna amb el nom "nou_salari".
```mysql
SELECT empleat_id, cognoms, ROUND(salari * 1.15) AS nou_salari
	FROM empleats;
````
15. Partint de la consulta anterior, afegeix una nova columna que mostri la diferencia salarial entre el nou salari i l’antic. Etiqueta la columna com a "increment"
```mysql
SELECT empleat_id, cognoms, 
    ROUND(salari * 1.15) AS nou_salari,
    ROUND(salari * 1.15) - salari AS increment
	FROM empleats;
````
16. Fes una consulta on mostri el cognom de l’empleat en majúscules i la longitud del cognom dels empleats on el seu cognom comenci per J, A o M. Ordena els resultats per cognom d’empleat.
```mysql
SELECT UPPER(cognoms) AS cognom_majuscules, LENGTH(cognoms) AS longitud
	FROM empleats
WHERE LEFT(cognoms, 1) IN ('J', 'A', 'M')
ORDER BY cognoms;

````
17. Fes una consulta on mostri el codi, nom i cognoms dels empleats que van ser contractats un dilluns o un divendres.
```mysql
SELECT empleat_id, nom, cognoms
	FROM empleats
WHERE DAYOFWEEK(data_contractacio) IN (2, 6);
````
18. Mostra l’import de comissió dels empleats. Etiqueta la columna com a "import_comissio" i arrodoneix el valor a 2 decimals. Si un empleat no té assignada comissió fes el que calgui per tal que el resultat de l’operació surti zero en comptes de NULL. Mostra també el cognom i el salari en Ptes despreciant els decimals (truncar)
```mysql
SELECT cognoms,
    TRUNCATE(pct_comissio, 0) AS salari_truncat,
    IFNULL(pct_comissio, 0) AS pct_comissio,
    ROUND(pct_comissio, 2) AS import_comissio
	FROM empleats;
````
19. Utilitzant alguna de les funcions de dates, obté el nom, cognom i data de contractació dels empleats contractats durant el 1997.
```mysql
SELECT nom, cognoms, data_contractacio
	FROM empleats
WHERE YEAR(data_contractacio) = 1997;
````
20. Utilitzant alguna de les funcions de data, mostra tots els empleats que van ser contractats entre els mesos de juny i novembre, independentment de l’any.
```mysql
SELECT nom, data_contractacio 
	FROM empleats
WHERE MONTH(data_contractacio) BETWEEN 6 AND 11
ORDER BY data_contractacio;
````
21. El nostre departament de recursos humans s’aborreix molt i per justificar el seu sou està elaborant tot d’estadístiques extranyes. Ara volen saber quins empleats varen ser contractats en un dia parell.
```mysql
SELECT nom, data_contractacio
	FROM empleats
WHERE DAYOFMONTH(data_contractacio) % 2 = 0
ORDER BY data_contractacio; 
````
22. Mostra el cognom, la data de contractació i el dia de la setmana en el que va començar l’empleat a treballar. Etiqueta la columna com a dia. Ordena els resultats per dia de la setmana.
```mysql
SELECT cognoms, data_contractacio, DAYOFWEEK(data_contractacio) AS dia
	FROM empleats
ORDER BY dia; 
````
23. Mostra el codi d’empleat, cognoms i la data de contractació en format AAAAMM. El dia no ens interessa ara en el nostre estudi estadístic. Ordena per aquest nou format de data.
```mysql
SELECT empleat_id, cognoms, 
       CONCAT(EXTRACT(YEAR FROM data_contractacio), '-', LPAD(EXTRACT(MONTH FROM data_contractacio), 2, '0')) AS data_formatada
FROM empleats
ORDER BY data_formatada;
````
24. Per cada empleat, mostra el cognom, la data de contractació i el número de mesos entre el dia d’avui i la data de contractació. Etiqueta la columna com a "mesos_treballats" i arrodoneix sense decimals. Ordena el resultat segons els mesos treballats de més a menys.
```mysql
SELECT cognoms, data_contractacio, 
	ROUND(DATEDIFF(CURRENT_DATE, data_contractacio) AS mesos_treballats), 0)
	FROM empleats
ORDER BY mesos_treballats ASC; 
````
25. Mostra el nom, cognom i anys d’antiguitat dels empleats que tenen una antiguitat superior o igual a 20 anys a l’empresa.
```mysql
SELECT 
    nom, 
    cognoms, 
    EXTRACT(YEAR FROM data_contractacio) AS antiguitat
FROM empleats
WHERE EXTRACT(YEAR FROM data_contractacio) >= 20;
````
26. Crea una consulta per mostrar el cognom i salari de tots els empleats que guanyen més de 10.000 a l’any. Dona format al camp salari per a que tingui 15 caràcters de longitud, omplint per l’esquerra amb $. Etiqueta la columna com a salari.
```mysql
SELECT cognoms, 
       LPAD(salari, 15, '$') AS salari
FROM empleats
WHERE salari > 10000;
````
27. Mostra el cognom, salari i percentatge de comissió dels empleats. Afegeix una nova columna en que si un empleat no té assignada comissió, posi "SenseComissió". Etiqueta la columna amb nom "no_comissio"
```mysql
SELECT 
    cognoms,
    salari,
    pct_comissio,
    CASE 
        WHEN pct_comissio IS NULL THEN 'SenseComissió' END AS no_comissio
FROM empleats;
````
28. Mostra el cognom, salari i utilitzant la funció CASE, mostra el següent en
funció del valor del salari
  - Si el salari està entre 0 i 3000 -> "Mig"
  - Si el salari està entre 12000 i 24000 -> "Alt"
  - Qualsevol altre valor posa "Altres"
  - Anomena la columna com a "poder_adquisitiu" i ordena per salari de menor a major
```mysql
SELECT cognoms, salari,
        CASE
                WHEN salari >= 0 AND salari <= 3000 THEN "Mig"
                WHEN salari >= 12000 AND salari <= 24000 THEN "Alt"
                ELSE "Altres"
        END AS poder_adquisitiu
        FROM empleats
ORDER BY salari ASC;
```
28. Llista el codi dels departaments dels empleats que apareixen a la taula empleats.
```mysql
SELECT departament_id
FROM empleats;
````
29. Partint de la consulta anterior elimina els codis de departament repetits.
```mysql
SELECT DISTINCT departament_id
FROM empleats;
````
30. Calcula el nombre d'empleats que **no tenen** comissió assignada.
```mysql

````
