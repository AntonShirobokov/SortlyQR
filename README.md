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
  - TimescaleDB (расширение PostgreSQL)
  - RabbitMQ (QR Redirect microservice)  

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
