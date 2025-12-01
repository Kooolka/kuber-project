# ClickHouse проект для Kubernetes

Простой и надежный проект для развертывания ClickHouse в Kubernetes. Поддерживает single-инсталляцию с полной параметризацией и управлением пользователями.

## Особенности

- Single-инсталляция - оптимальное решение для standalone развертывания
- Параметризация версий - легкая смена версии ClickHouse через values.yaml
- Управление пользователями - гибкая настройка пользователей, профилей и прав доступа
- Persistent storage - автоматическое создание PVC через StatefulSet
- Health monitoring - встроенные liveness и readiness пробы
- Мультипротокольность - поддержка HTTP, Native, MySQL и PostgreSQL интерфейсов
- Конфигурация через ConfigMap - горячее обновление конфигурации без пересоздания Pod

## Быстрый старт

### Предварительные требования

- **Kubernetes кластер** версии 1.19+
- **kubectl** настроенный для доступа к кластеру
- **Helm 3.x** установленный локально

### Установка

1. Клонируйте репозиторий

2. Установка Helm-чарта
# Установите чарт в namespace clickhouse
helm install clickhouse ./clickhouse-chart -n clickhouse --create-namespace

3. Проверка установки
# Проверьте статус StatefulSet
kubectl get statefulsets -n clickhouse

# Проверьте поды
kubectl get pods -n clickhouse

# Проверьте сервис
kubectl get svc -n clickhouse

3. Конфигурация
Основные параметры конфигурации находятся в файле values.yaml

4. Подключение к ClickHouse
Внутри кластера Kubernetes:
# Подключение через port-forward
kubectl port-forward svc/clickhouse 8123:8123 -n clickhouse
# Теперь можно подключиться на localhost:8123

5. Примеры запросов
-- Проверка версии
SELECT version();

-- Создание таблицы
CREATE TABLE test_table (id Int32, name String) ENGINE = MergeTree ORDER BY id;

-- Вставка данных
INSERT INTO test_table VALUES (1, 'test');

-- Выборка данных
SELECT * FROM test_table;

6. Удаление
# Полное удаление
kubectl delete pvc -l app=clickhouse -n clickhouse

7. Структура проекта
clickhouse-chart/
├── Chart.yaml         
├── values.yaml        
├── templates/         
│   ├── deployment.yaml 
│   ├── service.yaml    
│   ├── networkpolicy.yaml
│   ├── configmap.yaml  
│   └── _helpers.tpl    
└── README.md          

8. Заменчания 

Для продакшена лучше использовать Secrets и хэши паролей.
NetworkPolicy базово работает, но надо расширить для фильтрации по портам.
