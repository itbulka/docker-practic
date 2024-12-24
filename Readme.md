### Описание задания

Перед развертыванием публичного
контейнера внутрь корпоративного контура
компания N приняла решение внедрить
следующую стратегию поведения:

- Развернуть контейнер с публичным образом
- Запустить в нем скрипт test.sh
- Добавить протестированный образ в свой аккаунт
Docker Hub с тегом tested

---

### Ход работы

- Написал самый простой API, который по запросу к серверу выдает `{ message: "Hello world" }`
- Описал Dockerfile: Устанавливаем рабочую директорию app, перекидываем туда файлы package.json package-lock.json в которых прописаны все нужные зависимости, устанавливаем эти зависимости командой `npm install`, после чего перекидываем остальные файлы проекта и запускаем его.
- Собираем образ командой `docker build -t api-hello-world`
- Запускаем контейнер с нашим образом: `docker run -p 3000:3000 -d api-hello-world`
- Проверяем что контейнер запущен: `docker ps`

  |CONTAINER ID |  IMAGE                  |  COMMAND                |  CREATED        |  STATUS       |  PORTS                  |  NAMES             |
  |-------------|-------------------------|-------------------------|-----------------|---------------|-------------------------|--------------------|
  |e91b8e6411bf |  api-hello-world:latest |  "docker-entrypoint.s…" |  20 minutes ago |  Up 2 seconds |  0.0.0.0:3000->3000/tcp |  confident_jepsen` |
- Подготовим простой скрипт test.sh , который просто выведет в консоль `Успешная проверка`.
- Копируем данный файл в контейнер следующей командой: `docker cp ./test.sh e91b8e6411bf:/app`
- Запуска данный скрипт в контейнере: `docker exec -it e91b8e6411bf sh ./test.sh`
> vladimir@DESKTOP-CEO5I5J:~/Docker-lessons/BasicsVirtualization/lecture3-task$ docker exec -it e91b8e6411bf sh ./test.shУспешная проверка
- После чего я сбилдил новый образ с тегом :tested и запушил его в Docker Hub:  
`docker build -t a0xa0x/api-hello-world:tested .`  
`docker push a0xa0x/api-hello-world:tested`

