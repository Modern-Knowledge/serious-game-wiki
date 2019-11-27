# Datenbank

### Import eines SQL-Dumps in einen MySQL-Docker Container 

```bash
cat /path/dump.sql | docker exec -i CONTAINER /usr/bin/mysql -u USERNAME --password=PASSWORD --default-character-set=utf8 DATABASE
```
