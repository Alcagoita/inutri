# Mockup do website e da app

Ecrãs de referência para as tarefas da primeira versão (INU-5 a INU-28). Mostram o que se constrói, com a marca e o tom do [CLAUDE.md](../../CLAUDE.md). Não são especificação: em caso de dúvida, a tarefa no Jira e o [docs/decisoes.md](../decisoes.md) mandam.

- **Versão interativa:** https://claude.ai/artifact/QZmrX7SCdgCPgEbhKPdwFi (privada; se não abrir, pedir à Inês).
- **Estes ficheiros** são a fonte dos ecrãs (`.dc.html`) e a disposição do quadro (`canvas.json`). Precisam do visualizador do artifact para mostrar os dados de exemplo; abertos sozinhos no browser, aparecem sem eles.
- **Dados:** todos inventados («Ana Martins», emails `@exemplo.pt`, códigos de barras, valores). Nunca usar dados reais de pacientes aqui.
- **Nome:** «inutri» é provisório até se decidir o nome da app.

## Ecrãs e tarefas

### Website da nutricionista
| Ecrã | Ficheiro | Tarefas |
|---|---|---|
| Entrar (passkey ou código por email) | `Site-Login.dc.html` | INU-6 |
| Pacientes | `Site-Pacientes.dc.html` | INU-7 |
| Gestor de utilizadores (só as contas da app) | `Site-Utilizadores.dc.html` | INU-7 |
| Resumo da paciente, evolução e registos | `Main.dc.html` | INU-8, INU-12, INU-13 |
| Folha do plano (dia tipo) | `Site-Plano.dc.html` | INU-10 |
| Base de alimentos e códigos de barras | `Site-Alimentos.dc.html` | INU-9, INU-27 |
| Ficha clínica (privada) | `Site-Ficha.dc.html` | INU-11 |
| Reports da app | `Site-Reports.dc.html` | INU-14, INU-27 |

### App da paciente
| Ecrã | Ficheiro | Tarefas |
|---|---|---|
| Entrar | `App-Login.dc.html` | INU-15 |
| Consentimento no primeiro acesso | `App-Consentimento.dc.html` | INU-22 |
| Início | `App-Inicio.dc.html` | INU-16, INU-19 |
| Plano | `App-Plano.dc.html` | INU-16 |
| Feedback semanal (grupo com GLP-1) | `App-Feedback.dc.html` | INU-17 |
| O que estás a manter | `App-Semana.dc.html` | INU-17 |
| Evolução | `App-Evolucao.dc.html` | INU-18 |
| Registar medição | `App-Registar.dc.html` | INU-18 |
| Guia de compras (com aviso de "sem rede") | `App-Compras.dc.html` | INU-20, INU-25 |
| Código de barras | `App-Scanner.dc.html` | INU-21 |
| Documentos | `App-Documentos.dc.html` | INU-16 |

### Fase seguinte (fora da primeira versão)
| Ecrã | Ficheiro |
|---|---|
| Diário alimentar com fotos e kcal por IA | `App-Diario.dc.html` |

## Revisão de 24 de novembro de 2026

Os ecrãs foram revistos contra as tarefas INU-5 a INU-28 e as decisões desse dia:

**App**
- O separador Diário saiu da barra. A barra fica com Início, Plano, Compras, Evolução e Documentos.
- O feedback semanal passou a ter as perguntas decididas, pela ordem decidida. O peso e o perímetro estão no fim, são opcionais e vêm pré-preenchidos.
- "O que estás a manter" e o início mostram os comportamentos dessas perguntas. A resposta curta da nutricionista aparece num cartão salmão claro.
- A evolução e o registo de medições têm massa gorda e massa muscular, em kg ou %.
- O guia de compras agrupa os alimentos do plano pela secção do alimento. Também mostra o aviso de "sem rede".
- O login explica o primeiro acesso pelo email do convite e o desbloqueio com Face ID ou impressão digital.
- O ecrã de consentimento é novo.

**Website**
- O login tem passkey ou código por email, e a sessão dura 7 dias.
- O gestor da equipa passou para a fase seguinte. A página de utilizadores fica com as contas da app e explica o que faz desativar o acesso.
- Os pacientes têm a próxima consulta. O aviso "plano a terminar" lembra de marcar a consulta.
- O resumo da paciente tem as perguntas do feedback, a massa gorda e a massa muscular (com a escolha kg ou %), e a próxima consulta.
- A folha do plano tem o dia do feedback semanal, as versões publicadas e o aviso sobre o fim do plano.
- A base de alimentos já não tem sugestões por IA. Tem uma secção por alimento e a lista de códigos de barras ligados.
- Os reports já não têm o diário alimentar. Têm a resposta curta ao feedback e os produtos por ligar.
