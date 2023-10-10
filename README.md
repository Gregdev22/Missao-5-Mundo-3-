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

* CadastroClient.java
  
``` java
  /*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Main.java to edit this template
 */
package cadastroclient;

import java.io.IOException;
import java.io.InputStreamReader;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.PrintStream;
import java.net.Socket;
import java.util.List;
import java.util.Scanner;
import model.Produto;

/**
 *
 * @author grego
 */
public class CadastroClient {

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args)throws ClassNotFoundException, IOException {
        Socket socket = new Socket("localhost", 4321);
        ObjectOutputStream out = new ObjectOutputStream(socket.getOutputStream());
        ObjectInputStream in = new ObjectInputStream(socket.getInputStream());
        
        out.writeObject("op1");
        
        out.writeObject("op1");
        
        System.out.println((String)in.readObject());

        out.writeObject("L");

        List<Produto> produtos = (List<Produto>) in.readObject();
        for (Produto produto : produtos) {
            System.out.println(produto.getNome());
        }

        out.close();
        in.close();
        socket.close();
    }
    
}

```
* CadastroServer.java

```java
/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Main.java to edit this template
 */
package cadastroserver;
import controller.ProdutoJpaController;
import controller.UsuarioJpaController;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintStream;
import java.net.ServerSocket;
import java.net.Socket;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;


/**
 *
 * @author grego
 */
public class CadastroServer {

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) throws IOException{
        ServerSocket serverSocket = new ServerSocket(4321);
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("CadastroServerPU");
        ProdutoJpaController ctrl = new ProdutoJpaController(emf);
        UsuarioJpaController ctrlUsu = new UsuarioJpaController(emf);
        
        while (true) {

            Socket clienteSocket = serverSocket.accept();
            System.out.println("Cliente conectado: " + clienteSocket.getInetAddress());

            CadastroThread thread = new CadastroThread(ctrl, ctrlUsu, clienteSocket);
            
            thread.start();
            System.out.println("Aguardando nova conexÃ£o...");
        }
        
        }
    } 
```
* CadastroThread.java
  
```java
/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Class.java to edit this template
 */
package cadastroserver;

import controller.ProdutoJpaController;
import controller.UsuarioJpaController;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.net.Socket;
import java.util.Scanner;
import model.Usuario;

/**
 *
 * @author grego
 */
public class CadastroThread extends Thread {
    
    private ProdutoJpaController ctrl;
    private UsuarioJpaController ctrlUsu;
    private Socket s1;
    private ObjectOutputStream out;
    private ObjectInputStream in;
    

    CadastroThread  (ProdutoJpaController ctrl, UsuarioJpaController ctrlUsu, Socket s1) {
	this.ctrl = ctrl;
        this.ctrlUsu = ctrlUsu;
        this.s1 = s1;
	}
    
    
    @Override
    public void run(){
      
        String login = "";
        
        try{
            out = new ObjectOutputStream(s1.getOutputStream());
            in = new ObjectInputStream(s1.getInputStream());
            
            System.out.println("Cliente conectado.");

            login = (String) in.readObject();
            String senha = (String) in.readObject();

            Usuario usuario = ctrlUsu.findUsuario(login, senha);
            if (usuario == null) {
                System.out.println("UsuÃ¡rio invÃ¡lido."); //Login="+ login +", Senha="+ senha
                out.writeObject("UsuÃ¡rio invÃ¡lido.");
                return;
            }
            
            System.out.println("UsuÃ¡rio conectado.");
            out.writeObject("UsuÃ¡rio conectado.");

            System.out.println("Aguardando comandos...");
            String comando = (String) in.readObject();
            
            if (comando.equals("L")) {
                System.out.println("Listando produtos.");
                out.writeObject(ctrl.findProdutoEntities());
            }           
        }catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            close();
            System.out.println("ConexÃ£o finalizada.");
        }
        
    }
    
    private void close() {
        try {
            if (out != null) {
                out.close();
            }
            if (in != null) {
                in.close();
            }
            if (s1 != null) {
                s1.close();
            }
        } catch (IOException ex) {
            System.out.println("Falha ao finalizar conexÃ£o.");
        }
    }
}

```

[Procedimento 2: Servidor Completo e Cliente Assíncrono](https://github.com/Gregdev22/Missao-5-Mundo-3-/tree/main/Procedimento%202)

* CadastroClientv2.java
``` java
/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Main.java to edit this template
 */
package cadastroclient;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.PrintStream;
import java.net.Socket;
import java.util.List;
import java.util.Scanner;
import model.Produto;

/**
 *
 * @author grego
 */


public class CadastroClientv2 {   
    
    private static ObjectOutputStream socketOut;
    private static ObjectInputStream socketIn;
    private static ThreadClient threadClient;
    
    
    /**
     * @param args the command line arguments
     */
    
    
    public static void main(String[] args)throws ClassNotFoundException, IOException {
        Socket socket = new Socket("localhost", 4321);
        socketOut = new ObjectOutputStream(socket.getOutputStream());
        socketIn = new ObjectInputStream(socket.getInputStream());
        
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
        
        SaidaFrame saidaFrame = new SaidaFrame();
        saidaFrame.setVisible(true);
        
        threadClient = new ThreadClient(socketIn, saidaFrame.texto);
        threadClient.start();
        
        socketOut.writeObject("op1");
        
        socketOut.writeObject("op1");

        Character commando = ' ';
        try {
            while (!commando.equals('X')) {
                System.out.println("Escolha uma opÃ§Ã£o:");
                System.out.println("L - Listar | X - Finalizar | E - Entrada | S - SaÃ­da");

                commando = reader.readLine().charAt(0);

                processaComando(reader, commando);
            }
        } catch(Exception e) {
            e.printStackTrace();
        } finally {
            saidaFrame.dispose();
            socketOut.close();
            socketIn.close();
            socket.close();
            reader.close();
        }
    }

    static void processaComando(BufferedReader reader, Character commando) throws IOException {
        socketOut.writeChar(commando);
        socketOut.flush();
                
        switch (commando) {
            case 'L':
                break;
            case 'S':
            case 'E':
                socketOut.flush();

                System.out.println("Digite o Id da pessoa:");
                int idPessoa = Integer.parseInt(reader.readLine());
                System.out.println("Digite o Id do produto:");
                int idProduto = Integer.parseInt(reader.readLine());
                System.out.println("Digite a quantidade:");
                int quantidade = Integer.parseInt(reader.readLine());
                System.out.println("Digite o valor unitÃ¡rio:");
                long valorUnitario = Long.parseLong(reader.readLine());

                socketOut.writeInt(idPessoa);
                socketOut.flush();
                socketOut.writeInt(idProduto);
                socketOut.flush();
                socketOut.writeInt(quantidade);
                socketOut.flush();
                socketOut.writeLong(valorUnitario);
                socketOut.flush();
                break;
            case 'X':
                threadClient.cancela(); 
                break;
            default:
                System.out.println("OpÃ§Ã£o invÃ¡lida!");
        }
    }
    
}

```

* SaidaFrame 
``` java
/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Class.java to edit this template
 */
package cadastroclient;
import javax.swing.*;
/**
 *
 * @author grego
 */
public class SaidaFrame extends JDialog {
    public JTextArea texto;

    public SaidaFrame() {
        setBounds(100, 100, 400, 300);

        setModal(false);

        texto = new JTextArea(25, 40);
        texto.setEditable(false); // Bloqueia ediÃ§Ã£o do campo de texto
        
        JScrollPane scroll = new JScrollPane(texto);
        scroll.setHorizontalScrollBarPolicy(ScrollPaneConstants.HORIZONTAL_SCROLLBAR_NEVER); // Bloqueia rolagem horizontal
        add(scroll);
    }
}
```

* ThreadClient.java 
``` java
/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Class.java to edit this template
 */
package cadastroclient;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.net.SocketException;
import java.util.List;
import javax.swing.JTextArea;
import javax.swing.SwingUtilities;
import model.Produto;

/**
 *
 * @author grego
 */
public class ThreadClient extends Thread {
    private ObjectInputStream entrada;
    private JTextArea textArea;
    private Boolean cancelada;

    public ThreadClient(ObjectInputStream entrada, JTextArea textArea) {
        this.entrada = entrada;
        this.textArea = textArea;
        this.cancelada = false;
    }

    @Override
    public void run() {
        while (!cancelada) {
            try {
                Object resposta = entrada.readObject();
                SwingUtilities.invokeLater(() -> {
                    processaResposta(resposta);
                });
            } catch (IOException | ClassNotFoundException e) {
                if (!cancelada) {
                    System.err.println(e);
                }
            }
        }
    }
    
    public void cancela() {
        cancelada = true;
    }
    
    private void processaResposta(Object resposta) {
        textArea.append(">> Nova comunicaÃ§Ã£o em " + java.time.LocalTime.now() + ":\n");
        
        if (resposta instanceof String) {
            textArea.append((String) resposta + "\n");
        } else if (resposta instanceof List<?>) {
            textArea.append("> Listagem dos produtos:\n");
            List<Produto> lista = (List<Produto>) resposta;
            for (Produto item : lista) {
                textArea.append("Produto=[" + item.getNome() + "], Quantidade=["+ item.getQuantidade() + "]\n");
            }
        }
        textArea.append("\n");
        textArea.setCaretPosition(textArea.getDocument().getLength());
    }
}

```

* CadastroServer.java
``` java
/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Main.java to edit this template
 */
package cadastroserver;
import controller.MovimentoJpaController;
import controller.PessoaJpaController;
import controller.ProdutoJpaController;
import controller.UsuarioJpaController;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintStream;
import java.net.ServerSocket;
import java.net.Socket;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;


/**
 *
 * @author grego
 */
public class CadastroServer {

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) throws IOException{
        ServerSocket serverSocket = new ServerSocket(4321);
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("CadastroServerPU");
        ProdutoJpaController ctrl = new ProdutoJpaController(emf);
        UsuarioJpaController ctrlUsu = new UsuarioJpaController(emf);
        MovimentoJpaController ctrlMov = new MovimentoJpaController(emf);
        PessoaJpaController ctrlPessoa = new PessoaJpaController(emf);
        
        while (true) {
            Socket clienteSocket = serverSocket.accept();
            System.out.println("Cliente conectado: ");

            CadastroThreadv2 thread = new CadastroThreadv2(ctrl, ctrlUsu, ctrlMov, ctrlPessoa, clienteSocket);
            
            thread.start();
            System.out.println("Aguardando nova conexÃ£o...");
        }
        
        }
    } 

```
* CadastroThreadv2.java
``` java
/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Class.java to edit this template
 */
package cadastroserver;

import controller.MovimentoJpaController;
import controller.PessoaJpaController;
import controller.ProdutoJpaController;
import controller.UsuarioJpaController;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.net.Socket;
import java.util.Scanner;
import java.util.logging.Level;
import java.util.logging.Logger;
import model.Movimento;
import model.Produto;
import model.Usuario;

/**
 *
 * @author grego
 */
public class CadastroThreadv2 extends Thread {
    
    private ProdutoJpaController ctrl;
    private UsuarioJpaController ctrlUsu;
    private MovimentoJpaController ctrlMov;
    private PessoaJpaController ctrlPessoa;
    private Socket s1;
    private ObjectOutputStream out;
    private ObjectInputStream in;
    private Usuario usuario;
    private Boolean continuaProcesso = true;

    CadastroThreadv2  (ProdutoJpaController ctrl, UsuarioJpaController ctrlUsu, MovimentoJpaController ctrlMov, PessoaJpaController ctrlPessoa, Socket s1) {
	this.ctrl = ctrl;
        this.ctrlUsu = ctrlUsu;
        this.ctrlMov = ctrlMov;
        this.ctrlPessoa = ctrlPessoa;
        this.s1 = s1;
	}
    
    
    @Override
    public void run(){
      
        String login = "";
        
        try{
            out = new ObjectOutputStream(s1.getOutputStream());
            in = new ObjectInputStream(s1.getInputStream());
            
            System.out.println("Cliente conectado.");

            login = (String) in.readObject();
            String senha = (String) in.readObject();
            usuario = ctrlUsu.findUsuario(login, senha);

            if (usuario == null) {
                System.out.println("UsuÃ¡rio invÃ¡lido.");
                out.writeObject("UsuÃ¡rio invÃ¡lido.");
                return;
            }
            
            System.out.println("UsuÃ¡rio conectado.");
            out.writeObject("UsuÃ¡rio conectado.");
            out.flush();
       
            
            while (continuaProcesso) {
                continuaProcesso = processaComando();
            }

        }catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        } catch (Exception ex) {
            Logger.getLogger(CadastroThreadv2.class.getName()).log(Level.SEVERE, null, ex);
        } finally {
            close();
            System.out.println("ConexÃ£o finalizada.");
        }
        
    }
    

    private Boolean processaComando() throws Exception {
        System.out.println("Aguardando comandos...");
        Character comando = in.readChar();

        switch (comando) {
            case 'L':
                System.out.println("Comando recebido, listando produtos.");
                out.writeObject(ctrl.findProdutoEntities());
                continuaProcesso = true;
                return true;
            case 'E':
                continuaProcesso = true;
                return true;
            case 'S':
                System.out.println("Comando Movimento tipo ["+ comando +"] recebido.");
                int idPessoa = in.readInt();
                int idProduto = in.readInt();
                int quantidade = in.readInt();
                Float valorUnitario = in.readFloat();

                Produto produto = ctrl.findProduto(idProduto);
                if (produto == null) {
                    out.writeObject("Produto invÃ¡lido.");
                    continuaProcesso = true;
                    return true;
                }
                
                if (comando.equals('E')) {
                    produto.setQuantidade(produto.getQuantidade() + quantidade);
                    continuaProcesso = true;
                    return true;
                } else if (comando.equals('S')) {
                    produto.setQuantidade(produto.getQuantidade() - quantidade);
                    continuaProcesso = true;
                    return true;
                }

                ctrl.edit(produto);

                Movimento movimento = new Movimento();
                movimento.setTipo(comando);
                movimento.setUsuarioidUsuario(usuario);
                movimento.setPessoaIdpessoa(ctrlPessoa.findPessoa(idPessoa));
                movimento.setProdutoIdproduto(produto);
                movimento.setQuantidade(quantidade);
                movimento.setValorUnitario(valorUnitario);

                ctrlMov.create(movimento);
                out.writeObject("Movimento registrado com sucesso.");
                out.flush();
                System.out.println("Movimento registrado com sucesso.");
                continuaProcesso = true;
                return true;
            case 'X':
                continuaProcesso = false;
                return false;
            default:
                System.out.println("OpÃ§Ã£o invÃ¡lida!");
                continuaProcesso = false;
                return true;
        }
        
    }
    
    private void close() {
        try {
            if (out != null) {
                out.close();
            }
            if (in != null) {
                in.close();
            }
            if (s1 != null) {
                s1.close();
            }
        } catch (IOException ex) {
            System.out.println("Falha ao fechar conexÃ£o.");
        }
    }
}

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
