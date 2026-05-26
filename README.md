## Лабораторная работа по работе с docker
## Homework (by Daniel Zakirov IU8-23)

В репозитории приведен код web-приложения, которое сохраняет в БД введенную информацию о задаче - ее имя.

## Часть I. Docker

1. Добавьте в код Dockerfile, который позволит запустить web-приложение с исходным кодом в каталоге app/ через docker.

Мой [Dockerfile](Dockerfile)

2. Выполните запуск контейнера с этим приложением.

```console
$ sudo docker build -t dp-app .
```

<details><summary>Output</summary>

```
[+] Building 25.4s (10/10) FINISHED                                                                                                                docker:default
 => [internal] load build definition from Dockerfile                                                                                                         0.1s
 => => transferring dockerfile: 208B                                                                                                                         0.0s
 => [internal] load metadata for docker.io/library/python:3.14-slim                                                                                          7.2s
 => [internal] load .dockerignore                                                                                                                            0.1s
 => => transferring context: 2B                                                                                                                              0.0s
 => [1/5] FROM docker.io/library/python:3.14-slim@sha256:c845af9399020c7e562969a13689e929074a10fd057acd1b1fad06a2fb068e97                                    5.7s
 => => resolve docker.io/library/python:3.14-slim@sha256:c845af9399020c7e562969a13689e929074a10fd057acd1b1fad06a2fb068e97                                    0.1s
 => => sha256:99500e0cbdb98ad73639044dd5113029e40bdab38b4ea5fe594532bb79cbe57f 12.33MB / 12.33MB                                                             1.2s
 => => sha256:b83fa8e4dee39a3198704bc54184a177c88e40a821cb24d55437776d8cfdf85a 249B / 249B                                                                   0.4s
 => => sha256:dcf23796d1e8ae9b05eee16765fcf83722d2c0fdda9a27d09d25610a7ed9a0a0 1.05MB / 1.29MB                                                              17.5s
 => => sha256:5b4d6ff92fc4e14e911b7753c954fac965d48c40fe1075758d284148ccace970 29.78MB / 29.78MB                                                             2.3s
 => => extracting sha256:5b4d6ff92fc4e14e911b7753c954fac965d48c40fe1075758d284148ccace970                                                                    1.7s
 => => extracting sha256:dcf23796d1e8ae9b05eee16765fcf83722d2c0fdda9a27d09d25610a7ed9a0a0                                                                    0.2s
 => => extracting sha256:99500e0cbdb98ad73639044dd5113029e40bdab38b4ea5fe594532bb79cbe57f                                                                    0.9s
 => => extracting sha256:b83fa8e4dee39a3198704bc54184a177c88e40a821cb24d55437776d8cfdf85a                                                                    0.1s
 => [internal] load build context                                                                                                                            0.1s
 => => transferring context: 1.76kB                                                                                                                          0.0s
 => [2/5] WORKDIR /app                                                                                                                                       0.7s
 => [3/5] COPY app/requirements.txt .                                                                                                                        0.1s
 => [4/5] RUN pip install --no-cache-dir -r requirements.txt                                                                                                 5.7s
 => [5/5] COPY app/ .                                                                                                                                        0.2s 
 => exporting to image                                                                                                                                       5.2s 
 => => exporting layers                                                                                                                                      3.1s 
 => => exporting manifest sha256:a8408676a97f54e55d8f609e07628f0cbd64cec0eda5af06a1053f481bf75439                                                            0.0s 
 => => exporting config sha256:8694dc526288f4fe268ba81d8b2f898d950bb833ce30014be687b9612f9a7550                                                              0.1s 
 => => exporting attestation manifest sha256:bdf067e68d5f4508b9d836ef0cc09fff3a9a99db69e00f48fc8e233dbb1cedf2                                                0.1s 
 => => exporting manifest list sha256:702705b0db6b22b509914e2de06e3ad98b9f883397bda95fbfe8ea4e3fc4d485                                                       0.0s
 => => naming to docker.io/library/dp-app:latest                                                                                                             0.0s
 => => unpacking to docker.io/library/dp-app:latest    
```

</details>

```console
$ sudo docker run -p 5000:5000 --name dp-app-container dp-app
 * Serving Flask app 'app'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:5000
 * Running on http://172.17.0.2:5000
Press CTRL+C to quit
172.17.0.1 - - [26/May/2026 19:20:23] "GET / HTTP/1.1" 200 -  # попытка посетить сайт - ОК
```

3. Скопируйте из консоли в каталог /home/ контейнера файл README.md.

```console
$ sudo docker cp README.md dp-app-container:/home/
Successfully copied 7.23kB (transferred 9.22kB) to dp-app-container:/home/
```

4. Подключитесь к терминалу контейнера с приложением в интерактивном режиме. Проверьте, что скопированный файл находится в нужном каталоге.

```console
$ sudo docker exec -it dp-app-container /bin/bash
root@9f21c9ce3f20:/app# ls -l /home/README.md 
-rw-rw-r-- 1 1000 1000 7229 May 26 19:22 /home/README.md
```

5. Выйдите из интерактивного режима.

```console
exit
```
Или <kbd>Ctrl</kbd>+<kbd>D</kbd>

6. Остановите контейнер с приложением.

```console
sudo docker stop dp-app-container
```

## Часть II. Docker compose
1. Создайте файл docker-compose.yml таким образом, чтобы совместно с описанным в части 1 контейнером работала бы база данных mysql. Файл инициализации БД в каталоге db/init.sql. Также пропишите порт подключения к приложению. Например 5000.
2. Запустите связку web-приложение - БД.
3. Проверьте подключение к приложению через браузер. Сделайте снимок экрана.
4. Проверьте работу приложения через браузер.

