# Модель: TreeDistributorPolicy

## 1. Описание

**`TreeDistributorPolicy`** — это модель, которая определяет набор правил для группы связей `TreeDistributor`. **Конкретная логика политики (например, приостановка участия) реализуется на стороне бэкенде**.

## 2. Назначение и решаемые задачи

Основная цель — централизованное управление состоянием участия дистрибьюторов в деревьях.

### Ключевые задачи:
- **Централизованное управление**: Позволяет изменять состояние участия тысяч дистрибьюторов через обновление одной политики.
- **Прозрачность**: `name` и `description` дают понимание, почему к группе участников применено то или иное правило.

## 3. Ключевые поля и концепции

- `name`: Уникальное имя политики.
- `description`: Текстовое описание, поясняющее суть и логику работы политики.

## 4. Сценарии использования (кейсы)

- **Блокировка в маркетинге**: Администратор создает `TreeDistributorPolicy` с `description`="Блокировка в дереве 'Matrix 2023' за нарушение правил". Бэкенд, видя эту политику, блокирует все действия дистрибьютора (покупку слотов, получение бонусов) в рамках данного дерева.

## 5. Связи с другими моделями

- **`TreeDistributor`**: `TreeDistributor` **может** управляться одной политикой (связь по `policy_id`).
