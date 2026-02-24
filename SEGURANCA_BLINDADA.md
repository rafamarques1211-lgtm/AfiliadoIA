# Segurança Blindada

Este documento detalha a implementação de segurança robusta para aplicações, abordando os seguintes aspectos:

## 1. Rate Limiting
O rate limiting é uma técnica que limita o número de requisições que um usuário pode fazer em um determinado período. Isso ajuda a prevenir abusos e ataques de negação de serviço (DoS).

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 100 // limite cada IP a 100 requisições por janela
});

app.use(limiter);
```

## 2. Helmet
Helmet é um middleware para Express que ajuda a proteger suas aplicações definindo várias cabeçalhos HTTP.

```javascript
const helmet = require('helmet');

app.use(helmet());
```

## 3. Autenticação JWT
JSON Web Tokens (JWT) é uma maneira leve e segura para transmitir informações entre partes como um objeto JSON.

### Geração de JWT
```javascript
const jwt = require('jsonwebtoken');

const token = jwt.sign({ userId: user._id }, 'seu_segredo', { expiresIn: '1h' });
```

### Verificação de JWT
```javascript
const verifyToken = (req, res, next) => {
  const token = req.headers['authorization'];
  jwt.verify(token, 'seu_segredo', (err, user) => {
    if (err) return res.sendStatus(403);
    req.user = user;
    next();
  });
};
```

## 4. Refresh Tokens
Os refresh tokens são usados para obter novos access tokens após a expiração dos anteriores, mantendo a usabilidade da aplicação.

### Implementação
```javascript
const refreshTokens = {};

app.post('/login', (req, res) => {
  // Autentica o usuário e gera um refresh token
  const refreshToken = jwt.sign({ userId: user._id }, 'seu_segredo_refresh');
  refreshTokens[user._id] = refreshToken;
  res.json({ refreshToken });
});
```

## 5. Validação de Entrada
A validação de entrada é crucial para prevenir injeções e outros tipos de ataques. Use bibliotecas como Joi ou express-validator.

```javascript
const { body, validationResult } = require('express-validator');

app.post('/user', [
  body('email').isEmail(),
  body('password').isLength({ min: 5 }),
], (req, res) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({ errors: errors.array() });
  }
  // Processa a entrada validada
});
```

## 6. Audit Logging
A auditoria das ações e acessos ajuda a identificar comportamentos suspeitos e possíveis brechas de segurança.

```javascript
const fs = require('fs');

app.use((req, res, next) => {
  const log = `${new Date()} - ${req.method} - ${req.url}\n`;
  fs.appendFile('audit.log', log, err => {
    if (err) console.error(err);
  });
  next();
});
```

---

Estas práticas, quando implementadas corretamente, ajudarão a proteger sua aplicação contra várias vulnerabilidades graves.