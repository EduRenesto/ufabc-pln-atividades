# Resumo (500 words)

O artigo aborda o desafio de reproduzir e expandir métodos de aprendizado de
máquina para provas teóricas, especialmente usando modelos de linguagem de
grande escala (LLMs) em assistentes de prova como o Lean. A dificuldade reside
em códigos privados, dados restritos e altos requisitos computacionais. Para
superar essas barreiras, os autores apresentam o LeanDojo, um ambiente de código
aberto que inclui ferramentas, dados, modelos e benchmarks.

O LeanDojo extrai dados do Lean e permite interação programática com o ambiente
de prova. Ele fornece anotações detalhadas das premissas em provas, crucial para
a seleção de premissas, um gargalo fundamental na prova de teoremas. Utilizando
esses dados, os pesquisadores desenvolvem o ReProver: um provador baseado em LLM
*fine-tuned* para selecionar premissas na biblioteca matemática *mathlib*. O
mecanismo de seleção usa semelhança entre textos gerados a partir dos estados
das provas para identificar as premissas mais adequadas. Este mecanismo é
chamado de *retrieval* e é um dos principais destaques do artigo.

Além disso, é criado um novo conjunto de dados extraído da
biblioteca matemática do Lean, utilizado para treinamento e como benchmark. Esse
conjunto apresenta uma divisão dos dados minuciosa, exigindo que o provador generalize para
teoremas que dependem de premissas novas, não usadas no treinamento. Os
resultados experimentais demonstram a eficácia do ReProver em comparação com
outros modelos, como o GPT-4.

Os principais destaques são a utilização de *retrieval*, e a disponibilização do
código fonte do projeto e da base de dados LeanDojo Benchmark construída.

# Introdução (200 words)

A crise na matemática no começo do século 20 gerou inúmeros desenvolvimentos na
área da lógica matemática, com a vitória decisiva da Teoria Axiomática dos
Conjuntos como fundamentação da matemática para evitar os problemas até então
encontrados.

Uma outra solução, que teve seu maior desenvolvimento alguns anos depois e em
outro contexto, é a Teoria de Tipos. Com o Isomorfismo de Curry-Howard,
provou-se que provas matemáticas correspondem um-a-um com programas de
computador, o que hoje serve de fundamentação teórica para os chamados
Assistentes de Prova, que são ambientes de programação que permitem
formalização, checagem e auxílio em demonstrações de teoremas. O Lean é uma
dessas ferramentas.

Vendo o potencial dos modelos LLM em gerar código, é natural aplicá-los a
assistentes de prova. O LeanDojo consiste numa integração entre o Lean e um
modelo de LLM especialmente treinado para criar um ambiente interativo para
demonstrações matemáticas.

# Problema (100 words)

O LeanDojo não é o primeiro projeto a unir LLMs e assistentes de provas, mas os
trabalhos anteriores sofrem com a falta de interatividade e a dificuldade de
reproducibilidade, principalmente devido a código fonte proprietário e
*datasets* privados.

Além disso, LLMs sem modificações não são eficientes em escolher próximos passos
em demonstrações matemáticas, tendo um espaço de pesquisa proibitivamente
grande.

# Metodologia

O LeanDojo, em primeira ordem, conta com um *dataset* público, batizado de
*LeanDojo Benchmark*, de teoremas, provas, documentação e código em geral
extraído da biblioteca *mathlib*, um grande repositório aberto de matemática
escrito para o Lean. O conjunto de dados é dividido de maneira cuidadosa, de
modo que modelos treinados e validados a partir dele não sejam viesados para
"decorar" teoremas similares. O dataset pode ser encontrado em https://leandojo.org/.

Treinado no conjunto de dados, os autores introduzem o *ReProver*, um modelo LLM
*fine-tuned* para escolher os próximos passos de maneira eficiente. Para tal,
extraem de maneira programática dados de cada estado intermediário numa prova
dentro do Lean, os quais são utilizados para gerar textos que são encodados numa
representação vetorial relevante. Em seguida, o ReProver calcula a semelhança de
textos para descobrir quais serão os melhores próximos passos numa prova. Este
passo de semelhança é o passo de *retrieval*, que é uma das ideias originais do
artigo. O modelo foi escrito em Python, utilizand a biblioteca `transformers`, e
seu código está disponível em https://github.com/lean-dojo/ReProver.

Ainda, os autores introduzem um plugin para o ChatGPT para fazer a integração
direta do chatbot com o LeanDojo. Com tal plugin, é possível "dirigir" o
LeanDojo por meio de linguagem natural, e é possível obter uma experiência de
assistente de prova em matemática informal, também utilizando linguagem natural.
O plugin também é escrito em Python, e seu código está disponível em
https://github.com/lean-dojo/LeanDojoChatGPT.

# Resultados

Com a divisão específica do dataset *LeanDojo Benchmark*, mostrou-se que na verdade a
performance de outros modelos utilizados em conjunto com assistentes de prova,
como o GPT-4, na verdade são menores do que antes publicados.

Comparado com outros modelos, o ReProver com a melhoria de *retrieval* se deu
muito melhor em tarefas de tentativa de provas de teoremas, em especial com a
divisão do dataset do projeto.

O artigo mostra que o *retrieval* é uma boa ideia para ser pesquisada em
trabalhos futuros.

# Refs

Dense Passage Retrieval for Open-Domain Question Answering. Vladimir Karpukhin
(Estados Unidos) (Principal), Barlas Oğuz, Sewon Min, Patrick Lewis, Ledell Yu
Wu, Sergey Edunov, Danqi Chen, Wen-tau Yih. DOI
https://doi.org/10.18653/v1%2F2020.emnlp-main.550. Conference on Empirical
Methods in Natural Language Processing. 13 páginas, 55 referências, 1888 citações.

The Lean Theorem Prover (System Description). L. D. Moura (Estados Unidos)
(Principal), Soonho Kong, J.  Avigad, Floris van Doorn, Jakob von Raumer. DOI
https://doi.org/10.1007/978-3-319-21401-6_26. CADE. 10 páginas, 27 referências,
376 citações.

DeepMath - Deep Sequence Models for Premise Selection. G. Irving (Estados
Unidos) (Principal), Christian Szegedy, Alexander A. Alemi, N. Eén, François
Chollet, J. Urban. DOI https://doi.org/10.48550/arXiv.1606.04442. Neural
Information Processing Systems. 10 páginas, 44 referências, 189 citações.

# Cits

A Language-Agent Approach to Formal Theorem-Proving. Amitayush Thakur (Estados
Unidos) (Principal), Yeming Wen, Swarat Chaudhuri. DOI
https://doi.org/10.48550/arXiv.2310.04353. Ainda não publicado. 15 páginas, 25
referências, 2 citações.
