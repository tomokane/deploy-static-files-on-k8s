Deploy Static files on k8s
======================================================

# 1. Build Nginx docker image with static files

put your static files (eg: html, txt file etc) in `html` directory. and build docker image.

```bash
$ docker build -t <YOUR REGISTRY URL AND IMAGE NAME> .
Sending build context to Docker daemon  20.48kB
Step 1/2 : FROM nginx
 ---> 3f8a4339aadd
Step 2/2 : COPY html /usr/share/nginx/html
 ---> Using cache
 ---> eb068a4597ce
Successfully built eb068a4597ce
Successfully tagged <YOUR REGISTRY URL AND IMAGE NAME>

$ docker push <YOUR REGISTRY URL AND IMAGE NAME>
The push refers to repository [<YOUR REGISTRY URL AND IMAGE NAME>]
c07992bce642: Layer already exists 
a103d141fc98: Layer already exists 
73e2bd445514: Layer already exists 
2ec5c0a4cb57: Layer already exists 
latest: digest: sha256:b1620fe617d535a07f4eadbcde181debb0389519eae21c93067342ccae0ae050 size: 1155
```

# 2. Edit yml and deploy to k8s

## Set your docker registry

set your registry name that you pushed docker image previours topic in `deployment.yml`.

```yml
    spec:
      containers:
      - name: yourapp
        image: <YOUR IMAGE NAME> # replace image name on your registry.
        imagePullPolicy: Always
        ports:
        - containerPort: 80
          protocol: TCP
```

## Set your domain name

set your domain name in `ingress.yml`

```yml
spec:
  rules:
  - host: yourdomain.com # edit here
    http:
      paths:
      - path: /
        backend:
          serviceName: yourapp
          servicePort: http
```

If you need set each deployment, service, ingress name, Please edit each file names.

## Apply yml to k8s

```bash
$ kubectl apply -f deployment.yml
$ kubectl apply -f service.yml
$ kubectl apply -f ingress.yml
```


