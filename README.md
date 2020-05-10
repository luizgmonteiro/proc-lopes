# Processo Seletivo - Lopes Labs<br/>

## Perguntas<br/>
***1. Como foi a definição da sua estratégia de modelagem?***<br/>
R: Na análise inicial foi possível obter informações importantes sobre os dados:
* A variável de interesse é discreta. *quality* possui valores de 3 a 9. Dado isso, a escolha por uma regressão logística  multivariável ao invés de uma regressão linear multivariável se deu por questões de métrica de avaliação. Avaliar um output contínuo contra uma variável discreta torna a avaliação do modelo errônea, além de dificultar consideravelmente a otimização de hiperparâmetros do modelo. Poderia sim arredondar o output do modelo para metrificar, mas desse modo perderia uma parte preciosa da avaliação da curva ideal e de predição do modelo.
* A distribuição da variável de interesse é muito próxima de uma normal: sendo desbalanceada a escolha da métrica de avaliação no aprendizado não poderia ser geral.<br/>

***2. Como foi definida a função de custo utilizada?***<br/>
R: A função de custo foi decidida como a função de custo de regressão logística multivariável, onde y pode assumir valores contínuos entre 0 e 1, identificando a probabilidade de determinado outcome ser positivo. Essa função é definida pelo próprio XGBoost e segue a paremetrização da função objetivo de regressão logística em tree gradient boost, onde a função objetivo é:<br/>
<img src="https://render.githubusercontent.com/render/math?math=\sum_{i=1}^n [g_i f_t(x_i) + \frac{1}{2} h_i f_t^2(x_i)] + \Omega(f_t)"><br/>
E a função de custo da regressão logística:<br/>
<img src="https://render.githubusercontent.com/render/math?math=J(\theta) = - \dfrac{1}{m} [\sum_{i=1}^{m} y^{(i)} \log(h_\theta(x^{(i)})) + (1 - y^{(i)}) \log(1-h_\theta(x^{(i)}))]"> 

<br/>
***3. Qual foi o critério utilizado na seleção do modelo final?***<br/>
R: O modelo considerado final foi baseado em alguns aprendizados que obtive durante a análise sobre os dados, somada à anlásie das métricas. Na análise é possível verificar que foram identificados diferentes grupos com diferentes características na avaliação e por isso optei pelo ensemble de vários modelos no final. Os modelos com XGBoost foram brevemente comparados com um MLP. É comum no meu dia a dia no trabalho comparar várias implementações de modelos de forma a buscar aquele que mais se adequa a determinado problema. As comparações são feitas por exemplo entre XGBoost, LGBM, SVC, etc. No caso dessa análise a comparação foi mais demonstrativa do que de fato prática em relação ao MLP. O maior critério utilizado foi a performance medida em acurácia.<br/>

***4. Qual foi o critério utilizado para a validação do modelo? Por que escolheu utilizar esse método?***<br/>
R: A validação do modelo foi feita utilizando as métricas AUC e f1.<br/>
* Por que AUC? Um dataset desbalanceado como o do teste dificulta uma análise concisa das métricas de precision, recall e f1, mesmo que se analisadas separadamente. Sempre costumo validar tais métricas com AUC, uma vez que um alto valor abaixo da curva implica numa avaliação melhor distribuída e maior confiança nos valores de precision e recall.  <br/> 
* Por que f1? A princípio busco alta precisão nas predições para analisar o que o modelo leva mais em consideração ***no dataset*** para fazer as previsões. A partir daí tomo decisões para o modelo generalizar melhor. Uma forma de se fazer isso é observar a relação existente entre precision e recall, possibilitada de forma intuitiva com f1.<br/>

***5. Quais evidências você possui de que seu modelo é suficientemente bom?***<br/>
R: As evidências colhidas para aprovar o modelo como final foram as métricas de f1 para a maioria das classes, com bastante espaço para melhora, porém. <br/>
É importante citar que, analisar um modelo envolve não só olhar para as métricas obtidas, mas também o contexto no qual será aplicado. No caso do teste obviamente não havia muito o que considerar além das métricas, claro -- Muitas vezes são necessários testes A/B, verificar a escalabilidade, comparação de performance contra o mecanismo atual de classificação, se houver, e assim por diante.
