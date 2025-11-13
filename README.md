  # Anon Decentral Messenger
Madmax — децентрализованный одноранговый мессенджер на Python 3 с графическим интерфейсом PyQt6.  
Приложение использует современную криптографию для шифрования сообщений и не требует центральных серверов.

---

## Возможности

- P2P-взаимодействие без централизованных узлов  
- Автоматический обмен ключами X25519 (ECDH)  
- Симметричное шифрование ChaCha20 (256 бит)  
- Контроль целостности через SHA-256  
- Сессионные ключи, пересоздающиеся при каждом подключении  
- Локальная база SQLite с возможностью шифрования AES-256-CBC  
- Кроссплатформенность (Linux / Windows)

---

## СБОРКА
GNU/LINUX and UNIX-like system

```bash
source ...
pip install -r requirements.txt
cd madmaxOBSF
pyinstaller --onefile --noconsole --clean main.py \
  --name madmax \
  --collect-all PyQt6 \
  --collect-all cryptography \
  --hidden-import=sqlite3 \
  --hidden-import=PyQt6.QtWidgets \
  --hidden-import=PyQt6.QtCore \
  --hidden-import=PyQt6.QtGui \
  --hidden-import=cryptography.hazmat.bindings._openssl \
  --hidden-import=cryptography.hazmat.bindings._rust \
  --hidden-import=cryptography.hazmat.primitives.asymmetric.x25519 \
  --add-data "ui:ui" \
  --add-data "db_module:db_module" \
  --add-data "socket_module:socket_module" \
  --add-data "../pyarmor_runtime_000000:pyarmor_runtime_000000"
  cd bild
```

# ИЛИ 

https://mega.nz/fm/qR1hkDDA

---

## Запуск

Исполняемый файл или shell-скрипт **madmax** не требует параметров:

```bash
chmod +x madmax
./madmax
```
Для ручного выбора графического бэкенда Qt:

QT_QPA_PLATFORM=xcb ./madmax       # X11
QT_QPA_PLATFORM=wayland ./madmax   # Wayland


---

## Криптографическая основа

#Обмен ключами (ECDH, X25519)

1. При подключении хост и клиент обмениваются открытыми ключами X25519.


2. Каждый вычисляет общий секрет с помощью алгоритма Диффи–Хеллмана.


3. Из секрета формируется симметричный ключ длиной 256 бит:

shared_key = SHA256(X25519.exchange(private_key, peer_public_key))



# Симметричное шифрование
 
Алгоритм: ChaCha20

Ключ: 256 бит

Nonce: 128 бит, уникален для каждого пакета

Контроль целостности осуществляется через SHA-256:

digest = SHA256(nonce || ciphertext || SHA256(key))


При несовпадении digest пакет отвергается.

Локальное хранение

История чатов сохраняется в SQLite (chat.db)

Поддерживается шифрование базы AES-256-CBC с PKCS#7 паддингом

Ключ для шифрования базы — SHA-256 от пользовательского пароля



---

## Логика работы

1. Пользователь запускает приложение и выбирает режим:

Host — создаёт комнату и слушает входящие подключения

Client — подключается к IP и порту хоста



2. После установления соединения выполняется обмен открытыми ключами X25519


3. Генерируется общий симметричный ключ ChaCha20


4. Все сообщения и файлы передаются в зашифрованном виде


5. Приёмник проверяет хеш SHA-256 и расшифровывает сообщение


6. Все события и сообщения сохраняются локально




---

## Основные модули

МОДУЛЬ | НАЗНАЧЕНИЕ 
-------|--------
socket_module/net.py	| Сетевой слой, обмен ключами, ChaCha20-шифрование
ui/main_window.py | Графический интерфейс на PyQt6
db_module/orm.py |	Работа с базой SQLite
db_module/edb.py | Шифрование / расшифровка базы данных
main.py |	Точка входа в приложение



---

## Безопасность

Ключи сеанса создаются заново при каждом подключении

ChaCha20 используется для потокового шифрования сообщений

X25519 обеспечивает безопасный обмен ключами без пароля

SHA-256 гарантирует проверку целостности данных

Нет серверов — трафик передаётся напрямую между участниками

Локальная изоляция — история сообщений хранится только на устройстве пользователя

Обфускация кода возможна при сборке с помощью PyArmor



---


## Лицензия

Программа распространяется свободно и предоставляется «как есть»,
без гарантий. Разрешено использование в исследовательских и образовательных целях





# Anon Decentral Messenger

Madmax — a decentralized peer-to-peer messenger for Python 3 with a PyQt6 graphical interface.  
The application uses modern cryptography for message encryption and does not require central servers.

---

## Features

- P2P interaction without centralized nodes
- Automatic X25519 (ECDH) key exchange
- Symmetric ChaCha20 encryption (256 bit)
- Integrity check via SHA-256
- Session keys, recreated with each connection
- Local SQLite database with optional AES-256-CBC encryption
- Cross-platform (Linux / Windows)

---

## BUILD
GNU/LINUX and UNIX-like system

```bash
source ...
pip install -r requirements.txt
cd madmaxOBSF
pyinstaller --onefile --noconsole --clean main.py \
  --name madmax \
  --collect-all PyQt6 \
  --collect-all cryptography \
  --hidden-import=sqlite3 \
  --hidden-import=PyQt6.QtWidgets \
  --hidden-import=PyQt6.QtCore \
  --hidden-import=PyQt6.QtGui \
  --hidden-import=cryptography.hazmat.bindings._openssl \
  --hidden-import=cryptography.hazmat.bindings._rust \
  --hidden-import=cryptography.hazmat.primitives.asymmetric.x25519 \
  --add-data "ui:ui" \
  --add-data "db_module:db_module" \
  --add-data "socket_module:socket_module" \
  --add-data "../pyarmor_runtime_000000:pyarmor_runtime_000000"
  cd bild
```

# OR

https://mega.nz/fm/qR1hkDDA

---

## Launch

The executable file or shell script **madmax** does not require parameters:

```bash
chmod +x madmax
./madmax
```
For manual selection of the Qt graphical backend:

QT_QPA_PLATFORM=xcb ./madmax       # X11
QT_QPA_PLATFORM=wayland ./madmax   # Wayland


---

## Cryptographic Foundation

# Key Exchange (ECDH, X25519)

1. When connecting, the host and client exchange X25519 public keys.

2. Each party computes a shared secret using the Diffie–Hellman algorithm.

3. A symmetric key (256 bit) is derived from the secret:

shared_key = SHA256(X25519.exchange(private_key, peer_public_key))


# Symmetric Encryption

Algorithm: ChaCha20

Key: 256 bit

Nonce: 128 bit, unique for each packet

Integrity check is performed via SHA-256:

digest = SHA256(nonce || ciphertext || SHA256(key))


If the digest does not match, the packet is rejected.

Local Storage

Chat history is saved in SQLite (chat.db)

AES-256-CBC encryption of the database with PKCS#7 padding is supported

The key for database encryption is the SHA-256 hash of the user password



---

## Workflow

1. The user launches the application and selects a mode:

Host — creates a room and listens for incoming connections

Client — connects to the host's IP and port

2. After establishing a connection, X25519 public keys are exchanged.

3. A shared symmetric ChaCha20 key is generated.

4. All messages and files are transmitted encrypted.

5. The receiver checks the SHA-256 hash and decrypts the message.

6. All events and messages are saved locally.

---

## Core Modules

Module | Purpose
-------|--------
socket_module/net.py | Network layer, key exchange, ChaCha20 encryption
ui/main_window.py | PyQt6 graphical interface
db_module/orm.py | SQLite database operations
db_module/edb.py | Database encryption / decryption
main.py | Application entry point

---

## Security

- Session keys are recreated with each connection.
- ChaCha20 is used for streaming message encryption.
- X25519 ensures secure passwordless key exchange.
- SHA-256 guarantees data integrity verification.
- No servers — traffic is transmitted directly between participants.
- Local isolation — message history is stored only on the user's device.
- Code obfuscation is possible when building with PyArmor.

---

## License

The program is distributed freely and is provided "as is",
without warranties. Permitted for use in research and educational purposes.
