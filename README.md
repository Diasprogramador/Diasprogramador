# Caio Gonçalves Fernandes Dias

**Desenvolvedor de software — back-end e front-end**
Conselheiro Lafaiete, MG · Concluindo Desenvolvimento de Sistemas

Construo sistemas onde correção importa mais que velocidade de entrega:
idempotência, consistência entre serviços, tolerância a falha. Meus projetos
documentam a decisão junto com a alternativa que foi descartada — porque numa
conversa técnica a alternativa descartada costuma valer mais que a escolhida.

---

## Projetos

### PayFlow — Gateway de pagamentos assíncrono

Gateway que separa a aceitação da cobrança da liquidação junto ao adquirente,
para que uma indisponibilidade externa não vire erro para o lojista. Cinco
módulos Maven, dois executáveis independentes, **129 testes**.

- **Idempotência em duas camadas** — Redis com script Lua atômico para o caso
  comum, e restrição de unicidade no PostgreSQL como garantia final. Há um teste
  que apaga o cache inteiro no meio do fluxo para provar que a proteção
  sobrevive.
- **Transactional outbox** — o evento é gravado na mesma transação de banco que
  a mudança de estado, eliminando a dupla escrita entre banco e broker. O relay
  usa `FOR UPDATE SKIP LOCKED` para escalar horizontalmente.
- **Backoff exponencial com jitter** — um tópico Kafka por nível de retentativa,
  com Dead Letter Queue para o que exige conciliação. Sem jitter, mil cobranças
  que falham juntas retentam juntas e derrubam o adquirente que tenta se
  recuperar.
- **Assinatura HMAC-SHA256** com comparação em tempo constante, anti-replay por
  nonce e rate limit distribuído em token bucket.
- **Arquitetura verificada pelo build** — seis regras de ArchUnit falham a
  compilação se o domínio importar Spring, se um controller acessar o banco
  direto, ou se o número do cartão vazar do pacote que o tokeniza.

`Java 21` `Spring Boot 4.1` `Apache Kafka` `PostgreSQL 17` `Redis` `Keycloak` `Docker` `Next.js 16`

### Grão da Serra — Loja integrando o PayFlow

Loja de café que consome o gateway como um integrador externo de verdade.
Construí-la expôs duas lacunas no PayFlow — não havia provisionamento de lojista
nem webhook — e as duas foram implementadas por causa disso.

O contrato de assinatura é testado **entre as duas linguagens**: vetores gerados
pela implementação TypeScript ficam travados num teste Java. Isso encontrou um
bug real que nenhum teste de um lado só teria pego — o cliente usava a string
base64 do segredo como chave do HMAC, enquanto o gateway usava os bytes
decodificados.

`Next.js 16` `React 19` `TypeScript` `HMAC` `Webhooks`

### [Maquete Industrial](https://github.com/sandersonElias/Maquete_Industrial) — Sistema de monitoramento IoT

Projeto em equipe de quatro pessoas, com 249 commits. **Sou o maior
contribuidor, com 115 deles.** Quatro aplicações integradas e um caminhão
basculante impresso em 3D controlado por Bluetooth.

- **Backend Node** — API REST e WebSocket com dez módulos de rota, autenticação
  JWT, rate limiting, validação com Joi, logging estruturado e exportação de
  relatórios em PDF, CSV e Excel. 52 casos de teste.
- **App Android nativo em Kotlin** — controle por Bluetooth RFCOMM com joystick
  de oito direções, protocolo de comandos próprio e gravação de movimentos para
  modo autônomo.
- **Modelagem 3D procedural em Python** — geração da maquete inteira via `bpy`
  no Blender: extração, beneficiamento, ferrovia, porto, malha rodoviária e
  vegetação, versionada como código em vez de arquivo binário.
- **Visualização 3D no navegador** com Three.js e React Three Fiber.

`Node.js` `Express` `Socket.IO` `PostgreSQL` `Redis` `Kotlin` `Python` `Three.js` `React Native`

### Performance — Área do aluno

Aplicação de academia com autenticação, frequência, ficha de treino e
indicações. **92 casos de teste** e nove migrações versionadas, incluindo
políticas de **Row Level Security**, concessão de privilégios por papel e uma
migração dedicada a endurecimento — o isolamento entre alunos é imposto pelo
banco, não por um `if` na aplicação.

[Ver funcionando](https://academia-performance-opal.vercel.app) ·
`React 19` `TypeScript` `Supabase` `PostgreSQL` `RLS`

### [Flavoro](https://github.com/Diasprogramador/flavoro-delivery) — Aplicativo de delivery

Catálogo de 30 restaurantes e 450 pratos em 13 cozinhas, doze telas navegáveis e
cupons aplicados no checkout. Sistema de design em CSS puro com tokens, sem
framework de componentes.

[Ver funcionando](https://flavoro-delivery-three.vercel.app) ·
`React 19` `TypeScript` `Vite`

### [Brothers Tech](https://github.com/Diasprogramador/Brothers_Tech) — Site de agência

Animação de carregamento, cursor customizado, rolagem suave e transições
coreografadas entre seções, respeitando `prefers-reduced-motion`.

`React` `TypeScript` `GSAP` `Framer Motion` `Lenis`

---

## Stack

Listada por onde eu realmente usei, não por onde já ouvi falar.

| Área | Tecnologias | Onde |
|---|---|---|
| **Back-end** | Java 21, Spring Boot 4, Spring Security, JPA/Hibernate, Flyway | PayFlow |
| | Node.js, Express, Socket.IO, JWT, Joi, Winston | Maquete Industrial |
| **Dados** | PostgreSQL, indexação, RLS, `SKIP LOCKED`, Supabase | PayFlow, Performance |
| | Redis com scripts Lua, Apache Kafka | PayFlow, Maquete |
| **Front-end** | TypeScript, React 19, Next.js 16, Vite, Tailwind CSS | todos |
| | GSAP, Framer Motion, Lenis, Three.js, React Three Fiber | Brothers Tech, Maquete |
| **Mobile** | Kotlin, Android nativo, Bluetooth RFCOMM, React Native | Maquete Industrial |
| **Python** | Blender `bpy` para 3D procedural, geração de `.docx` | Maquete, automações |
| **Testes** | JUnit 5, AssertJ, Testcontainers, ArchUnit, Jest | PayFlow, Performance, Maquete |
| **Infra** | Docker, Docker Compose, Keycloak/OIDC, Git, Vercel | PayFlow, deploys |

---

## Como eu trabalho

- Documento a decisão junto com a alternativa descartada.
- Teste é parte do projeto, não etapa posterior — 221 casos só nos dois projetos
  que fiz sozinho.
- Prefiro falhar alto e cedo a degradar em silêncio.
- Comentário explica *por quê*, não *o quê* — o código já diz o que faz.

---

## Contato

**caiogoncalvesfdias@gmail.com** · Conselheiro Lafaiete, MG
Português nativo · Inglês para leitura técnica e documentação
