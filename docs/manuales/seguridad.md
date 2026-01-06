# Seguridad

> Principios, herramientas y prácticas para proteger sistemas, datos y usuarios contra amenazas y vulnerabilidades.

---

## 🎯 Seguridad como Cultura

**Qué:** Integrar seguridad en cada fase del desarrollo (Shift-Left Security).

**Por qué:** Detectar vulnerabilidades temprano cuesta 100x menos que en producción. Una brecha puede destruir reputación y negocio.

**Quién:** Todo el equipo: developers, DevOps, architects, QA.

**Costo:** Inversión 15% del tiempo de desarrollo, previene incidentes catastróficos.

---

## 🔐 Principios Fundamentales

| Principio | Qué | Por qué | Cuándo | Cómo | Herramientas |
| :---------- | :----- | :---- | :----- | :---- | :------------- |
| **Least Privilege** | Acceso mínimo necesario | Limitar impacto de compromiso | Siempre | RBAC, políticas IAM granulares | [AWS IAM](https://aws.amazon.com/iam/), [Keycloak](https://www.keycloak.org/) |
| **Zero Trust** | Nunca confiar, siempre verificar | No asumir red interna = segura | Redes corporativas, cloud | Autenticar cada request, micro-segmentación | [BeyondCorp](https://cloud.google.com/beyondcorp), [Istio](https://istio.io/) |
| **Defense in Depth** | Múltiples capas de seguridad | Si una falla, otras protegen | Toda la arquitectura | WAF + Firewall + Encryption + Auth + Monitoring | Stack completo |
| **Fail Secure** | Fallar en estado seguro | Mejor denegar que permitir por error | Validaciones, autorizaciones | `if (!authorized) throw 403`, no `if (authorized) allow` | Code review |
| **Separation of Duties** | Nadie tiene acceso completo solo | Prevenir acciones maliciosas unilaterales | Operaciones críticas | Aprobaciones multi-persona, 4-eyes principle | [PagerDuty](https://www.pagerduty.com/), workflow engines |

---

## 🛡️ OWASP Top 10 (2021)

| Vulnerabilidad | Qué | Por qué crítico | Cómo prevenir | Ejemplo |
| :--------------- | :----- | :------------ | :-------------- | :-------- |
| **A01: Broken Access Control** | Usuarios acceden a recursos no autorizados | Exposición de datos sensibles | Validar autorización en backend, no confiar en cliente | Usuario cambia `user_id` en URL y ve datos ajenos |
| **A02: Cryptographic Failures** | Datos sensibles sin cifrado adecuado | Robo de datos en tránsito/reposo | TLS 1.3, cifrado AES-256, no almacenar contraseñas en claro | Contraseñas en texto plano en DB |
| **A03: Injection** | SQL, NoSQL, LDAP, OS command injection | Ejecución de código arbitrario | Prepared statements, ORMs, validación de inputs | `SELECT * FROM users WHERE name = '${input}'` |
| **A04: Insecure Design** | Arquitectura sin controles de seguridad | Vulnerabilidades estructurales | Threat modeling, secure by design | API sin rate limiting → DDoS |
| **A05: Security Misconfiguration** | Configs por defecto inseguras | Puertas traseras abiertas | Hardening, deshabilitar debug en prod | Admin panel expuesto, errores con stack traces |
| **A06: Vulnerable Components** | Librerías desactualizadas con CVEs | Exploits conocidos públicos | Dependency scanning, actualizar regularmente | Log4Shell (CVE-2044228) |
| **A07: Auth & Session Failures** | Autenticación o sesiones débiles | Robo de identidad, session hijacking | MFA, tokens seguros (JWT firmados), HTTPS only | Sesiones sin timeout, IDs predecibles |
| **A08: Software & Data Integrity** | Código/datos sin verificación de integridad | Supply chain attacks, tampering | Firmar artifacts, verificar checksums, SRI | NPM package malicioso en dependencies |
| **A09: Logging & Monitoring Failures** | Ausencia de logs/alertas | Ataques no detectados | Logs estructurados, alertas en anomalías, SIEM | Intento de login masivo no detectado |
| **A10: SSRF** | Server-Side Request Forgery | Acceso a recursos internos | Whitelist de URLs, no permitir localhost | `fetch(userInput)` → accede a `http://localhost:8080/admin` |

---

## 🔑 Autenticación y Autorización

| Mecanismo | Qué | Por qué | Cuándo | Cómo |
| :---------- | :----- | :---- | :----- | :---- |
| **JWT** | JSON Web Token: token autofirmado | Stateless, escalable | APIs REST, microservicios | Firmar con secret (HS256) o keypair (RS256), validar en cada request |
| **OAuth 2.0** | Delegación de autorización | No compartir contraseñas | Integraciones third-party (Login con Google) | Authorization Code Flow con PKCE |
| **SAML** | Security Assertion Markup Language | SSO empresarial | Organizaciones con IdP central | Identity Provider emite assertions XML |
| **MFA/2FA** | Multi-Factor Authentication | Segunda capa de defensa | Cuentas sensibles, admin panels | TOTP (Google Authenticator), SMS, U2F/WebAuthn |
| **API Keys** | Identificador simple | APIs internas, scripts | M2M communication | Rotar regularmente, scopes limitados |
| **mTLS** | Mutual TLS: cliente y servidor se autentican | Máxima seguridad M2M | Microservicios críticos | Certificados client-side + server-side |

---

## 🔒 Secrets Management

**Qué:** Gestión centralizada de credenciales, API keys, certificados.

**Por qué:** Evitar hardcodear secrets en código (Git history es eterno).

**Ver [Capítulo 37 - Gestión de Secretos](./gestion-secretos.md) para implementación detallada de Vault, AWS Secrets Manager, Azure Key Vault y mejores prácticas.**

---

## 🛡️ Patrones de Seguridad Avanzados

| Patrón | Qué | Por qué | Cuándo | Cómo | Herramientas |
| :------- | :----- | :---- | :----- | :---- | :------------- |
| **WAF** | Web Application Firewall | Filtrar tráfico HTTP malicioso | Apps públicas | Reglas contra SQL injection, XSS, bots | [Cloudflare WAF](https://www.cloudflare.com/waf/), [AWS WAF](https://aws.amazon.com/waf/) |
| **DDoS Protection** | Mitigar ataques de denegación | Mantener disponibilidad | Servicios públicos | Rate limiting, CDN, anycast | [Cloudflare](https://www.cloudflare.com/), [AWS Shield](https://aws.amazon.com/shield/) |
| **Key Rotation** | Rotar claves criptográficas periódicamente | Limitar ventana de compromiso | Siempre | Generar nueva key, re-encriptar, deprecar antigua | Vault Transit Engine, KMS |
| **Audit Logging Inmutable** | Logs append-only, no modificables | Forense, compliance | Sistemas regulados (finanzas, salud) | Blockchain privado, append-only storage | [AWS QLDB](https://aws.amazon.com/qldb/), [Hyperledger Fabric](https://www.hyperledger.org/projects/fabric) |
| **HSTS** | HTTP Strict Transport Security | Forzar HTTPS | Toda app con datos sensibles | Header: `Strict-Transport-Security: max-age=31536000` | Nginx, Apache |
| **CSP** | Content Security Policy | Prevenir XSS, data injection | Toda app web | Header: `Content-Security-Policy: default-src 'self'` | Meta tag, server headers |
| **CORS** | Cross-Origin Resource Sharing | Controlar qué origins acceden | APIs públicas | Whitelist de origins permitidos | Backend config |

---

## 🔍 Security Testing

| Tipo | Qué | Por qué | Cuándo | Cómo | Herramientas |
| :----- | :----- | :---- | :----- | :---- | :------------- |
| **SAST** | Static Application Security Testing | Detectar bugs en código fuente | CI/CD pipeline | Analizar código sin ejecutarlo | [SonarQube](https://www.sonarsource.com/products/sonarqube/), [Checkmarx](https://checkmarx.com/) |
| **DAST** | Dynamic Application Security Testing | Detectar vulnerabilidades en runtime | Staging, prod (controlled) | Testear app running como atacante | [OWASP ZAP](https://www.zaproxy.org/), [Burp Suite](https://portswigger.net/burp) |
| **SCA** | Software Composition Analysis | Escanear dependencias por CVEs | Pre-commit, CI | Analizar `package.json`, `requirements.txt` | [Snyk](https://snyk.io/), [Dependabot](https://github.com/dependabot) |
| **Penetration Testing** | Simular ataque real | Validar defensas | Trimestralmente, pre-launch | Contratar pentesters o red team interno | [Metasploit](https://www.metasploit.com/), [Kali Linux](https://www.kali.org/) |
| **Fuzz Testing** | Inputs aleatorios/malformados | Encontrar crashes, overflows | APIs, parsers | Generar millones de inputs inválidos | [AFL](https://github.com/google/AFL), [libFuzzer](https://llvm.org/docs/LibFuzzer.html) |

---

## 🔐 Cifrado

| Tipo | Qué | Cuándo | Algoritmos | Ejemplo |
| :----- | :----- | :----- | :----------- | :-------- |
| **Simétrico** | Misma key para cifrar/descifrar | Datos en reposo, comunicación interna | AES-2GCM | Cifrar DB backups |
| **Asimétrico** | Keypair pública/privada | Firmas, intercambio de keys | RSA-4096, Ed25519 | HTTPS (TLS), SSH |
| **Hashing** | One-way, no reversible | Contraseñas, integridad | bcrypt, Argon2, SHA-256 | Almacenar passwords |
| **HMAC** | Hash con key secreta | Validar integridad + autenticidad | HMAC-SHA256 | Webhooks, API signatures |
| **TLS** | Transport Layer Security | Datos en tránsito | TLS 1.3 | HTTPS, gRPC |

---

## 🚨 Incident Response

| Fase | Qué | Cómo |
| :----- | :----- | :---- |
| **Preparation** | Plan, runbooks, contactos | Documentar escalation, access a logs |
| **Detection** | Identificar brecha | SIEM, alertas, anomalías |
| **Containment** | Limitar daño | Aislar sistemas afectados, revocar tokens |
| **Eradication** | Eliminar amenaza | Patchear vulnerabilidad, eliminar backdoors |
| **Recovery** | Restaurar servicio | Validar integridad, monitoring aumentado |
| **Lessons Learned** | Postmortem | Blameless, actualizar runbooks |

---

## 📋 Security Checklist

### Aplicación

- [ ] HTTPS en todos los endpoints
- [ ] Inputs validados y sanitizados (backend)
- [ ] Outputs encoded (prevenir XSS)
- [ ] CSRF tokens en formularios
- [ ] Rate limiting en APIs públicas
- [ ] Passwords hasheados (bcrypt/Argon2)
- [ ] Secrets en Vault, no en código
- [ ] Dependency scan en CI
- [ ] CORS configurado correctamente
- [ ] Headers de seguridad (CSP, HSTS, X-Frame-Options)

### Infraestructura

- [ ] Firewall configurado (allow list)
- [ ] SSH con keys, no passwords
- [ ] Ports innecesarios cerrados
- [ ] Logs centralizados e inmutables
- [ ] Backups cifrados y testeados
- [ ] IaC versionado en Git
- [ ] Secrets rotados trimestralmente
- [ ] MFA para accesos admin
- [ ] Network segmentation
- [ ] Auto-patching de OS

---

## 🚫 Anti-patrones

| Anti-patrón | Problema | Solución |
| :------------ | :--------- | :--------- |
| **Security through obscurity** | Confiar en que atacante no conoce sistema | Asumir que sí conoce, aplicar defensa real |
| **Hardcoded secrets** | Credentials en código | Vault, environment variables |
| **Admin interface expuesta** | `/admin` sin IP whitelist | VPN, IP allowlist, strong auth |
| **No input validation** | Confiar en cliente | Validar TODO en backend |
| **Logging PII/passwords** | Datos sensibles en logs | Sanitizar antes de loguear |

---

## 📚 Recursos

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [CWE Top 25](https://cwe.mitre.org/top25/)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security)
