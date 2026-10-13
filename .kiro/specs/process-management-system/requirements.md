# Requirements Document

## Introduction

O **Sistema de Gerenciamento de Processos Internos** é uma plataforma que permite organizações mapear, documentar, analisar e melhorar seus processos de negócio. O sistema suporta o registro de processos com suas etapas, responsáveis, entradas e saídas, além de possibilitar a documentação do estado atual (AS-IS) e do estado futuro desejado (TO-BE), facilitando iniciativas de melhoria contínua.

---

## Glossary

- **Sistema**: O Sistema de Gerenciamento de Processos Internos como um todo.
- **Usuário**: Pessoa autenticada que interage com o sistema.
- **Gestor**: Usuário com permissão para criar, editar e publicar processos.
- **Analista**: Usuário com permissão para visualizar processos e registrar melhorias.
- **Processo**: Conjunto estruturado de etapas com início e fim definidos que produz um resultado de valor para a organização.
- **Etapa**: Unidade atômica de trabalho dentro de um processo, com responsável, entradas e saídas definidas.
- **Responsável**: Papel ou pessoa designada para executar uma etapa.
- **Entrada**: Recurso, documento ou informação necessários para iniciar ou executar uma etapa.
- **Saída**: Resultado, documento ou informação produzidos pela execução de uma etapa.
- **Versão_AS_IS**: Representação documentada do processo no estado atual de execução.
- **Versão_TO_BE**: Representação documentada do processo no estado futuro desejado após melhorias.
- **Mapeamento**: Ato de criar ou atualizar a documentação de um processo.
- **Publicação**: Ato de tornar uma versão de processo visível para todos os usuários com acesso.

---

## Requirements

### Requisito 1: Cadastro de Processos

**User Story:** Como Gestor, quero cadastrar processos internos da organização, para que todos os processos relevantes estejam documentados e acessíveis de forma centralizada.

#### Critérios de Aceitação

1. THE Sistema SHALL permitir que o Gestor crie um novo Processo informando nome (obrigatório, máximo 200 caracteres), descrição (obrigatória, máximo 2000 caracteres), área responsável (obrigatória) e objetivo (obrigatório, máximo 1000 caracteres).
2. WHEN o Gestor submeter um formulário de criação de Processo com campos obrigatórios em branco, THE Sistema SHALL exibir uma mensagem de erro identificando cada campo obrigatório não preenchido, sem submeter o formulário.
3. WHEN o Gestor cadastrar um Processo com sucesso, THE Sistema SHALL atribuir um identificador único e imutável ao Processo, registrar a data e hora da criação e o identificador do Usuário responsável pela criação.
4. THE Sistema SHALL permitir que o Gestor edite o nome, descrição, área responsável e objetivo de um Processo existente cujo status seja ativo.
5. WHEN o Gestor tentar criar um Processo com um nome idêntico ao de outro Processo já existente na mesma área responsável, THE Sistema SHALL exibir uma mensagem de aviso e aguardar confirmação explícita do Gestor antes de prosseguir com a criação.
6. IF o Gestor confirmar a criação de um Processo com nome duplicado na mesma área, THEN THE Sistema SHALL criar o Processo e exibir ambos os Processos na listagem, distinguindo-os pelo identificador único.
7. WHEN o Gestor solicitar o arquivamento de um Processo ativo, THE Sistema SHALL alterar o status do Processo para inativo e impedir sua edição, sem excluí-lo permanentemente.
8. WHEN um Processo for arquivado, THE Sistema SHALL preservar todas as Etapas, versões e histórico associados ao Processo, mantendo-os acessíveis para consulta.

---

### Requisito 2: Descrição e Gestão de Etapas

**User Story:** Como Gestor, quero descrever as etapas de cada processo, para que a sequência de trabalho fique clara e documentada.

#### Critérios de Aceitação

1. THE Sistema SHALL permitir que o Gestor adicione uma ou mais Etapas a um Processo, informando nome (obrigatório, máximo 100 caracteres), descrição (obrigatória, máximo 1000 caracteres) e ordem de execução (inteiro positivo único dentro do Processo).
2. THE Sistema SHALL permitir que o Gestor reordene as Etapas de um Processo por meio de uma interface de arrastar e soltar ou por atribuição manual de sequência numérica, garantindo que nenhuma Etapa fique sem posição definida após a reordenação.
3. WHEN o Gestor remover uma Etapa de um Processo, THE Sistema SHALL recalcular automaticamente a sequência das Etapas restantes de forma contínua, sem lacunas, iniciando em 1.
4. THE Sistema SHALL permitir que o Gestor defina o tipo de fluxo entre Etapas como sequencial, paralelo ou condicional, com valor padrão sequencial quando nenhum tipo for explicitamente definido.
5. WHEN o Gestor salvar alterações em uma Etapa, THE Sistema SHALL registrar a data, hora (com precisão de minutos) e o identificador do Usuário responsável pela modificação no histórico da Etapa.
6. IF uma Etapa não possuir nome ou ordem de execução preenchidos, THEN THE Sistema SHALL impedir o salvamento e exibir mensagem indicando os campos obrigatórios ausentes.
7. WHEN o Gestor tentar remover uma Etapa que possua instâncias de execução ativas vinculadas, THE Sistema SHALL bloquear a remoção e exibir mensagem informando que a Etapa não pode ser removida enquanto houver instâncias ativas.
8. IF o Gestor definir o tipo de fluxo de uma Etapa como condicional, THEN THE Sistema SHALL exigir o preenchimento de ao menos uma condição de transição antes de permitir o salvamento da Etapa.

---

### Requisito 3: Identificação de Responsáveis

**User Story:** Como Gestor, quero identificar os responsáveis por cada etapa do processo, para que a accountability de cada atividade fique explícita e rastreável.

#### Critérios de Aceitação

1. WHEN o Gestor atribuir Responsáveis a uma Etapa, THE Sistema SHALL permitir a seleção de um até 20 Responsáveis por Etapa, escolhidos entre papéis ou usuários cadastrados na organização.
2. WHEN o Gestor atribuir um Responsável a uma Etapa, THE Sistema SHALL exigir que o Gestor classifique o tipo de responsabilidade desse Responsável como executor, aprovador ou consultado.
3. WHEN o Gestor salvar uma Etapa sem nenhum Responsável do tipo executor atribuído, THE Sistema SHALL exibir um aviso indicando a ausência de executor e permitir que o Gestor confirme o salvamento ou retorne para corrigir a atribuição, salvando a Etapa somente após confirmação explícita.
4. WHEN o Gestor substituir o Responsável de uma Etapa, THE Sistema SHALL registrar no histórico da Etapa o nome ou papel do Responsável anterior, o seu tipo de responsabilidade, a data de atribuição e a data de substituição, mantendo esse registro acessível ao Gestor sem limite de entradas.
5. WHEN o Gestor visualizar uma Etapa, THE Sistema SHALL exibir a lista de Responsáveis ativos da Etapa, apresentando para cada um o nome ou papel, o tipo de responsabilidade e a data de atribuição.
6. IF uma Etapa possuir histórico de Responsáveis substituídos, THEN THE Sistema SHALL permitir que o Gestor acesse o histórico de Responsáveis da Etapa, exibindo para cada entrada o nome ou papel, o tipo de responsabilidade, a data de atribuição e a data de substituição.

---

### Requisito 4: Registro de Entradas e Saídas

**User Story:** Como Gestor, quero registrar as entradas e saídas de cada etapa, para que os insumos necessários e os resultados produzidos estejam documentados e facilitem a análise de fluxo de informações.

#### Critérios de Aceitação

1. WHEN o Gestor adicionar uma Entrada a uma Etapa, THE Sistema SHALL exigir o preenchimento obrigatório de nome (máx. 200 caracteres) e tipo (documento, dado, sistema ou material), e aceitar opcionalmente origem (máx. 500 caracteres).
2. WHEN o Gestor adicionar uma Saída a uma Etapa, THE Sistema SHALL exigir o preenchimento obrigatório de nome (máx. 200 caracteres) e tipo (documento, dado, sistema ou material), e aceitar opcionalmente destino (máx. 500 caracteres).
3. WHEN o Gestor vincular a Saída de uma Etapa como Entrada de outra Etapa do mesmo Processo, THE Sistema SHALL exibir visualmente essa conexão no diagrama do Processo como uma seta direcional rotulada com o nome da Saída/Entrada vinculada.
4. WHEN o Gestor remover uma Saída que esteja vinculada como Entrada de outra Etapa do mesmo Processo, THE Sistema SHALL exibir aviso informando as Etapas que perderão o vínculo e exigir confirmação explícita antes de concluir a remoção.
5. WHEN o Gestor solicitar a edição ou remoção de uma Entrada ou Saída de uma Etapa, THE Sistema SHALL aplicar a alteração somente na Etapa selecionada, sem afetar as Entradas e Saídas das demais Etapas do Processo.
6. WHEN o Analista solicitar a exportação da lista de Entradas e Saídas de um Processo, THE Sistema SHALL gerar o arquivo no formato selecionado (CSV ou PDF), incluindo para cada item o nome, tipo, origem ou destino, e a Etapa à qual pertence.

---

### Requisito 5: Documentação AS-IS e TO-BE

**User Story:** Como Gestor, quero documentar o processo atual (AS-IS) e o processo futuro (TO-BE), para que seja possível comparar o estado presente com o estado desejado e planejar melhorias de forma estruturada.

#### Critérios de Aceitação

1. WHEN o Gestor solicitar a criação de uma versão de Processo, THE Sistema SHALL permitir que ele escolha o tipo da versão como Versão_AS_IS ou Versão_TO_BE, criando as duas independentemente para cada Processo.
2. WHEN o Gestor publicar uma Versão_AS_IS ou Versão_TO_BE, THE Sistema SHALL registrar a data de publicação e o identificador do Usuário responsável, e tornar a versão somente leitura, bloqueando edições futuras sem que uma nova versão seja criada a partir dela.
3. WHEN o Gestor solicitar a comparação de uma Versão_AS_IS com uma Versão_TO_BE do mesmo Processo, THE Sistema SHALL exibir as duas versões lado a lado, destacando com indicador visual distinto cada Etapa adicionada, removida ou modificada na Versão_TO_BE em relação à Versão_AS_IS.
4. IF o Gestor tentar comparar versões e uma das versões selecionadas não existir, THEN THE Sistema SHALL exibir mensagem informando qual versão está ausente e não iniciar a comparação.
5. WHEN o Gestor solicitar a criação de uma nova versão a partir de uma versão existente, THE Sistema SHALL criar uma cópia editável com o conteúdo da versão de origem, preservando a versão original como somente leitura.
6. WHEN o Gestor iniciar a criação de uma Versão_TO_BE, THE Sistema SHALL oferecer a opção de importar a estrutura da Versão_AS_IS publicada mais recente como ponto de partida; IF não existir nenhuma Versão_AS_IS publicada, THEN THE Sistema SHALL iniciar a Versão_TO_BE em branco sem oferecer a opção de importação.
7. WHEN o Usuário solicitar a visualização do histórico de versões de um Processo, THE Sistema SHALL exibir a lista de todas as versões publicadas do Processo, incluindo para cada versão o tipo (AS-IS ou TO-BE), a data de publicação e o identificador do Usuário que publicou.

---

### Requisito 6: Análise e Melhoria de Processos

**User Story:** Como Analista, quero analisar os processos documentados e registrar oportunidades de melhoria, para que a organização possa evoluir continuamente seus processos internos.

#### Critérios de Aceitação

1. WHEN o Analista solicitar o registro de uma oportunidade de melhoria, THE Sistema SHALL criar o registro com status inicial "aberta", vinculado a uma Etapa específica ou ao Processo como um todo, exigindo obrigatoriamente descrição (máx. 2000 caracteres) e prioridade ("baixa", "média" ou "alta"), e aceitando opcionalmente impacto esperado (máx. 1000 caracteres).
2. WHEN o Gestor solicitar a associação de uma oportunidade de melhoria a uma Versão_TO_BE, THE Sistema SHALL registrar a associação, vinculando a oportunidade à versão indicada.
3. WHEN o Gestor publicar uma Versão_TO_BE que contenha oportunidades de melhoria associadas, THE Sistema SHALL atualizar o status dessas oportunidades de "aberta" para "incorporada".
4. WHEN o Usuário acessar o painel consolidado, THE Sistema SHALL exibir o total de Processos cadastrados, o total de Etapas, o número de oportunidades de melhoria com status "aberta" e o número de Versões_TO_BE publicadas, apresentando valor zero quando não houver registros em cada categoria.
5. WHEN o Usuário aplicar filtros ou busca na listagem de Processos, THE Sistema SHALL retornar os Processos que correspondam aos critérios informados de nome, área responsável e/ou status ("ativo" ou "arquivado"), exibindo no máximo 50 resultados por página.
6. WHEN o Analista solicitar a exportação da documentação de um Processo, THE Sistema SHALL gerar um arquivo PDF contendo Etapas, Responsáveis, Entradas, Saídas e as versões AS-IS e TO-BE existentes, omitindo seções cujo conteúdo ainda não tenha sido registrado e indicando no documento quais seções foram omitidas.
7. IF o Analista tentar registrar uma oportunidade de melhoria vinculada a uma Etapa ou Processo que não existe, THEN THE Sistema SHALL rejeitar o registro e exibir uma mensagem de erro indicando que o vínculo informado não foi encontrado, sem criar o registro.

---

### Requisito 7: Controle de Acesso

**User Story:** Como administrador da organização, quero controlar quais usuários podem criar, editar ou apenas visualizar processos, para que a integridade das documentações seja preservada e o acesso seja apropriado a cada perfil.

#### Critérios de Aceitação

1. THE Sistema SHALL diferenciar exatamente três perfis de acesso — Administrador, Gestor e Analista — onde o Administrador pode criar, editar, publicar, arquivar e gerenciar perfis de Usuários; o Gestor pode criar, editar, publicar e arquivar Processos, mas não gerenciar perfis; e o Analista pode apenas visualizar Processos.
2. WHEN um Usuário com perfil Analista tentar criar, editar, publicar ou arquivar um Processo, THE Sistema SHALL negar a operação, exibir mensagem informando que o perfil não possui permissão para esta ação, e manter o estado atual do Processo inalterado.
3. WHEN um Usuário com perfil Gestor tentar atribuir ou revogar perfis de acesso a outros Usuários, THE Sistema SHALL negar a operação e exibir mensagem informando que o perfil não possui permissão para esta ação.
4. WHEN o Administrador submeter a atribuição ou revogação de um perfil de acesso a um Usuário, THE Sistema SHALL aplicar a alteração imediatamente, sem necessidade de reinicialização, fazendo com que o Usuário afetado passe a operar sob o novo perfil na próxima ação que executar.
5. WHEN um Usuário não autenticado tentar acessar qualquer funcionalidade do Sistema, THE Sistema SHALL redirecionar o Usuário para a tela de autenticação e descartar a requisição original.
6. WHEN um Usuário autenticado executar com sucesso qualquer operação de criação, edição, publicação ou arquivamento de Processo, THE Sistema SHALL registrar em log a operação, o identificador do Processo, o identificador do Usuário, e a data e hora da operação com precisão de segundos, retendo cada entrada de log por no mínimo 90 dias.
7. IF o Sistema falhar ao registrar uma entrada de log após uma operação de criação, edição, publicação ou arquivamento de Processo, THEN THE Sistema SHALL exibir mensagem de erro indicando falha no registro de auditoria e reverter a operação realizada.
