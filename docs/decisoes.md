# inutri — decisões

As decisões são tomadas em brainstorming com o dono que está na sessão, e essa decisão é final. Cada decisão fica aqui registada com o que se decidiu, quem decidiu, a data e porquê, e depois transforma-se em tarefas de execução no Jira. O quadro do Jira só tem tarefas de execução: trabalho com resultado visível na app ou no website.

As mais recentes ficam no topo.

## 24 de novembro de 2026

### O que entra na primeira versão
- **Decisão:** a primeira versão inclui, além do que foi decidido na INU-2 e na INU-4:
  - **Website:** página de reports, onde chegam os feedbacks semanais, as medições e as perguntas das pacientes; confirmar ou corrigir as medições registadas pelas pacientes; marcar em que supermercados se encontra cada alimento.
  - **App:** registar peso e medidas; guia de compras por supermercado; leitor de código de barras que diz se o produto está no guia e em que refeição do plano entra.
- **Fica para a fase seguinte:** diário alimentar com fotos e kcal estimadas por IA; gestor da equipa no website (convidar outras nutricionistas ou administradores).
- **Quem:** Inês Nascimento.
- **Porquê:** os reports e as medições pela paciente fecham o ciclo entre a app e o website. O guia de compras e o código de barras ajudam a paciente a seguir o plano no dia a dia. O diário com IA precisa de um serviço de IA alojado na Europa e tem mais riscos e custos, por isso espera.
- **Para confirmar antes de construir:** licença da tabela do INSA; uso da base Open Food Facts (licença ODbL) para identificar os produtos pelo código de barras.

## 23 de novembro de 2026

### Perguntas do feedback semanal
- **Decisão:** o feedback semanal tem estas perguntas fixas. Os comportamentos vêm primeiro; o peso e o perímetro abdominal vêm no fim, são opcionais e aparecem já preenchidos se a paciente os registou nessa semana.
- **Comuns aos dois grupos:**
  1. Fome/apetite: baixo · equilibrado · alto
  2. Energia: baixa · média · boa
  3. Sono: mal · razoável · bem
  4. Água ao longo do dia: dias em que conseguiu (0–7)
  5. Atividade física: dias com pelo menos 30 minutos (0–7)
  6. Adesão ao plano: dias em que seguiu o plano (0–7)
  7. Principal dificuldade desta semana (texto livre)
  8. O que correu bem? (texto livre)
  9. O que quero melhorar na próxima semana? (texto livre)
  10. Peso e perímetro abdominal (opcionais)
  11. Queres dizer alguma coisa à Inês? (texto livre)
- **Só para o grupo com GLP-1:**
  - Proteína de acordo com o plano: dias em que conseguiu (0–7)
  - Sintomas que sentiste com frequência esta semana (escolher os que se aplicam): náuseas · vómitos · enfartamento · obstipação · diarreia · refluxo · nenhum destes. Serve só para a nutricionista ajustar o plano; a app não dá nenhum conselho sobre sintomas.
- **Só para o grupo sem GLP-1:**
  - Comer entre refeições, fora do plano: dias (0–7)
  - Houve momentos em que comeste por stress ou cansaço? nunca · às vezes · várias vezes
- **Quem:** Inês Nascimento.
- **Porquê:** desloca a atenção da paciente de "quanto perdi esta semana?" para "que comportamentos estou a conseguir manter?". As respostas em dias permitem mostrar tendências ("Mantiveste 4 dias, mais 1 do que na semana passada").

### Medidas registadas
- **Decisão:** além do peso e dos perímetros (cintura/perímetro abdominal e anca), registam-se a **massa gorda** e a **massa muscular**.
- **Quem:** Inês Nascimento.
- **Porquê:** a evolução da composição corporal diz mais do que o peso sozinho.

### Formato do plano
- **Decisão:** o plano é um **dia tipo** (refeições e quantidades), não um plano diferente para cada dia da semana.
- **Quem:** Inês Nascimento.
- **Porquê:** é como os planos já são feitos e é mais simples para a paciente seguir.

### Supermercados no guia de compras
- **Decisão:** é a nutricionista que marca, no website, em que supermercados encontra cada alimento. O guia de compras da app mostra só os alimentos marcados para o supermercado escolhido pela paciente.
- **Quem:** Inês Nascimento.
- **Porquê:** os supermercados não publicam as listas de produtos de forma aberta, e a marcação feita à mão é fiável.
- **Nota:** o âmbito do guia de compras (primeira versão ou fase seguinte) ainda não está decidido.

### Sem tarefas de decisão no quadro
- **Decisão:** as decisões saem de brainstorming e ficam registadas neste ficheiro. O quadro do Jira só tem tarefas de execução.
- **Quem:** Inês Nascimento.
- **Porquê:** o quadro deve mostrar o trabalho que vai ser construído, não discussões.

### Folha do plano, base de alimentos, ficha clínica e doses na app (INU-4)
- **Decisão:** entram na primeira versão a folha de cálculo do plano, a base de alimentos (a partir da tabela do INSA, editável e com alimentos adicionados à mão), a ficha clínica privada por paciente e as doses de medicação visíveis na app.
- **Quem:** Inês Nascimento.
- **Condição:** confirmar a licença da tabela do INSA antes de a importar.
- **Detalhe:** [INU-4](https://olegarioncnascimento.atlassian.net/browse/INU-4).

### Identidade visual e tom do feedback (INU-3)
- **Decisão:** paleta inspirada na melancia e feedback semanal com curiosidade, não julgamento.
- **Quem:** Inês Nascimento.
- **Detalhe:** secção "Brand and tone" do [CLAUDE.md](../CLAUDE.md) e [INU-3](https://olegarioncnascimento.atlassian.net/browse/INU-3).

### Website para a nutricionista e primeira versão (INU-2)
- **Decisão:** a parte da nutricionista é um website, ligado à app por um servidor comum que guarda os dados de saúde na Europa.
- **Quem:** Inês Nascimento.
- **Detalhe:** [INU-2](https://olegarioncnascimento.atlassian.net/browse/INU-2).
