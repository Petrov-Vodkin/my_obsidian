2021-11-11 17:57
#tag
# Kubectl
### **Императивный подход**
С помощью kubectl мы можем взаимодействовать с Кубернетес как императивно (указывать сделать, что мы хотим), так и декларативно (описать, что мы хотим и просить сделать).
На уроке про Докер мы запушили наш Веб-сервис на Go в Докерхаб, настало время использовать этот образ.
```
kubectl create deployment first-deployment  --image=ksxack/lesson1:v0.2 
```
В данном случае, мы _императивно_ создали Деплоймент first-deployment используя для Пода образ `ksxack/lesson1:v0.2` 
**Что такое Деплоймент** - мы изучим через урок, пока нам необходимо понять на примере два абсолютно разных способа взаимодействия с Кубернетес.
### **Декларативный подход**
Чтобы создать деплоймент декларативно создайте файл `deployment.yaml`  и заполните следующим содержимым: 
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: declarative-deployment
  labels:
    app: go-web
spec:
  replicas: 1
  selector:
    matchLabels:
      app: goapp
  template:
    metadata:
      labels:
        app: goapp
    spec:
      containers:
      - name: goapp
        image: ksxack/lesson1:v0.2
        ports:
        - containerPort: 8080
```
А затем впишите в командной строке команду:
```bas
kubectl apply -f deployment.yaml
kubectl get deployments # Проверить, что деплойменты создались можно командой
kubectl get pod			# проверить наличие Подов
				-A		# проверить наличие ВСЕХ Подов
```
____________________________________
### **Команды kubectl**
**Смена кластеров**
Работа в нашем курсе идет с одним кластером Кубернетес, однако в реальных условиях кластеров будет несколько, для переключения между ними используйте команды:
```bash
kubectl config get-contexts                          # показать список контекстов
kubectl config current-context                       # показать текущий контекст (current-context)
kubectl config use-context my-cluster-name           # установить my-cluster-name как контекст по умолчанию
```
**Создание объектов**
```bash
kubectl apply -f ./my-manifest.yaml            # создать объект из файла
kubectl create deployment nginx --image=nginx  # запустить один экземпляр nginx
```
**Просмотр информации**
```bash
kubectl get pods -o wide                      # Вывести все поды и показать, на каких они нодах
kubectl describe pods my-pod                  # Просмотреть информацию о поде такую как время                                                                 # старта, количество и причины рестартов, QoS-класс и прочее
kubectl logs -f my-pod                        # Просмотр логов в режиме реального времени
kubectl top pods                              # Вывести информацию об утилизации ресурсов подами
```
**Изменение объектов**
```bash
kubectl edit pod my-pod                       # Изменение .yaml манифеста пода
```
**Удаление объектов** 
```bash
kubectl delete pod my-pod                       # Удаление пода
```
**Погружение в командную оболочку Пода (проваливаемся вовнутрь)**
```
kubectl exec -it -n namespace-name podname sh   # На конце выбираем оболочку, если нет sh, ставим bash
```
**Копируем файл в под**
```bash
kubectl cp {{namespace}}/{{podname}}:path/to/directory /local/path  # Копирование файла из Пода
kubectl cp /local/path namespace/podname:path/to/directory          # Копирования файла в Под
```
**Проброс портов**
```bash
kubectl port-forward pods/mongo-75f59d57f4-4nd6q 28015:27017  # Проброс порта Пода
kubectl port-forward mongo-75f59d57f4-4nd6q 28015:27017       # Проброс порта Сервиса
```


_____________
#### Links