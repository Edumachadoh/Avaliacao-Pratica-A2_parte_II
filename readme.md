# Questão 1

### Padrão: Strategy

O Padrão Strategy é usado aqui para mudar dinamicamente o algoritmo de cálculo de risco (VaR, ES, Stress) em tempo de execução, como visto no Client.java.

Ele permite que a classe Context execute um cálculo sem precisar saber os detalhes de como ele é feito; ela apenas delega a tarefa para a "estratégia" atual (um objeto Algorithm).

Isso evita o uso de condicionais (if/else) dentro do Context e torna o sistema flexível, pois novos algoritmos (como ValueAtRiskAlgorithm ou ExpectedShortfallAlgorithm) podem ser adicionados ou trocados facilmente sem alterar o Context.

# Questão 2

### Padrão: Adapter

O Padrão Adapter é usado para resolver a incompatibilidade entre a interface moderna ProcessadorTransacoes e o SistemaBancarioLegado.

O Client deseja chamar um método limpo, como autorizar(1600.0, "...", "BRL"). No entanto, o sistema legado espera um HashMap complexo e dados específicos, como codigoMoeda=3 e um codigoLoja obrigatório.

A classe PagamentoAdapter atua como um "tradutor bidirecional". Ela converte a chamada simples do cliente no HashMap que o legado entende (a "ida") e, em seguida, converte a resposta HashMap do legado de volta no objeto RespostaAutorizacao que o cliente espera (a "volta"). Isso permite que o Client utilize o sistema legado sem nunca saber de sua complexidade.

# Questao 3

### Padrão: State

O Padrão State é usado aqui para permitir que o objeto UsinaNuclear altere seu comportamento dinamicamente com base em sua condição interna (como Normal, Alerta ou Emergência).

Em vez de ter métodos (manutencao, emergencia) com lógica if/else complexa para verificar o estado atual, a UsinaNuclear delega essas ações diretamente para o objeto de estado que ela possui.

Cada classe de estado concreta (como OperacaoNormal ou AlertaVermelho) implementa a interface State e contém a lógica específica e as regras de transição. Por exemplo, emergencia() falha no estado OperacaoNormal, mas funciona e transiciona o estado em AlertaVermelho.

# Questao 4

### Padrão: Chain of Responsibility

O Padrão Chain of Responsibility é usado aqui para criar uma cadeia de validações para um DocumentoFiscal.

Ele desacopla o Cliente dos validadores individuais; o Cliente apenas monta a cadeia e envia a solicitação (o ValidacaoContexto) para o primeiro elo.

Cada Handler (como ValidadorSchemaHandler ou ValidadorCertificadoHandler) é focado em uma única regra de negócio.

Um objeto de ValidacaoContexto é passado ao longo da cadeia, permitindo que os Handlers compartilhem estado, contem falhas (para o Circuit Breaker) e registrem tarefas de Rollback.

O AbstractValidadorHandler controla a lógica de passagem, verificando context.deveParar() antes de invocar o próximo Handler.

Isso torna o sistema flexível, pois a ordem dos validadores pode ser alterada ou novos podem ser adicionados no Cliente sem modificar a lógica dos Handlers.