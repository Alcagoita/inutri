# inutri — visão do produto

Conclusões da configuração do projeto (22 de novembro de 2026), para toda a equipa que vai trabalhar na inutri. As decisões são tomadas em brainstorming e ficam registadas em [decisoes.md](decisoes.md); este ficheiro resume-as.

## O que é
Uma ferramenta de acompanhamento nutricional para pacientes em perda de peso, **com ou sem** terapêutica com agonistas de GLP-1.

Tem duas partes:

**Parte do profissional** (para o nutricionista)
- Criar o plano de cada paciente, sempre feito à medida.
- Colocar documentos e ferramentas de apoio.
- Registar peso, perímetros e, para quem faz GLP-1, as doses e as datas.
- Ver gráficos de evolução e cruzar, ao longo do tempo, as doses de GLP-1 com a perda de peso e os perímetros.
- Receber os feedbacks semanais e os reports dos pacientes.

**App do paciente** (telemóvel)
- Consultar o seu plano, os seus dados e a sua evolução.
- Ver os documentos e ferramentas de apoio.
- Preencher o feedback semanal e os reports do plano.
- Receber um alerta no dia da toma da medicação (pacientes com GLP-1).

## Promessas (o que a torna diferente)
1. **Só o profissional cria ou altera o plano.** O paciente consulta, nunca altera.
2. **Cada plano é construído à mão, para um paciente.** Sempre diferente de paciente para paciente; nunca gerado ou copiado automaticamente.
3. **Só o profissional coloca documentos e ferramentas de apoio.**
4. **Nenhuma recomendação médica.** A app regista as doses de GLP-1 indicadas pelo profissional e lembra o dia da toma, e mais nada.

Uma mudança que quebre uma promessa precisa primeiro de uma decisão de um dos donos, registada em [decisoes.md](decisoes.md).

## Factos base
1. Cada paciente tem um só profissional.
2. Um plano tem início e fim; o paciente segue um plano de cada vez.
3. O feedback é semanal, com perguntas fixas para cada grupo.
4. As perguntas do feedback são diferentes para quem faz GLP-1.
5. O profissional regista se o paciente faz GLP-1, e isso muda o plano, não só as perguntas.
6. As doses de GLP-1 são registadas pelo profissional, com data, para cruzar com a evolução do peso e dos perímetros.
7. Peso, perímetros e medicação são dados de saúde: a categoria mais sensível de dados pessoais (RGPD).

## O que nunca se faz
- Usar dados reais de pacientes em testes, exemplos ou capturas de ecrã.
- Dar conselhos médicos ou de dose na app.
- Enviar dados de saúde para serviços fora da Europa.

## Marca e tom
Decidido em [INU-3](https://olegarioncnascimento.atlassian.net/browse/INU-3). O nome da app ainda está em aberto.
- **Cores inspiradas na melancia:** verde casca, verde lima (só destaques), verde claro e salmão, com creme, semente (texto), amarelo mel e salmão claro como cores de apoio. Os códigos estão em [CLAUDE.md](../CLAUDE.md).
- **Feedback semanal com curiosidade, não julgamento:** três estados sem vermelho (*Correu bem*, *A construir*, *Ficou para a próxima*), os comportamentos mantidos antes do peso, e cada paciente comparado só consigo próprio.

## Tecnologia e forma de trabalhar
- **Kotlin Multiplatform**, para Android e iPhone.
- **Código:** https://github.com/Alcagoita/inutri (público).
- **Tarefas:** Jira, projeto **INU**, num quadro com quatro colunas: To do, In development, Testing, Done.
- Cada mudança nasce de uma tarefa, vai num branch com o nome da tarefa (`INU-12-…`) e chega por pull request a `develop`. As verificações têm de passar, o outro dono aprova, e qualquer um dos dois decide quando entra.
- As regras completas para a IA estão em [CLAUDE.md](../CLAUDE.md).

## Em aberto
- **Onde corre a parte do profissional:** computador, web ou a mesma app? Ver [INU-2](https://olegarioncnascimento.atlassian.net/browse/INU-2).
- **O que entra na primeira versão.** Também em INU-2.
- **Equipa:** dois donos com o mesmo poder de decisão, Olegário e Inês. Qualquer um decide em brainstorming; cada mudança precisa da aprovação do outro. O quadro do Jira só tem tarefas de execução.
