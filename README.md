<div align="center">
  <img src="https://user-images.githubusercontent.com/61476935/115932683-c3e1ba00-a463-11eb-94c8-a906fcd5ad8b.png">
</div>
<br>     
<img src="https://img.shields.io/static/v1?label=Angular&message=Library&color=purple&style=for-the-badge&logo=Angular"/>

<h1>
  Store
</h1>

  Store é o gerenciador de estados globais do RxJS para aplicações Angular. Inspirado pelo Redux, comum a
  desenvolvedores React, é um container de controle de estados desenvolvido para ajudar na criação de
  aplicações consistentes e performáticas sobre a estrutura do Angular.

<h2>
  Conceitos Chave
</h2>

<ul>
  <li>
   Ações descrevem eventos únicos que são despachados dos componentes e services;
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

<h2>
  Diagrama
</h2>

<div align="center">
   O diagrama a seguir representa no geral uma visão do fluxo de estado de uma aplicação no NgRx.
</div>

<div align="center">
  <img width="80%" src="https://user-images.githubusercontent.com/61476935/115942501-fbab2a80-a480-11eb-8386-ff31ff33f434.png">
</div>

<h1>
  Arquitetura
</h1>

<h2>
  Action
</h2>

Ações são um dois pilares de sustentação do NgRx. Ações expressam eventos únicos que acontecem por tada
a aplicação. De interações do usuário, interações externas no processo de network requests e interações
diretas com Api's, todos estes eventos e outros podem ter seu início com uma action ou definirem uma.

<h2>
  A Interface das Actions
</h2>

    interface Action {
      type: string;
    }

A interface de uma action possui o type como sua única propriedade, sendo esta representada por uma string,
e tendo a função de descrever a action que será despachada. O valor de um type se dá através de um "[Fonte]
Evento" e é usado para dizer a que tipo essa action se associa e de onde a action foi despachada. Também é
possível adicionar propriedades a uma action, provendo contextos ou metadata adicionais.

Abaixo há alguns exemplos de actions resultantes de eventos:

     {
        type: '[Auth API] Login Success'
     }

<h5>
  Essa ação descreve o disparo de um evento através de uma autenticação bem sucedida após a interação com 
  uma API;
</h5>

     {
       type: '[Login Page] Login',
       username: string;
       password: string;
     }

<h5>
   Esse caso descreve um evento disparado por um clique no botão de login, autenticando um usuário. O nome do
   usuário e sua senha são definidos como dados adicionais providos pela página de login;
</h5> 

<h2>
  Escrevendo Actions
</h2>

Há alguns pontos a se ater quando se define e escreve actions, permitindo um melhor aproveitamento de suas funções:

<ul>
 <li>
   Preparo - Escreva actions antes de definir as características de suas fontes(componentes, services, etc). Essa 
   prevenção torna possível entender melhor as características dos componentes ou services sendo implementados;
 </li>
 <li>
   Divisão - Categorize actions se baseando nos eventos fonte;
 </li>
 <li>
   Quantidade - A definição de actions não é custosa, então quanto mais actions são escritas, melhor expressos serão
   os fluxos da sua aplicação;
 </li>
 <li>
   Impulso - Capture eventos, <strong>não comandos</strong>, pois você está separando a descrição de um evento e seu
   tratamento;
 </li>
 <li>
   Descrição - Garanta que o contexto direcionado para seus eventos seja mais detalhado, essas informações podem ser 
   úteis no processo;
 </li>
</ul>

<h5>Abaixo há um exemplo de action de iniciação de resquest de login</h5>

    import { createAction, props } from '@ngrx/store';
   
    export const login = createAction(
      '[Login Page] Login',
      props<{ username: string; password: string }>()
    );

O método <strong>createAction</strong> retorna uma função que, quando chamada, retorna um objeto no formato da interface
de uma action. Já o método <strong>props</strong> é usado para definir qualquer dado adicional que seja necessário para 
lidar com a action. Action creators promovem um meio consistente e seguro de construir as actions que serão despachadas.

    onSubmit(username: string, password: string) {
      store.dispatch(login({ username: username, password: password }));
    }

No caso acima o action creator <strong>login</strong> recebe um objeto de <strong>username</strong> e <strong>password</strong>,
retornando um objeto simples com uma propriedade <strong>type</strong> de <strong>[Login Page] Login</strong>, com 
<strong>username</strong> e <strong>password</strong> como propriedades adicionais.
