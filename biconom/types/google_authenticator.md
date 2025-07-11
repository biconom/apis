# Модель: GoogleAuthenticator

## 1. Описание

**`GoogleAuthenticator`** — это модель, представляющая собой **состояние** настройки двухфакторной аутентификации (2FA) для конкретного пользователя (`User`).

**Важно**: из соображений безопасности эта модель **не содержит** и никогда не должна содержать секретный ключ. Секретный ключ генерируется и показывается пользователю только один раз в момент установки 2FA.

## 2. Назначение и решаемые задачи

Основная цель — хранить информацию о том, включена ли 2FA для пользователя и на каком этапе настройки она находится.

### Ключевые задачи:
- **Отслеживание статуса**: Показывает, активна ли 2FA, неактивна или ожидает верификации.
- **Связь с пользователем**: Четко привязывает настройку 2FA к `user_id`.
- **Управление через политики**: Позволяет применять к настройке 2FA централизованные правила (`GoogleAuthenticatorPolicy`).

## 3. Ключевые поля и концепции

- `user_id`: ID пользователя (`User`), к которому относится эта настройка.
- `Status`: Вложенный `enum`, определяющий жизненный цикл настройки:
  - **`INACTIVE`**: 2FA не настроена.
  - **`PENDING_VERIFICATION`**: Пользователь начал процесс, отсканировал QR-код, но еще не подтвердил его вводом первого кода. На этом этапе бэкенд временно хранит секретный ключ, ожидая подтверждения.
  - **`ACTIVE`**: 2FA полностью настроена и активна.
- `policy_id`: Обязательное поле, связывающее настройку с ее политикой.

## 4. Сценарии использования (кейсы)

- **Начало установки 2FA**: Пользователь инициирует настройку. На бэкенде создается запись `GoogleAuthenticator` со статусом `PENDING_VERIFICATION`. Пользователю отправляется QR-код.
- **Подтверждение 2FA**: Пользователь вводит код из приложения. Бэкенд проверяет его, и если он верен, меняет статус на `ACTIVE`.
- **Отключение 2FA**: Пользователь (после прохождения необходимых проверок) отключает 2FA. Статус меняется на `INACTIVE`.
- **Проверка при входе**: Система видит, что у пользователя есть `GoogleAuthenticator` со статусом `ACTIVE` и запрашивает у него код.

## 5. Связи с другими моделями

- **`User`**: `GoogleAuthenticator` **принадлежит** одному `User` (связь по `user_id`).
- **`GoogleAuthenticatorPolicy`**: `GoogleAuthenticator` **управляется** одной политикой.
