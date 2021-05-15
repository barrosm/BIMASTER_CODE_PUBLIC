# Fatores de Risco de Fundos Multimercado Brasileiros

#### Aluno: Monica Barros (https://github.com/barrosm)
#### Orientador: Felipe Borges (https://github.com/FelipeBorgesC)

---

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

- [Link para o código](https://github.com/barrosm/BIMASTER_CODE_PUBLIC/tree/master).


---

### Resumo

A indústria de fundos de investimentos no Brasil vem apresentando crescimento expressivo na última década. Com a queda na SELIC, a taxa básica de juros,a busca dos investidores por estratégias mais rentáveis (e arriscadas) impulsionou o crescimento da classe de fundos multimercado. 

Estes se caracterizam por uma política de investimentos que envolve vários fatores de risco, sem o compromisso de concentração em nenhum deles. Os multimercados podem aplicar em diferentes mercados (renda fixa, câmbio e ações, entre outros), além de usar derivativos para alavancagem ou proteção da carteira. Eles preveem uma maior liberdade de gestão e buscam oferecer aos investidores um rendimento mais alto que em aplicações conservadoras.

Em 2020 a indústria de fundos cresceu e alcançou, no fim do ano, um patrimônio líquido de R$ 6 trilhões. Isso se deveu, principalmente, à captação positiva nos fundos de ações e multimercado [https://investnews.com.br/financas/industria-de-fundos-cresce-e-fecha-2020-com-patrimonio-de-r-6-trilhoes/)].

Neste trabalho procuramos identificar quais os fatores de risco que “explicam” o estilo dos principais fundos multimercado do país. A metolologia empregada é chamada de “Análise de Estilo baseada no retorno” (Return Based Style Analysis, RBSA) e foi originalmente proposta por William Sharpe num artigo publicado em 1988.  A RBSA busca inferir a que classes de ativos um fundo está exposto baseando-se apenas na série de retornos.

Empregamos dados diários de quotas de fundos multimercado entre Janeiro de 2017 e Fevereiro de 2021. A amostra exclui os fundos que, no cadastro da CVM, não estão em funcionamento normal ou são “exclusivos” (fundos com um só cotista, geralmente um investidor institucional como fundo de pensão ou seguradora).

Em março de 2020 ocorreram diversos “circuit-breakers” na B3 (Bolsa de Valores de São Paulo) por conta das incertezas ocasionadas pela epidemia de Covid-19. Esta alta volatilidade não foi privilégio dos ativos brasileiros – as bolsas mundiais desabaram à medida que ficava clara a dimensão da pandemia. 

Usamos inúmeras fontes de dados. Para os fundos e seus retornos empregamos a base da CVM (Comissão de Valores Mobiliários). No caso dos fatores de risco utilizamos as APIs do Yahoo Finance, Quandl, Ipeadata e FRED (Federal Reserve, EUA). As APIs Quandl e  FRED requerem cadastramento prévio. As chaves de acesso não foram mostradas nos programas desenvolvidos aqui e o usuário deve obtê-las antes de usar as respectivas APIs.

O trabalho foi desenvolvido em Python e está estruturado como um conjunto de Jupyter Notebooks que devem ser executados de maneira sequencial, respeitando a ordem "XXX_nome_do_arquivo.ipynb" onde XXX é o número do notebook. Assim, o notebook cujo nome começa com 000 deve ser executado antes daquele cujo nome começa com 001, e este antes do nomeado “002_.....ipynb” e assim sucessivamente. Também, o usuário deverá alterar o diretório raiz para execução dos códigos. Os códigos foram executados num container Docker com Jupyter Notebook, e o diretório padrão para execução dos códigos é “'/home/jovyan/work/@Fund_Eval/”. O leitor interessado pode obter informações sobre os containers Docker com Jupyter em [(https://jupyter-docker-stacks.readthedocs.io/en/latest/)].

Coletamos 38 “fatores de risco” diferentes, que correspondem a riscos referentes à renda fixa (pré e pós fixada), câmbio (dólar e euro), volatilidade do câmbio, mercado acionário brasileiro e americano e preços de petróleo. Destes, selecionamos 13 fatores mostrados na próxima tabela.

![Tabela 1](https://github.com/barrosm/BIMASTER_CODE_PUBLIC/blob/main/trab_final_fig1.jpg)

A seguir concentramos o trabalho nos fundos que compõem o IHFA (Indíce de Hedge Funds Anbima). Este índice busca ser uma referência no mercado, e é atualizado diariamente.  Segundo a Anbima (https://www.anbima.com.br/pt_br/informar/precos-e-indices/indices/ihfa.htm) , algumas das condições para inclusão no índice são:
- O fundo precisa estar enquadrado como multimercado há mais de um ano,
- Fundos fechados ou que não cobram taxa de performance não são considerados para a composição do índice,
- Fundos com menos de 10 quotistas no trimestre anterior à data de rebalanceamento do índice são excluídos,
- Fundos que não divulgam suas quotas diariamente são excluídos.


A amostra consiste em 257 fundos, para os quais calculamos os percentuais relativos a cada fator de risco.  Este problema de estimação dos percentuais de cada “fator de risco”, ou seja, a descoberta do “estilo” de cada fundo, se resume a um problema de mínimos quadrados em que impomos as seguintes restrições:
- Os percentuais (pesos) de cada fator são positivos. Esta restrição implica em que todos os fundos estão “comprados“ nas diversas classes de ativos e não são permitidas vendas a descoberto em categorias de estilo.
- A soma dos percentuais de cada fator é igual a um.
- Aplicamos a suposição adicional de que o peso máximo é igual a 0,35.

A próxima tabela apresenta os percentuais de alocação nos diversos fatores de risco para os dez fundos de maior peso na composição do IHFA.

![Tabela 2](https://github.com/barrosm/BIMASTER_CODE_PUBLIC/blob/main/trab_final_fig2.jpg)

O  mapa de calor a seguir mostra os percentuais de alocação nos diversos fatores para os 100 maiores fundos componentes do IHFA.

![Figura 1 - Mapa de Calor](https://github.com/barrosm/BIMASTER_CODE_PUBLIC/blob/main/trab_final_fig3.jpg)

Nota-se a predominância em aplicações em títulos públicos. Os fundos têm, em geral, grande exposição a títulos pós-fixados (LTFs, representados pelo índice IMA-S), indexados ao IPCA com prazo até 5 anos (IMA-B5) e pré-fixados de prazo inferior a 1 ano (IRF-M1). Uma parcela bem menor parece ser alocada em Bolsa de Valores brasileira(GM366_IBVSP366) ou americana (SP500). Alguns poucos fundos parecem ter exposição relevante em dólar. Também, o “alpha”, que representaria a contribuição do gestor acima dos retornos dos fatores a que o fundo está exposto, é zero, ou muito pouco acima de zero. Isso poderia sugerir que os fundos não agregam valor em relação ao que obteria através de uma carteira que simplesmente reproduzisse de maneira passiva os índices empregados aqui como fatores de risco.



---

Matrícula: 191.671.015

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
