# Deploy-Angular-application-on-Kubernetes

**Prerequisites:**

* Nodejs yuklenmis olmali.
* Docker yuklenmis olmali.
* Kubernetes yuklenmis olmali.

**Step 1: Create an angular application**

Asagidaki komut ile Angular CLI yukleyebiliriz:

```
npm install -g @angular/cli
```
Daha sonra ise, Angular uygulamasini olusturacagiz:

 ```
ng new hello-world
```

Sonra, hello-world directory'sine girip, yerel sunucuda calistiracagiz:

```
ng serve
```

Default olarak uygulamamaiz port numarasi 4200'da calisacaktir.

**Step 2: Build the application in production environment**

Dist klasorunu root directory'de olusturmak icin asagidaki komutu kullanacagiz:

```
ng build --production
```

**Step 3: Create a Dockerfile**

Dosyalari ***dist*** klasorunden alip, nginx path'e kopyalamaliyiz:

```
FROM nginx:alpine
COPY ./dist/hello-world ./usr/share/nginx/html
```

**Step 4: Build the Dockerfile**

Dockerfile'i olusturdugumuz directory'de asagidaki komutu yaziyoruz:

```
docker build -t angular/hello-world:v1 .
```

Olusturma sureci tamamlandiktan sonra, docker imajini kontrol edebiliriz:

```
docker ps -a
```

**Step 5: Create a kubernetes Deployment Pod**

```
kubectl apply -f deployment.yaml
```

YAML dosyasini kisaca ozetleyecek olursak:

- Turunu Deployment olarak belirledik
- apiVersion hangi kubernetes versiyonu oldugunu belirtir
- metadata deployment ismidir
- spec kisminin altinda, docker imajinin ismini belirtmeliyiz.
- imagePullPolicy never olarak belirtildiginden, kubernetes imaji docker registry'den cekecek
- en alt kisimda ise, deployment'in calisacagi port belirtilmistir

**Step 6: Create a Service**

```
kubectl apply -f service.yaml
```



Service pod'u uygulamalar arasi iletisimi saglamamzi icin bize yardimci olur.

Deployment'i dogrulatmak icin:

```
kubectl get all
```


**Step 7: Port Forwarding**

```
kubectl port-forward service/helo-world-service 80:90
```
[(http://localhost:80)](http://localhost:80)
