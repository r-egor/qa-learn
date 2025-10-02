**SOAP (Simple Object Access Protocol)** — протокол обмена данными по сети, обычно через **XML**. Старый стандарт, до сих пор встречается в крупных корпоративных системах.

---

### 🔹 Особенности SOAP

|Характеристика|Описание|Для QA|
|---|---|---|
|📦 Формат|Всегда **XML**|Нужно уметь читать XML|
|🛡 Безопасность|Поддерживает WS-Security (подписи, шифрование)|Проверка токенов, сертификатов|
|📑 Контракт|WSDL-файл описывает API (эндпоинты, типы, методы)|Можно проверить корректность документации|
|🔄 Протоколы|Обычно HTTP/HTTPS, но может работать и с SMTP|Не ограничено только вебом|
|⚖ Строгая структура|Жёсткие правила, меньше гибкости|Сложнее тестировать, но меньше хаоса|

---
### 📊 Пример SOAP-запроса

``` XML
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
                  xmlns:usr="http://example.com/user">
   <soapenv:Header/>
   <soapenv:Body>
      <usr:GetUserRequest>
         <usr:UserId>123</usr:UserId>
      </usr:GetUserRequest>
   </soapenv:Body>
</soapenv:Envelope>
```

**Пример ответа:**

``` XML
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
   <soapenv:Body>
      <usr:GetUserResponse>
         <usr:UserId>123</usr:UserId>
         <usr:Name>Alice</usr:Name>
         <usr:Email>alice@example.com</usr:Email>
      </usr:GetUserResponse>
   </soapenv:Body>
</soapenv:Envelope>
```

---
### 📦 XML — формат данных для SOAP API

**XML (eXtensible Markup Language)** — текстовый формат для структурированных данных.  
Похож на HTML, но предназначен для передачи данных между системами, а не для отображения.  

**Особенности для QA:**
- Структура из тегов `<Tag>` … `</Tag>`  
- Поддерживает вложенность и сложные объекты  
- Может быть проверен через XSD-схему (валидация типов и структуры)  
- Менее компактный и читаемый, чем JSON, но строгий и стандартизированный  

### 📑 WSDL и XSD

**WSDL (Web Services Description Language)**  
XML-документ, который описывает API: какие методы доступны, какие параметры, где находится эндпоинт.  
👉 Это как "паспорт" API.  

  **Пример фрагмента WSDL:**
  ```xml
  <definitions name="UserService"
       targetNamespace="http://example.com/user"
       xmlns="http://schemas.xmlsoap.org/wsdl/">
    <portType name="UserPortType">
      <operation name="GetUser">
        <input message="tns:GetUserRequest"/>
        <output message="tns:GetUserResponse"/>
      </operation>
    </portType>
  </definitions>
```
Здесь описано, что у сервиса есть операция `GetUser`, у которой есть вход (`GetUserRequest`) и выход (`GetUserResponse`).

**XSD (XML Schema Definition)**  
Файл, который описывает структуру и типы XML.  
👉 Гарантирует, что `<UserId>` — это число, `<Email>` — строка в правильном формате.

**Пример XSD-схемы:**
``` XML
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="GetUserRequest">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="UserId" type="xs:int"/>
        <xs:element name="Email" type="xs:string"/>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
</xs:schema>
```
Здесь указано, что `UserId` — это **целое число**, а `Email` — **строка**.

---
### 🌍 Где применяется SOAP API

- Корпоративные системы и банки (платёжные шлюзы, CRM, ERP)
- Государственные сервисы (например, обмен данными между ведомствами)
- Системы с жёсткой структурой данных и безопасностью
- Проекты, где критична строгая типизация и контракт через WSDL/XSD

### 🔍 Что проверяет QA

- ✅ Корректность XML-структуры
- ✅ Соответствие схемам (XSD/WSDL)
- ✅ Ошибки валидации (отсутствующие теги, неверные типы)
- ✅ Сообщения об ошибках сервера