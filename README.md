# go-postgresql
Setting up postgresql and go

// get docker lib for golang
1. Terminal:  
```
go get github.com/jackc/pgx/v4
```
2. Code:  
```
import _ "github.com/jackc/pgx/v4/stdlib"
```

// hosting postgresql via docker  
3. Terminal: The version at the end can be changed to latest  
```
docker run \
-d \
-e POSTGRES_USER=user \
-e POSTGRES_PASSWORD=password \
-e POSTGRES_DB=dbname \
-p port:5432 \
postgres:12.5-alpine
```

// setting up db connection  
4. Code:  
```
	dsn := url.URL{
		Scheme: "postgres",
		Host:   "localhost:port",
		User:   url.UserPassword("user", "password"),
		Path:   "dbname",
	}

	q := dsn.Query()
	q.Add("sslmode", "disable")

	dsn.RawQuery = q.Encode()

	db, err := sql.Open("pgx", dsn.String())
	if err != nil {
		log.Println(err)
		return
	}
	defer func() {
		_ = db.Close()
		log.Println("connection is closed")
	}()

	if err := db.PingContext(context.Background()); err != nil {
		log.Println(err)
		return
	}
  
	//querys
```

5. Access db manually:  
docker exec -it $container_id psql -U user -d dbname

## All set
