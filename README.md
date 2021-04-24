<div align="center">
  <img src="https://user-images.githubusercontent.com/61476935/115932683-c3e1ba00-a463-11eb-94c8-a906fcd5ad8b.png">
</div>
<br>     
<img src="https://img.shields.io/static/v1?label=Angular&message=Library&color=purple&style=for-the-badge&logo=Angular"/>

<h2>
  Store
</h2>

<h5>
  Store é o gerenciador de estados globais do RxJS para aplicações Angular, inspirado pelo Redux.
  Sendo um container de controle de estados, o Store foi desenvolvido para ajudar na criação de 
  aplicações consistentes e performáticas sobre a estrutura do Angular.
</h5>

<h3>
  Conceitos Chave
</h3>

<ul>
  <li>
   Ações descrevem eventos únicos que são despachados dos componentes e service;
  </li>
  <li>
   Mudanças de estado são gerenciadas por funções puras chamadas de <strong>reducers</strong>,
   as quais tomam o estado atual e a ação mais recente para criarem um novo estado;
  </li>
  <li>
   Selectors são funções puras usadas para selecionar, derivar e compor partes de um estado;
  </li>
  <li>
   O estado é acessado com o Store, um observable de estado e um observer de ações;
  </li>
</ul>

<h3>
  Diagrama
</h3>

<h5>
  O diagrama a seguir representa no geral ums visão geral do fluxo de estado de uma
  aplicação no NgRx.
</h5>

<div align="center">
  <img width="90%" src="https://user-images.githubusercontent.com/61476935/115942501-fbab2a80-a480-11eb-8386-ff31ff33f434.png">
</div>
