# **Інструменти k8s для розгортання в локальному середовищі**

Я розумію, що ви вже розбиралися з цим питанням, тому багато тексту нам не знадобиться.

Ось приклад порівняльної таблиці для наших трьох систем.


| **Feature**            | **minikube**              | **kind**              | **k3s/k3d**        |
| ------------------------ | --------------------------- | ----------------------- | -------------------- |
| Multiple Cluster       | YES                       | YES                   | Using k3d          |
| Multiple Nodes         | NO                        | YES                   | YES                |
| Root Privilege Recured | NO                        | NO                    | YES                |
| Free Memory            | 2 GB                      | 8 GB                  | 512 MB             |
| Runtime Support        | Docker, CRI-O, containerd | Docker, CRI-O         | Docker, containerd |
| Supported Architecture | AMD64, ARM64              | AMD64, ARM64          | AMD64, ARM64       |
| Operating System       | Linux, Windows, macOS     | Linux, Windows, macOS | Linux              |
| Add ons                | TES                       | NO                    | Partially YES      |

Тепер треба уточнити, що саме нам потрібно від цих інструментів.

Застосунок, який ви розробляєте, особливо якщо він контейнерізований, буде запускатись в кластері де є ще декілька речей, з якими йому треба взаїмодіяти. Також для розробки треба забезпечити можливості моніторінгу та автоматизації включно з CI/CD пайплайнами.

Можна вибирати.

1. minikube -  це локальна система Kubernetes, яка дозволяє розгортати кластер Kubernetes на одному комп'ютері, і вже тому він нам не дуже підходить.
2. Kind може створюати декілька Kubernetes кластерів, використовуючи Docker контейнери для вузлів кластера. Kind часто використовують для локального розробника або тестера.
3. k3d - це легка оболонка для запуску k3s. В k3d можна створювати кластери, також він має кращі можливості по роботі з CI пайплайнами. В цілому це зручний інструмент для являє собою автономне, готове до виробництва рішення, що підходить як для DEV, так і для PROD навантажень.

Мені здається, найкращім вибором для вас буде k3d.
Ось невеличке демо.

k3d cluster create hello
kubectl create deployment hello-k3d --image=k8s.gcr.io/echoserver:1.4
kubectl expose deployment hello-k3d --type=NodePort --port=8080
kubectl get deployments
kubectl get pods
kubectl get services



![](assets/20231204_091230_demo.gif)
