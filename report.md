# Vulnerabilidade – OWASP Juice Shop  
## Acesso Administrativo via Enumeração + Brute Force  
**Autor:** Vinícius Santos  
**Data:** 12/11/2025  
Ambiente: Juice Shop rodando em Docker  
Finalidade: Estudo prático de Segurança da Informação

---

## 1. Resumo

Durante testes no Juice Shop, foi possível comprometer a conta administrativa através da combinação de duas falhas:

1. Exposição de e-mails nos comentários dos produtos  
2. Ausência total de proteção contra tentativa de força bruta no login  

Com essas falhas, um script automatizado conseguiu:

- Enumerar produtos  
- Coletar e-mails expostos publicamente  
- Identificar o e-mail administrativo  
- Realizar brute force até encontrar a senha correta  
- Acessar o painel de administrador com sucesso  

Resultado: acesso total ao administrador  
Gravidade: Crítica

---

## 2. O que foi identificado

### Exposição de e-mails nos comentários
Os produtos utilizam IDs previsíveis:

/product/1
/product/2
/product/3


A partir disso, foi possível coletar comentários e identificar e-mails de usuários, incluindo:

admin@juice-sh.op


### Login sem proteção contra brute force
O endpoint de login não possui:

- Bloqueio por tentativas  
- Atraso progressivo  
- CAPTCHA  
- Rate limit  
- MFA  
- Monitoramento de tentativas excessivas  

Isso torna o brute force viável sem qualquer restrição.

---

## 3. Prova de Conceito (PoC)

### 3.1 Enumeração automática
Um script percorreu os IDs dos produtos para coletar e-mails expostos.

Adicionar print:  
`![Enumeração](./screenshots/enum.png)`

---

### 3.2 Ataque de força bruta
Com o e-mail administrativo identificado, o script testou uma lista de senhas até obter sucesso.

Adicionar print:  
`![Brute Force](./screenshots/bruteforce.png)`

---

### 3.3 Acesso administrativo
Após o brute force, o login foi bem-sucedido e o painel administrativo ficou acessível.

Adicionar print:  
`![Admin](./screenshots/admin.png)`

## 4. Impacto

- Acesso a informações sensíveis  
- Controle total da aplicação  
- Possibilidade de criar, editar e remover dados  
- Modificação de contas e configurações internas  
- Possibilidade de ataques subsequentes  
- Risco organizacional crítico  

Impacto final: Crítico

## 5. Recomendações de mitigação

- Remover e-mails reais de páginas públicas  
- Implementar bloqueio de tentativas, atrasos e rate limit  
- Adicionar CAPTCHA após tentativas falhas  
- Implementar MFA para contas administrativas  
- Exigir senhas fortes e verificar senhas vazadas  
- Melhorar o logging e alertas de comportamento suspeito  
- Revisar o código responsável por exibir e armazenar dados sensíveis  

## 6. Conclusão

A combinação de e-mails expostos e login sem proteções básicas permitiu um ataque simples que resultou em comprometimento completo da conta administrativa. Com as correções recomendadas, o risco é reduzido de forma significativa.

Relatório produzido exclusivamente para fins educacionais em ambiente controlado.
