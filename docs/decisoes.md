# inutri — decisões

As decisões são tomadas em brainstorming com o dono que está na sessão, e essa decisão é final. Cada decisão fica aqui registada com o que se decidiu, quem decidiu, a data e porquê, e depois transforma-se em tarefas de execução no Jira. O quadro do Jira só tem tarefas de execução: trabalho com resultado visível na app ou no website.

As mais recentes ficam no topo.

## 28 de novembro de 2026 — endereço temporário do website

- **Decisão:** enquanto a app não tem nome nem domínio, o website de teste funciona no endereço gratuito do **Cloudflare Pages** (`<projeto>.pages.dev`), ligado ao projeto Supabase de **teste**. Cada pull request tem também um link de pré-visualização próprio. Quando houver nome, compra-se o domínio e liga-se no Cloudflare; o endereço `pages.dev` continua a funcionar.
- **Regras até haver domínio:**
  - O nome "inutri" fica **num só sítio** da configuração (app e website), para mudar numa linha.
  - Os links do login e da palavra-passe (Supabase Auth) aceitam o endereço `pages.dev`; quando houver domínio, acrescenta-se.
  - As passkeys ficam ligadas ao endereço: as de teste no `pages.dev`; voltam a registar-se uma vez no domínio final.
  - Os emails de teste (Brevo) saem de um único endereço verificado; no domínio final verificam-se SPF e DKIM.
  - **Nenhuma paciente real antes do domínio final** (e dos textos legais e acordos de dados).
- **Quem:** Olegário Nascimento.
- **Porquê:** não comprar um domínio antes do nome, sem parar o trabalho: o website é construído e testado online desde o início, no mesmo serviço que vai usar em produção.

## 24 de novembro de 2026 — detalhes para as tarefas

Decididos pelo Olegário Nascimento ao rever as tarefas INU-5 a INU-28. O modelo de dados completo está em [modelo-de-dados.md](modelo-de-dados.md).

- **Unidades:** peso em kg; cintura e anca em cm; massa gorda e massa muscular em **kg ou %**, guardadas com a unidade usada em cada registo. A nutricionista escolhe como os gráficos de cada paciente as mostram, e a conversão usa o peso do mesmo dia (sem peso nesse dia, mostra-se a unidade registada). Dose = medicamento + mg + data.
- **Estados do feedback:** respostas em dias (0–7): *Correu bem* 5–7, *A construir* 3–4, *Ficou para a próxima* 0–2; ao contrário para "comer entre refeições, fora do plano". Respostas de três opções: melhor, meio, pior.
- **Resposta ao feedback:** a nutricionista escreve **uma resposta curta** a cada feedback semanal, que a paciente vê na app. Não é um chat.
- **Guia de compras:** são os **alimentos do plano**, filtrados pelo supermercado escolhido. Cada alimento tem uma secção (fruta, laticínios, …) na base de alimentos.
- **Sugestões por IA na base de alimentos:** **fora da primeira versão.** A nutricionista escreve os valores ou copia-os do rótulo. Sem custos nem serviços extra.
- **Alterar um plano publicado:** cria uma **nova versão**; publicar substitui o que a app mostra, e todas as versões publicadas ficam no histórico. Os planos de uma paciente nunca se sobrepõem.
- **Filtro "plano a terminar":** planos que acabam nos próximos **7 dias**, com uma mensagem para marcar também a próxima consulta.
- **Desativar o acesso:** a paciente deixa de entrar; os dados ficam todos e o acesso pode ser reativado. Apagar dados é um pedido à parte (INU-22).
- **Alojamento do website:** **Cloudflare Pages**. Só guarda os ficheiros do site; os dados ficam na Supabase (UE).

## 24 de novembro de 2026 — arquitetura técnica

Brainstorming de arquitetura com o Olegário Nascimento. Desbloqueia a [INU-5](https://olegarioncnascimento.atlassian.net/browse/INU-5).

### Servidor e base de dados
- **Decisão:** **Supabase**, alojado na região da UE (Frankfurt): base de dados PostgreSQL, autenticação, armazenamento de ficheiros e regras de acesso por linha. Dois projetos separados: **teste** (só dados inventados) e **produção**.
- **Quem:** Olegário Nascimento.
- **Porquê:** é o caminho com menos trabalho para uma equipa de duas pessoas: base de dados, login, ficheiros e permissões num só serviço, com os dados guardados na Europa. O PostgreSQL encaixa nos dados (pacientes → planos → refeições → alimentos, medições com data).
- **Notas:** a Supabase é uma empresa americana; os dados ficam na região europeia. Assinar o acordo de tratamento de dados (DPA) da Supabase antes de entrarem dados reais. As regras partilhadas (somas do plano, perguntas do feedback) ficam na base de dados, para a app e o website calcularem igual. Cada paciente pertence a uma profissional nos dados desde o início, a pensar no gestor da equipa.

### Website
- **Decisão:** o website da nutricionista é feito em **React + TypeScript**. A app continua em Kotlin Multiplatform.
- **Quem:** Olegário Nascimento.
- **Porquê:** ferramentas maduras para a folha do plano e para gráficos, biblioteca oficial da Supabase, e comporta-se como um site normal (copiar texto, zoom, leitores de ecrã).
- **Em aberto:** onde fica alojado o website (de preferência na Europa). O website não guarda dados de saúde; vai buscá-los à Supabase.

### Login da nutricionista (website)
- **Decisão:** email e palavra-passe, e depois um segundo passo: **passkey** (impressão digital, Touch ID ou Windows Hello) e, em alternativa, um **código por email**. A sessão dura **7 dias** no mesmo computador; "Sair" termina-a logo.
- **Quem:** Olegário Nascimento.
- **Porquê:** o website tem as fichas clínicas de todas as pacientes, por isso o segundo passo fica; a passkey é mais segura e mais rápida do que um código. Pedir login a cada 30 minutos (como dizia a INU-6) era impraticável.
- **Para confirmar antes de construir:** que a Supabase Auth suporta passkeys e código por email como segundo passo. Se não suportar, usar o que suportar mais perto disto e voltar a decidir.

### Login da paciente (app)
- **Decisão:** login normal com email e palavra-passe. Não há registo na app: só a nutricionista cria contas. Quando cria a conta, a paciente recebe um email a dizer que tem uma conta à espera de palavra-passe, e define-a numa **página web simples**, que só serve para definir e recuperar a palavra-passe. Depois entra na app. A app fica com a sessão aberta e **pede Face ID, impressão digital ou o código do telemóvel** ao abrir.
- **Quem:** Olegário Nascimento.
- **Porquê:** a paciente só entra, sem passos extra; o bloqueio ao abrir protege os dados de saúde se o telemóvel estiver desbloqueado noutras mãos.

### Envio de emails
- **Decisão:** **Brevo** (empresa francesa, dados na UE), ligado à Supabase para convites, recuperação de palavra-passe e códigos. Os emails nunca levam dados de saúde.
- **Quem:** Olegário Nascimento.

### Erros e falhas
- **Decisão:** **Sentry, região da UE**, na app e no website, configurado para nunca enviar dados de saúde nem dados pessoais.
- **Quem:** Olegário Nascimento.

### App sem internet
- **Decisão:** a app **lê sem internet**: o plano, o guia de compras e os documentos ficam guardados no telemóvel, cifrados, e são apagados ao sair da conta. Feedbacks e medições são enviados quando voltar a haver rede.
- **Quem:** Olegário Nascimento.
- **Porquê:** o guia de compras usa-se no supermercado, onde a rede falha muitas vezes.

### Lembretes
- **Decisão:** o lembrete do dia da toma é **marcado no próprio telemóvel** a partir do dia definido no plano (sem notificações enviadas pelo servidor). Há também um **lembrete do feedback semanal**, no dia que a nutricionista define para cada paciente; nesse dia fecha a semana do feedback. Os textos das notificações nunca levam dados de saúde.
- **Quem:** Olegário Nascimento.
- **Porquê:** funciona sem rede e sem passar dados pelos serviços de notificações da Google e da Apple.

### Fim do plano
- **Decisão:** quando o plano chega à data de fim, **deixa de aparecer na app**, que mostra "À espera do teu novo plano". Os planos antigos ficam guardados no website como histórico.
- **Quem:** Olegário Nascimento.

### Código de barras e alimentos
- **Decisão:** um código de barras é um produto concreto e liga-se a um alimento da base de **duas formas**: a nutricionista liga no website os produtos mais comuns; quando a paciente lê um produto desconhecido, ele chega aos reports e a nutricionista liga-o uma vez, ficando a funcionar para todas as pacientes.
- **Quem:** Olegário Nascimento.
- **Porquê:** a base tem alimentos genéricos ("arroz agulha") e o código identifica uma marca e um tamanho; sem esta ligação o leitor não sabe a que refeição o produto pertence.

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
