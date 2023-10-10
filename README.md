<!-- PROJECT LOGO -->
<div align="center">
   <a href="https://github.com/othneildrew/Best-README-Template">
      <img src="https://logodownload.org/wp-content/uploads/2014/12/estacio-logo-1-2048x1641.png" alt="estacio logo" width="80"                  height="80">
   </a>
    <h1 align="center"> Universidade Estácio de Sá </h1>
     <hr>
</div> 

* DESENVOLVIMENTO FULL STACK- TURMA 23.3 -9003
* Disciplina: RPG0018  - Por que não paralelizar.
* Semestre Letivo: 2023.2
* Repositorio Git: https://github.com/Gregdev22/Missao-5-Mundo-3-

<hr>

* [EMERSON GREGORIO ALVES](https://github.com/Gregdev22) - MATRICULA: 2022.0908.4986
<hr>
 <h1 align="center"> Missão Prática | Nível 5 | Mundo 3 </h1>
 <h2 align="left" > IServidores e clientes baseados em Socket, com uso de Threads tanto no lado cliente quanto no lado servidor, acessando o banco de dados via JPA. </h2> 
 <h3>Procedimento 1: Criando o Servidor e Cliente de Teste </h3>
 <h3>Procedimento 2: Servidor Completo e Cliente Assíncrono </h3>
 <hr>

 <h2> :clipboard: Objetivos da Prática </h2>

* Criar servidores Java com base em Sockets.
* Criar clientes síncronos para servidores com base em Sockets.
* Criar clientes assíncronos para servidores com base em Sockets.
* Utilizar Threads para implementação de processos paralelos.
* No final do exercício, o aluno terá criado um servidor Java baseado em Socket, com acesso ao banco de dados via JPA, além de utilizar os recursos nativos do Java para
implementação de clientes síncronos e assíncronos. As Threads serão usadas tanto no servidor, para viabilizar múltiplos clientes paralelos, quanto no cliente, para implementar a resposta assíncrona.
<hr>

<h2> Códigos </h2>

[Procedimento 1: Criando o Servidor e Cliente de Teste](https://github.com/Gregdev22/Missao-5-Mundo-3-/tree/main/Procedimento%201)

* Classe JAVA AQUI
  ``` java
  ```
  <br>

[Procedimento 2: Servidor Completo e Cliente Assíncrono](https://github.com/Gregdev22/Missao-5-Mundo-3-/tree/main/Procedimento%202)

* Classe JAVA AQUI
  ``` java
  ```
  <br>
  <hr>
<h1>Resultados: </h1>
:triangular_flag_on_post: Procedimento 1: https://github.com/Gregdev22/Missao-5-Mundo-3-/tree/main/Procedimento%201
<img src="https://github.com/Gregdev22/Missao-5-Mundo-3-/blob/main/Procedimento%201/Resultados%20missao%205/1%20proc1%20missao%205.png" alt="resultado 1" width="640" height="360">
<img src="https://github.com/Gregdev22/Missao-5-Mundo-3-/blob/main/Procedimento%201/Resultados%20missao%205/2%20proc%201%20missao%205.png" alt="resultado 2" width="640" height="360">

<br>
:triangular_flag_on_post: Procedimento 2: (https://github.com/Gregdev22/Missao-5-Mundo-3-/tree/main/Procedimento%202)
<img src="https://github.com/Gregdev22/Missao-5-Mundo-3-/blob/main/Procedimento%202/Resultados%20missao%205/2%20proc%202%20missao%205.png" alt="resultado 1" width="640" height="360">
<img src="https://github.com/Gregdev22/Missao-5-Mundo-3-/blob/main/Procedimento%202/Resultados%20missao%205/3%20proc%202%20missao%205.png" alt="resultado 2" width="640" height="640">

<hr>

<h1>Análise e Conclusão</h1>
<ul>
   <li>
      Como é organizado um projeto corporativo no NetBeans?  
      <p> 
        <h4> O projeto corporativo é dividido em um módulo EJB , onde ficam localizadas as classes relativas aos modelos e controles, e um módulo Web, onde ficam os arquivos JSP, HTML e Servlet. 
        </h4>         
      </p>
   </li>
             
   <li>
     Qual o papel das tecnologias JPA e EJB na construção de um aplicativo para a plataforma Web no ambiente Java?
      <p> 
         JPA --> Java Persistence API é responsável por gerenciar a interação com o banco de dados de maneira abstrata. Utilizando JPA, a persistência de dados é executada de maneira orientada a objetos. 
         EJB --> Enterprise JavaBeans contém a lógica de negócio que atua sobre os dados de negócio.  
      </p>
   </li>
   
   <li>
      Como o NetBeans viabiliza a melhoria de produtividade ao lidar com as tecnologias JPA e EJB?
      <p> 
          O desenvolvedor pode programar usando apenas conceitos de orientação a objetos, sem se preocupar com conceitos relacionais – como tabelas – ou detalhes do banco de dados.
      </p>
   </li>
   
   <li>
     O que são Servlets, e como o NetBeans oferece suporte à construção desse tipo de componentes em um projeto Web? 
      <p> 
       Servlet (mini-servidor) é um objeto Java que recebe requisições do cliente (request) e produz algo (response), como uma página HTML dinamicamente gerada. 
      </p>
   </li>

   <li>
     Como funciona o padrão Front Controller, e como ele é implementado em um aplicativo Web Java, na arquitetura MVC?
      <p> 
       É um padrão arquitetural que se comporta como um controlador tratando todas as solicitações para um site Web e então roteia para uma ação (ou comando). o Front Controller trata todas as chamadas vindas de um site web e é organizado em duas partes: através de um Manipulador Web e uma hierarquia de Comandos. O Manipulador Web é o objeto que efetivamente recebe as solicitações HTTP do tipo POST ou GET do servidor web. Ele extrai as informações necessárias da URL e das solicitações e então decide que tipo de ação iniciar e por fim delega a um objeto Comando para executar a ação. 
      </p>
   </li>
   <li>
      Quais as diferenças e semelhanças entre Servlets e JSPs?
      <p> 
        Ambos possuem uma grande portabilidade, facilidade de programação, flexibilidade e escalabilidade. Em uma página JSP a formatação HTML se encontra separada da programação, podendo ser modificada sem afetar a aplicação de modo mais profundo. Enquanto no Servlet o resultado de uma requisição se mistura lógica de aplicação. 
      </p>
   </li>
    <li>
     Qual a diferença entre um redirecionamento simples e o uso do método forward, a partir do RequestDispatcher? Para que servem parâmetros e atributos nos objetos HttpRequest?
      <p> 
        Redirect redireciona o cliente para uma página (sendRedirect) e RequestDispatcher encaminha uma requisição para ser atendida por outro recurso (forward). No primeiro caso (sendRedirect), o cliente receberá uma resposta http em cujo header haverá a informação de que ele deve requisitar outra página, e o browser fará esta requisição. Ou seja, o redirecionamento ocorre no lado no cliente. No segundo caso (forward), no lado do server a requisição do usuário será encaminhada para ser atendida por outro recurso (outro servlet). Este outro servlet eventualmente devolverá outra página para o usuário.  

Os parâmetros são as informações da página submetidos por um formulário, por exemplo. Nesse caso, cada controle de entrada de dados é um parâmetro, e todos eles pertencem à requisição HTTP. Se você submeter dados através do método GET, os parâmetros ficam visíveis na URL no seguinte formato: url?param1=valor1&param2=valor2 etc. 
Já os atributos são objetos associados a nomes - uma espécie de tabela onde a chave é uma string - que ficam guardados no servidor, associados a um determinado escopo que pode ser de página ( PageContext ), requisição ( HttpServletRequest ), sessão ( HttpSession ) ou aplicação ( ServletContext ). 
      </p>
   </li>
       <li>
       Como o framework Bootstrap é utilizado?
      <p> 
        o Bootstrap é um framework CSS para ser utilizado no front-end de aplicações web. Ele utiliza JavaScript e CSS para estilizar as páginas e adicionar funcionalidades atráves de classes específicas adicionadas as tags de um documento html. 
      </p>
   </li>
       <li>
       Por que o Bootstrap garante a independência estrutural do HTML?
      <p> 
        O Bootstrap apenas altera o estilo da página e adiciona funcionalidades.
      </p>
   </li>
       <li>
     Qual a relação entre o Boostrap e a responsividade da página?
      <p> 
        O uso do Bootstrap visa a possibilidade de acessar o site em qualquer dispositivo, pois ser responsivo é uma de suas principais características. Basicamente, ele trabalha com um sistema de grid que divide a tela em 12 colunas, que facilita a acomodação dos elementos conforme o tamanho da tela do dispositivo. 
      </p>
   </li>
</ul>
