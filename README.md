Add your ssh pub keys to the config-* directory

i.e:

```
for d in config-frontend1 frontend-2 config-haproxy; do
 cp $HOME/.ssh/id_rsa.pub $d
done
```

Then build the services with:

```docker-compose build```

And run it with:

```docker-compose up```


Now in your host you should be able to ssh into the three boxes without
password (assuming you have ansible installed in your host):

```
for h in frontend1 frontend2 haproxy; do ansible $h -i inventory.yml -m ping; done

```

To check if http servers are reachable on the frontends:

```
# frontend1
$ curl http://localhost:8080

# frontend2
$ curl http://localhost:8081
```

To check if the haproxy is working as expected:

```
$ curl http://localhost:8082
<html>
  <head></head>
  <body> 
    <h1>Hello, frontend1</h1>
  </body>
</html>

$ curl http://localhost:8082
<html>
  <head></head>
  <body> 
    <h1>Hello, frontend2</h1>
  </body>
</html>
```
