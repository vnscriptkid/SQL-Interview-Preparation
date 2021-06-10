## :star: Helper Commands

```sql
-- Where database files are stored
SHOW data_directory;

-- List all databases along with identifier number (in base/ folder)
SELECT oid, datname
FROM pg_database

-- List all tables's files
SELECT * FROM pg_class;
```

## :star: Terminologies

![image](https://user-images.githubusercontent.com/28957748/121564980-36275180-ca46-11eb-8b9a-5641fca02b6d.png)

![image](https://user-images.githubusercontent.com/28957748/121569143-ae901180-ca4a-11eb-84e4-b3bf675b3b01.png)

## :star: Page Layout

https://www.postgresql.org/docs/current/storage-page-layout.html

![image](https://user-images.githubusercontent.com/28957748/121571827-af767280-ca4d-11eb-9735-19eb76594977.png)


