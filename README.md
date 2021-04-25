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
  Store
</h2>

O Store é o ambiente que matém agrupadas todas as informações definidas como estados de uma aplicação. 
A partir dele é possível acessar e definir novos estados e alocá-los no lugar dos anteriores de forma
não definitiva. Abaixo temos um exemplo da declaração dos estados de uma aplicação:


     export interface AppState {
     
        books: BookList[];
        authors: AuthorList[];
        subjects: subjects[];
        
     }

<h2>
  Action
</h2>

Ações são um dos pilares de sustentação do NgRx. Ações expressam eventos únicos que acontecem por tada
a aplicação. De interações do usuário, interações externas no processo de network requests e interações
diretas com Api's, todos estes eventos e outros podem ter seu início com uma action ou definirem uma. 
Um ponto importante a se manter em mente é que as Actions não geram a mudança de estado de forma direta,
mas sim os reducers, que serão explicados mais a frente.

<h2>
  A Interface das Actions
</h2>

    interface Action {
      type: string;
    }

A interface de uma action possui o type como sua única propriedade, sendo esta representada por uma string,
e tendo a função de representar a action que será despachada. O valor de um type se dá através de um "[Fonte]
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

<h2>
  Declarando uma Action
</h2>

O ngrx/store disponibiliza uma série de módulos para definição de actions em projetos cuja intenção é ser reativo.
Um deles é o <strong>Action</strong>, que pode ser declarado em uma classe, a definindo como uma nova action. Esta
forma de definição de actions é comumente utilizada na comunidade de desenvolvedores Angular:
    
    import { Action } from '@ngrx/store';
    import { NewBook } from '...';

    export enum BooksActionsTypes{
        BOOKS_ALL = '[BOOKS_ALL] Obtem todos os livros',
        BOOKS_NEW = '[BOOKS_NEW] Criar novo livro',
    }

    export class BookAll implements Action {
        readonly type = BooksActionsTypes.BOOKS_ALL;
    }
    
    export class BookNew implements Action {
        readonly type = BooksActionsTypes.BOOKS_NEW;
        constructor(public payload: {book: NewBook}){}
    }

    export type BookActions = BookAll | BookNew;
   
Uma descrição bastante simplória dessa forma de uso é que três classes são criadas e exportadas; Uma definindo as ações e
seus tipos; E outra definindo o comportamento da action, incluíndo valores adicionais que seriam importantes no processo de
definição do novo estado dentro do reducer, sendo utilizado um constructor e um payload para tal. Em caso de não ser preciso
ter acesso a esses valores, o constructor é descartado, sendo ambos os casos vistos acima. Uma terceira classe agrupa os tipos
para que estes sejam acessados no reducer.


<h2>Reducers</h2>

Os Reducers no NgRx são os responsáveis por lidar com a transição de um estado para o outro em uma aplicação. As funções de um 
Reducer tratam estas transições determinando qual action tratar baseando-se em seu tipo, e com isso definindo a mudança no state.

<h2>A Função de um Reducer</h2>

É responsabilidade dos Reducers gerenciar certos pontos importantes de cada novo estado:

<ul>
 <li>
   Uma interface ou tipo que define a forma do estado;
 </li>
 <li>
   Os argumentos, incluíndo o estado inicial ou o estado e a ação mais recentes;
 </li>
 <li>
   A função que lida com as mudanças de estado e suas ações associadas;
 </li>
</ul>

Uma forma de definição de Reducers que a comunidade de desenvolvedores Angular comumente usa é a seguinte:

    import * as fromBookActions from './pix.actions';
    import { Book } from '...';
    
    export const initialState: Book = new Book();

    export function reducer( state = initialState, action: fromBookActions.BookActions ) {

      switch(action.type) {
          case fromBookActions.BooksActionsTypes.BOOKS_ALL:
            return state;

          case fromBookActions.BooksActionsTypes.BOOKS_NEW:
            state = null;
            state = action.payload;
            return state;
            
          default: 
            return state;
     }
    }
  
Nesse caso é bastante importante entender alguns padrões, estes são: 

<h3>Definição do Estado Inicial</h3>

O estado inial define um valor padrão, ou de um tipo padão, sendo este base para a definição
de um novo estado. Lembrando que um novo state é definido a partir do estado anterior e do
payload definido na action correspondente:

> import { Book } from '...';
> 
> export const initialState: Book = new Book();

<h3>Acesso as Actions</h3>

Tendo em mente que a função dos reducers é definir um novo estado a partir do disparo de uma 
ação e do seu tipo, é importante ter definidas as ações para comparação:

>import * as fromBookActions from './pix.actions';
>
>export function reducer( ... action: fromBookActions.BookActions ) {...}

<h3>Definir Novos States</h3>

A definição de um novo estado é determinada pelo tipo de action detectada, nesse caso, uma condicional
de comparação é usada para definir tipos distintos de retorno para cada caso:

>      switch(action.type) {
>          case fromBookActions.BooksActionsTypes.BOOKS_ALL:
>            return state;
>
>          case fromBookActions.BooksActionsTypes.BOOKS_NEW:
>            state = null;
>            state = action.payload;
>            return state;
>            
>          default: 
>            return state;
>     }


