# Modelagem de um Sistema de Manufatura

## Introdução

As **Redes de Petri** são uma poderosa ferramenta matemática e gráfica para modelar, analisar e simular sistemass dinâmicosa eventos discretos. Elas foram introduzidas por Carl Adam Petri em 1962 e são amplamente utilizadas para a maodelagem de fluxos de trabalho, redes de computadores, sistemas distribuídos, entre outroas aplicaçãoes.

A estrutura de uma Rede de Petri consiste em quatro elementos principais:
- Places: Podem representar estados, consições ou recursos no sistema;
- Transitions: Representam os eventos ou mudanças de estado;
- Arches: Conectam os *places* e as *transitions*, indicando o fluxo entre eles;
- Tokens: Representam a distribuição de recursos ou o estado atual do sistema.
  
Uma extensão destas é a classe de Redes de Petri Coloridas (CPN), que introduzem o conceito de cor. Com isso, cada *token* pode ser diferenciado por atributos, permitindo representar informações adicionais sobre o sistema. As CPNs permitem modelar sistemas mais complexos sem aumentar significativamente a quantidade de lugares e transições no sistema, ideias para lidar com sistemas que apresetnam múltiplos tipos de objetos ou recursos, e possui uma base matemática formal sólida que permite realizar análises rigorosas, comoa  identificação de gargalos ou comportamentos indesejados.

Este projeto propõe utilizar desse modelo a partir da ferramenta gráfica **CPN Tools**  para modelar um sistema de manufatura comporto por células de manufatura como as ilustradas na figura 1:

<div style="text-align: center;">
    <img src="imagens/Célula.png" alt="Célula de Manufatura" />
    <p><strong>Figura 1:</strong> Cálula de MAnufatura.</p>
</div>

Cada célula apresenta duas rotas para os recursos: i e j. Ambas as rotas passam pelo robô 1, máquina 1 e robô 2. Nesse momento, as rotas se dividem para as máquinas 2 e 3, passam pelo robô 3 e seguem para a saída da célula. O sistema apresenta uma capacidade, de modo que os depósios de entrada e saída das máquinas suportam no máximo 4 itens(*tokens*), e não deve apresentar bloqueios.


## Vídeo Explicativo

## Materiais e Método

Para a modelagem e análise gráfica do sitema de manufatura, foi utilizado o software [CPN Tools](http://cpntools.org), desenvolvido pela universidade de Aarhus, Dinamarca, e está disponível gratuitamente para usos não comerciais.
Esse software fornece ferramentas gráficas e em linguagem *Standard ML* para simulação de CPNs de forma intuitiva e completa.

A priori, cada elemento do sistema foi desenhado no software como lugar ou transição. Os depósitos de entrada e saída, tanto das máquinas como das células são lugares de uma CPN. As transições representam a movimentação dos robôs que transportam a carga de um lugar a outro e o processo realizado pelas máquinas, que não é analisado no escopo do projeto, e é abstraído, bastando apenas o conhecimento de que as máquinas 2 e 3 recebem tipos diferentes de recurso.
O esquema de uma célula de manufatura é apresentado na figura 2.

<div style="text-align: center;">
    <img src="imagens/Esquema.png" alt="Esquema de uma Célula" />
    <p><strong>Figura 2:</strong> Esquema de uma Célula de Manufatura.<br>Fonte: O próprio autor</p>
</div>

As cores de uma CPN são determinados no software pela declaração de *colorsets*, uma definição de tipo de dado, que especifica características dos *tokens*. O tipo de dado(cor) transmitido na célula é definido como PACKAGE, um *colorset* composto por um *colorset* inteiro, que serve apenas para identificação do recurso, e outro **ROUTE** do tipo **i** ou **j**, que determina a rota que o recurso deve seguir. Eles são declarados da seguinte maneira:

```sml
colset ROUTE = with i | j;
colset PACKAGE = record route: ROUTE * id: INT;
```
Para controlar a quantidade de tokens em um lugar na CPN, é necessária a utilziação de um *anti-place*, declarado tipo **E**, que fisicamente não faz parte do sistema, mas como o CPN não oferece um método para lidar diretamente com limites de tokens, é necessário essa aplicação, onde para cada lugar da rede, existe um *anti-place* que controla a quantidade de *tokens* máximos no lugar correspondente.

Note na imagem, que a robô 2 e o robô 3 são represetnados por duas transições cada, uma vez que é a ação de transporte do robô, a represetnada por uma transição, e não o robô em si. De modo que, o evento de transportar a carga do depósito de saída da máquina 1 para o depósito de entrada da máquina 2 e para o da máquina 3 são duas transições diferentes, bem como transportar o recurso dos depósitos de entrada de duas máquians diferentes para a saída da célula.

##Resultados e Conclusões
Essa estrutura cumpre os requisitos do projeto, onde os dados de cor **i** são transportados pela rota **i** da *figura 1*, e os dados de cor **j** são transportados pela rota **j**. Além disso, os depósitos de entrada e saída das máquinas possuem um limite de 4 itens por vez, de modo que ao atingir esse limite, a transição anterior a este lugar é desabilitada. E por fim, o sistema não apresenta bloqueio. A simulação automática do sistema ocorreu sem problemas e imprevistos. Todos os dados da entrada da célula são transportados para a saída da mesma via sua rota correspondente.

Pode-se dizer que o uso de uma CPN foi extremamente eficiente em modelar o sistema de manufatura, e que a ferramenta CPN Tools foi eficiente na esquematização, modelagem gráfica e simulação da CPN utilizada para represetnar o modelo deste projeto.

## Referências

- JENSEN, Kurt; KRISTENSEN, Lars M. *Coloured Petri Nets: Modelling and Validation of Concurrent Systems*. Springer, 2009. Disponível em: [https://link.springer.com/book/10.1007/b95112](https://link.springer.com/book/10.1007/b95112). Acesso em: 03 abr. 2025.

- CPN Tools. Universidade de Aarhus, Dinamarca. Disponível em: [http://cpntools.org](http://cpntools.org). Acesso em: 03 abr. 2025.




