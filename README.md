analysis-movermercado
=====================

project for PSI


CREATE TABLE `PSI`.`cadastro` (

`cod` INT( 255 ) NOT NULL AUTO_INCREMENT PRIMARY KEY ,
`nome` VARCHAR( 255 ) NOT NULL ,
`dt_nasc` VARCHAR( 255 ) NOT NULL ,
`telefone` VARCHAR( 255 ) NOT NULL ,
`email` VARCHAR( 255 ) NOT NULL
) ENGINE = MYISAM
  		

Ready now we can worry about Java classes:)
Let's create a Customer class with the same fields we put in the database.

cod
public class custumer {
    private int cod;
    private String nome;
    private String dt_nasc;
    private String telefone;
    private String email;

    public int getCod() {
        return cod;
    }

    public void setCod(int cod) {
        this.cod = cod;
    }

    public String getDt_nasc() {
        return dt_nasc;
    }

    public void setDt_nasc(String dt_nasc) {
        this.dt_nasc = dt_nasc;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getNome() {
        return nome;
    }

    public void setNome(String nome) {
        this.nome = nome;
    }

    public String getTelefone() {
        return telefone;
    }

    public void setTelefone(String telefone) {
        this.telefone = telefone;
    }

}

Ready to Friend class is created, we need to create a class to talk to our database. The class that we create now a search and insertion in the database.


import java.util.*;
import java.sql.*;
import java.util.ArrayList;

public class ConectaBanco {

    private String url;
    private String login;
    private String senha;

    public ConectaBanco(String url, String login, String senha) {
        setUrl(url);
        setLogin(login);
        setSenha(senha);
    }

    public String getLogin() {
        return login;
    }

    public void setLogin(String login) {
        this.login = login;
    }

    public String getSenha() {
        return senha;
    }

    public void setSenha(String senha) {
        this.senha = senha;
    }

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public void insert(String s, String msg) {
        try {
            Class.forName("com.mysql.jdbc.Driver").newInstance();
            //System.out.println("\n Salvando URL: ...\n");
            try {
                Connection conn = DriverManager.getConnection(getUrl(),
getLogin(), getSenha());
                try {
                    String sql = s;
                    Statement stm = conn.createStatement();
                    try {
                        stm.executeUpdate(sql);
                        System.out.println(msg);
                    } catch (Exception ex) {
                        System.out.println("\nErro no resultset!\n" + ex);
                    }
                } catch (Exception ex) {
                    System.out.println("\nErro no statement!");
                }
            } catch (Exception ex) {
                System.out.println("\nErro no connection!");
            }
        } catch (Exception ex) {
            System.out.println("\nDriver nao pode ser carregado!");
        }

    }

    public ArrayList busca(String s) {
        ArrayList amigo = new ArrayList();
        Amigo a = new custumer();
        try {
            Class.forName("com.mysql.jdbc.Driver").newInstance();
            //System.out.println("\n Salvando URL: ...\n");
            try {
                Connection conn = DriverManager.getConnection(getUrl(),
getLogin(), getSenha());
                try {
                    String sql = s;
                    Statement stm = conn.createStatement();
                    try {
                        ResultSet rs = stm.executeQuery(sql);
                        while (rs.next()) {
                            a.setCod(rs.getInt(1));
                            a.setNome(rs.getString(2));
                            a.setDt_nasc(rs.getString(3));
                            a.setTelefone(rs.getString(4));
                            a.setEmail(rs.getString(5));
                            custumer.add(a);
                        }
                        //System.out.println(rs.getInt(1));
                    } catch (Exception ex) {
                        System.out.println(ex);
                    }
                } catch (Exception ex) {
                    System.out.println("\nErro no statement!");
                }
            } catch (Exception ex) {
                System.out.println("\nErro no connection! " + ex);
            }
        } catch (Exception ex) {
            System.out.println("\nDriver nao pode ser carregado!");
        }
        return custumer;
    }
}

Now let's do the MAIN ...
In MAIN let's do a simple thing just register and pick up a friend in the database. 

import java.util.ArrayList;
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        // TODO code application loSgic here
        Scanner ler  = new Scanner(System.in);
        ConectaBanco cb = new ConectaBanco("jdbc:mysql://URL",
 "LOGIN", "SENHA");

        int x=-1;
        while (x != 0) {
            System.out.println("Escolha uma opcao");
            System.out.println("1 - Cadastrar custumer");
            System.out.println("2 - Buscar Custumer");
            System.out.println("3 - Sair");
            x = Integer.parseInt(ler.nextLine());

            switch (x) {
                case 1: {
                    Amigo amigo = new Custumer();

                    System.out.println("Digite um nome:");
                    amigo.setNome(ler.nextLine());
                    System.out.println("Digite a data de nascimento:");
                    amigo.setDt_nasc(ler.nextLine());
                    System.out.println("Digite o Telefone:");
                    amigo.setTelefone(ler.nextLine());
                    System.out.println("Digite o E-mail:");
                    amigo.setEmail(ler.nextLine());

                    cb.insere("INSERT INTO
nome_do_banco_de_dados.nome_da_tabela VALUES
(NULL , '" + custumer.getNome() + "', '" + custumer.getDt_nasc() +
 "', '" + amigo.getTelefone() + "', '" + amigo.getEmail() + "');
", "Amigo gravado corretamente...");
                    break;
                }
                case 2: {
                    ArrayList a = new ArrayList();
                    System.out.println("Digite o nome do seu custumer
para procura:");
                    String nome = ler.nextLine();
                    a =  cb.busca("SELECT * FROM
nome_do_banco_de_dados.nome_da_tabelao WHERE nome
LIKE '%" + nome + "%';");
                    if (a.size()>0){
                        a.get(0).show(a);
                    }
                    else{
                        System.out.println("nao achámos seu custumer    ");
                    }

                    break;
                }
                case 3: {
                    x=0;
                }
            }

        }
    }

}

