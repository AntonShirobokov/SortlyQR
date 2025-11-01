# SortlyQR #

**SortlyQR** — это сервис для создания персональных QR-кодов, которые связывают ваши вещи и события с цифровым миром. 
### Возможности:
- Генерация обычных QR кодов
- Генерация QR кодов с отслеживанием статистики: сколько раз, где, когда сканировали ваши QR-коды 
- Наводить порядок: помечайте коробки QR-кодами и всегда знайте, где что лежит    

Подходит как для **личного использования**, так и для **бизнеса и брендов**.  

## Архитектура проекта

<img width="1201" height="737" alt="image" src="https://github.com/user-attachments/assets/7b3c04e7-26d2-44fb-b99a-1fc4b844b0b7" />



**Общая схема:**
- Frontend — пользовательский интерфейс
- API Gateway — единая точка входа для клиентов  
- Authorization microservice — авторизация и аутентификация  
- QR Management microservice — управление QR-кодами  
- QR Redirect microservice — обработка сканирований  
- QR Analytics microservice — сбор и анализ статистики  

## Микросервисы

### 1. Authorization microservice
- **Назначение:** регистрация, вход, выдача JWS (асимметричная подпись)  
- **Инструменты:**  
  - PostgreSQL (пользователи, хэши паролей, роли)  
  - JWS (асимметричная подпись)  
  - `jjwt` (библиотека шифрования)  

---

### 2. QR Management microservice
- **Назначение:** создание, редактирование и удаление QR-кодов;  
- **Инструменты:**  
  - PostgreSQL (хранилище QR-кодов)
  - RabbitMQ (отправка событий о создании или удалении qr кода в QR Redirect microservice)
---

### 3. QR Redirect microservice
- **Назначение:** обработка сканирований, редиректы, сбор статистики  
- **Инструменты:**  
  - Redis (для кэширования редиректов)  
  - RabbitMQ (отправка событий о сканировании в Analytics)  
  - PostgreSQL

---

### 4. QR Analytics microservice
- **Назначение:** хранение и анализ статистики сканирований  
- **Инструменты:**  
  - RabbitMQ (QR Redirect microservice)  
- **Интеграции:**
  - Интеграция с api.ipwho.org для получения подробной информации по ip пользователя (Страна, регион, город и т.д.)
---

### 5. API Gateway
- **Назначение:** единая точка входа для фронтенда, маршрутизация и фильтры  
- **Инструменты:**  
  - Spring Cloud Gateway
  - Интеграция: Authorization microservice, QR Management microservice, QR Analytics microservice

---

### 6. Frontend
- **Назначение:** пользовательский интерфейс  
- **Инструменты:**  
  - React 
  - Общение только через API Gateway   

## Технологии проекта
- Java / Spring Boot / Spring Cloud  
- PostgreSQL
- Redis, RabbitMQ    
- Docker + Docker Compose  
- React (Frontend)

## Как запустить приложение

### 1. Клонирование репозитория
Склонируйте проект на ваш сервер или локальную машину:

**git clone https://github.com/AntonShirobokov/SortlyQR**

### 2. Настройка микросервиса API_Gateway

Перейдите в директорию API_Gateway/src/main/resources и откройте файл application-prod.yml.

В конфигурации замените значение поля frontend.host на IP-адрес сервера, где будет запущено приложение:

frontend:
  host: http://<ip-адрес>:5173

### 3. Настройка микросервиса Frontend_microservice

В директории Frontend_microservice откройте файл .env.production
и укажите IP-адрес вашего сервера в следующих параметрах:

VITE_API_GATEWAY_BASEURL=http://<ip-адрес>:8080

VITE_QR_MANAGEMENT_BASEURL=http://<ip-адрес>:5173

VITE_QR_REDIRECT_BASEURL=http://<ip-адрес>:8083

### 4. Запуск приложения

После того как все настройки указаны, выполните команду для сборки и запуска всех микросервисов:

**docker compose up**

После успешного запуска приложение будет доступно по адресу: http://<ip-адрес>:5173

## Результаты

<p align="center">
  <img width="1274" height="1011" alt="Профиль неавторизованный" src="https://github.com/user-attachments/assets/56de279c-7307-4703-8bb5-49b881c6e97f" />
  <br>
  <em>Профиль (неавторизованный пользователь)</em>
</p>

<p align="center">
  <img width="515" height="824" alt="Регистрация" src="https://github.com/user-attachments/assets/8e2230f8-ebf6-4f07-a8cf-6d30e170e4c4" />
  <br>
  <em>Регистрация</em>
</p>

<p align="center">
  <img width="605" height="595" alt="Авторизация" src="https://github.com/user-attachments/assets/43d20264-c061-4a95-aa78-270f8e442c05" />
  <br>
  <em>Авторизация</em>
</p>

<p align="center">
  <img width="1269" height="1023" alt="Создание простого qr кода" src="https://github.com/user-attachments/assets/8a0fa87d-faaa-401e-a37e-db20af645825" />
  <br>
  <em>Создание простого QR-кода</em>
</p>

<p align="center">
  <img width="1290" height="997" alt="Создание qr кода со списком содержимого" src="https://github.com/user-attachments/assets/c8c37746-1839-4733-aefc-05350d629e06" />
  <br>
  <em>Создание QR-кода со списком содержимого</em>
</p>

<p align="center">
  <img width="1279" height="1028" alt="Создание qr кода с отслеживанием статистики" src="https://github.com/user-attachments/assets/acc3c05c-681a-4c4b-a84a-b5d1747ae9fe" />
  <br>
  <em>Создание QR-кода с отслеживанием статистики</em>
</p>

<p align="center">
  <img width="1298" height="823" alt="Профиль пользователя" src="https://github.com/user-attachments/assets/fbac6ff0-42ae-4804-a839-f32fa776eaa6" />
  <br>
  <em>Профиль пользователя</em>
</p>

<p align="center">
  <img width="1286" height="818" alt="Подробная информация" src="https://github.com/user-attachments/assets/1bdd2a00-d305-4bb7-be41-8d45b8a80fcb" />
  <br>
  <em>Подробная информация (qr код со статистикой)</em>
</p>

<p align="center">
  <img width="1269" height="804" alt="Подробная информация" src="https://github.com/user-attachments/assets/b59ce90d-9f1a-4f1f-ae0f-74096de8be14" />
  <br>
  <em>Подробная информация (qr код список)</em>
</p>

<p align="center">
  <img width="570" height="350" alt="Страница при сканировании qr кода типа список" src="https://github.com/user-attachments/assets/c5646409-8745-4ac2-a307-bc9401d01045" />
  <br>
  <em>Страница при сканировании QR-кода (тип: список)</em>
</p>
