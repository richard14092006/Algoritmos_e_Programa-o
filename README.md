Main.java


import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        GerenciadorLocadora locadora = new GerenciadorLocadora();

        int opcao = 0;
        while (opcao != 7) {
            System.out.println("\n===== MENU LOCADORA =====");
            System.out.println("1. Cadastrar nova mídia");
            System.out.println("2. Listar mídias");
            System.out.println("3. Realizar locação");
            System.out.println("4. Devolver mídia");
            System.out.println("5. Listar histórico de locações");
            System.out.println("6. Mostrar total arrecadado");
            System.out.println("7. Sair");
            System.out.print("Escolha uma opção: ");
            opcao = sc.nextInt();
            sc.nextLine(); 

            switch (opcao) {
                case 1:
                    System.out.print("Tipo de mídia (1-VHS, 2-DVD, 3-Streaming): ");
                    int tipo = sc.nextInt();
                    sc.nextLine();
                    System.out.print("Título: ");
                    String titulo = sc.nextLine();
                    System.out.print("Preço base: ");
                    double preco = sc.nextDouble();
                    sc.nextLine();

                    Midia novaMidia = null;
                    switch (tipo) {
                        case 1:
                            VHS vhs = new VHS();
                            vhs.setTitulo(titulo);
                            vhs.setPreço(preco);
                            vhs.setDisponibilidade("Disponível");
                            novaMidia = vhs;
                            break;
                        case 2:
                            DVD dvd = new DVD();
                            dvd.setTitulo(titulo);
                            dvd.setPreço(preco);
                            dvd.setDisponibilidade("Disponível");
                            novaMidia = dvd;
                            break;
                        case 3:
                            Streaming s = new Streaming();
                            s.setTitulo(titulo);
                            s.setPreço(preco);
                            s.setDisponibilidade("Disponível");
                            novaMidia = s;
                            break;
                        default:
                            System.out.println("Tipo inválido!");
                    }
                    if (novaMidia != null) {
                        locadora.cadastrarMidia(novaMidia);
                        System.out.println("Mídia cadastrada com sucesso!");
                    }
                    break;

                case 2:
                    locadora.listarMidias();
                    break;

                case 3:
                    System.out.print("Nome do cliente: ");
                    String nomeCliente = sc.nextLine();
                    System.out.print("Email: ");
                    String email = sc.nextLine();
                    Cliente cliente = new Cliente(nomeCliente, email);
                    locadora.cadastrarCliente(cliente);

                    System.out.print("Título da mídia: ");
                    String tituloLoc = sc.nextLine();
                    Midia midiaLoc = locadora.buscarMidiaPorTitulo(tituloLoc);
                    if (midiaLoc == null) {
                        System.out.println("Mídia não encontrada!");
                        break;
                    }

                    System.out.print("Dias de locação: ");
                    int dias = sc.nextInt();

                    locadora.realizarLocacao(cliente, midiaLoc, dias);
                    break;

                case 4:
                    System.out.print("ID da locação: ");
                    int idLoc = sc.nextInt();
                    locadora.devolverMidia(idLoc);
                    break;

                case 5:
                    locadora.listarLocacoes();
                    break;

                case 6:
                    System.out.println("Total arrecadado: R$ " + locadora.calcularTotalArrecadado());
                    break;

                case 7:
                    System.out.println("Encerrando sistema...");
                    break;

                default:
                    System.out.println("Opção inválida!");
            }
        }
        sc.close();
    }
}

Midia.java
public abstract class Midia {
    private String titulo;
    private String disponibilidade;
    private double preco;

    public String getTitulo() {
        return titulo;
    }

    public void setTitulo(String titulo) {
        this.titulo = titulo;
    }

    public String getDisponibiliadade() {
        return disponibilidade;
    }

    public void setDisponibilidade(String disponibilidade) {
        this.disponibilidade = disponibilidade;
    }

    public double getPreço() {
        return preco;
    }

    public void setPreço(double preco) {
        this.preco = preco;
    }


    public abstract double calcularPreço(double precoBase);


    public abstract void exibirInfo();
}

VHS.java

public class VHS extends Midia {

    private boolean rebobinada;
    private double precoTaxa;

    public boolean getRebobinada() {
        return rebobinada;
    }

    public void setRebobinada(boolean rebobinada) {
        this.rebobinada = rebobinada;
    }

    public double getPreçoTaxa() {
        return precoTaxa;
    }

    public void setPreçoTaxa(double precoTaxa) {
        this.precoTaxa = precoTaxa;
    }

    @Override
    public void exibirInfo() {
        System.out.println("Título: " + getTitulo());
        System.out.println("Disponível: " + getDisponibiliadade());
        System.out.println("Rebobinada: " + (getRebobinada() ? "Sim" : "Não"));
        System.out.println("Preço: R$ " + getPreço());
        System.out.println("Preço com taxa: R$ " + (getPreço() + getPreçoTaxa()));
    }

    @Override
    public double calcularPreço(double precoBase) {
        
        if (!getRebobinada()) {
            return precoBase + precoTaxa;
        } else {
            return precoBase;
        }
    }
}

DVD.java

public class DVD extends Midia {

    private boolean possuiExtras;
    private double precoExtra;

    public boolean getPossuiExtras() {
        return possuiExtras;
    }

    public void setPossuiExtras(boolean possuiExtras) {
        this.possuiExtras = possuiExtras;
    }

    public double getPreçoExtra() {
        return precoExtra;
    }

    public void setPreçoExtra(double precoExtra) {
        this.precoExtra = precoExtra;
    }

    @Override
    public void exibirInfo() {
        System.out.println("Título: " + getTitulo());
        System.out.println("Disponível: " + getDisponibiliadade());
        System.out.println("Possui Extras: " + (getPossuiExtras() ? "Sim" : "Não"));
        System.out.println("Preço Base: R$ " + getPreço());
        if (getPossuiExtras()) {
            System.out.println("Preço com Extras: R$ " + (getPreço() + getPreçoExtra()));
        }
    }

    @Override
    public double calcularPreço(double precoBase) {
        if (getPossuiExtras()) {
            return precoBase + precoExtra;
        } else {
            return precoBase;
        }
    }
}

Streaming.java

public class Streaming extends Midia {

    private String plataforma;
    private double desconto;

    public String getPlataforma() {
        return plataforma;
    }

    public void setPlataforma(String plataforma) {
        this.plataforma = plataforma;
    }

    public double getDesconto() {
        return desconto;
    }

    public void setDesconto(double desconto) {
        this.desconto = desconto;
    }

    @Override
    public void exibirInfo() {
        System.out.println("Título: " + getTitulo());
        System.out.println("Plataforma: " + getPlataforma());
        System.out.println("Disponível: " + getDisponibiliadade());
        System.out.println("Preço original: R$ " + getPreço());
        System.out.println("Preço com desconto: R$ " + calcularPreço(getPreço()));
    }

    @Override
    public double calcularPreço(double precoBase) {
        
        return precoBase - (precoBase * desconto);
    }
}


Locacao.java

import java.time.LocalDate;

public class Locacao {
    private static int contador = 1;
    private int id;
    private Cliente cliente;
    private Midia midia;
    private int dias;
    private double valorTotal;
    private boolean ativa;
    private LocalDate dataLocacao;

    public Locacao(Cliente cliente, Midia midia, int dias) {
        this.id = contador++;
        this.cliente = cliente;
        this.midia = midia;
        this.dias = dias;
        this.valorTotal = midia.calcularPreço(midia.getPreço()) * dias;
        this.ativa = true;
        this.dataLocacao = LocalDate.now();
        midia.setDisponibilidade("Indisponível");
    }

    public void finalizarLocacao() {
        this.ativa = false;
        midia.setDisponibilidade("Disponível");
    }

    public boolean isAtiva() {
        return ativa;
    }

    public double getValorTotal() {
        return valorTotal;
    }

    @Override
    public String toString() {
        return "Locação ID: " + id +
                " Cliente: " + cliente.getNome() +
                " Mídia: " + midia.getTitulo() +
                " Dias: " + dias +
                " Valor: R$" + valorTotal +
                " Status: " + (ativa ? "Ativa" : "Finalizada");
    }
}


GerenciadorLocadora

import java.util.ArrayList;

public class GerenciadorLocadora {
    private ArrayList<Midia> midias = new ArrayList<>();
    private ArrayList<Locacao> locacoes = new ArrayList<>();
    private ArrayList<Cliente> clientes = new ArrayList<>();

    public void cadastrarMidia(Midia midia) {
        midias.add(midia);
    }

    public void cadastrarCliente(Cliente cliente) {
        clientes.add(cliente);
    }

    public void listarMidias() {
        if (midias.isEmpty()) {
            System.out.println("Nenhuma mídia cadastrada.");
            return;
        }
        for (Midia m : midias) {
            m.exibirInfo();
            System.out.println("    ");
        }
    }

    public void realizarLocacao(Cliente cliente, Midia midia, int dias) {
        if (midia.getDisponibiliadade().equalsIgnoreCase("Disponível")) {
            Locacao locacao = new Locacao(cliente, midia, dias);
            locacoes.add(locacao);
            System.out.println("Locação realizada com sucesso!");
        } else {
            System.out.println("Mídia indisponível no momento.");
        }
    }

    public void devolverMidia(int idLocacao) {
        for (Locacao loc : locacoes) {
            if (loc.isAtiva() && loc.toString().contains("ID: " + idLocacao)) {
                loc.finalizarLocacao();
                System.out.println("Mídia devolvida com sucesso!");
                return;
            }
        }
        System.out.println("Locação não encontrada ou já finalizada.");
    }

    public void listarLocacoes() {
        if (locacoes.isEmpty()) {
            System.out.println("Nenhuma locação registrada.");
            return;
        }
        for (Locacao l : locacoes) {
            System.out.println(l);
        }
    }

    public double calcularTotalArrecadado() {
        double total = 0;
        for (Locacao l : locacoes) {
            total += l.getValorTotal();
        }
        return total;
    }

    public Cliente buscarClientePorNome(String nome) {
        for (Cliente c : clientes) {
            if (c.getNome().equalsIgnoreCase(nome)) return c;
        }
        return null;
    }

    public Midia buscarMidiaPorTitulo(String titulo) {
        for (Midia m : midias) {
            if (m.getTitulo().equalsIgnoreCase(titulo)) return m;
        }
        return null;
    }
}

Cliente.java

public class Cliente {
    private static int contador = 1;
    private int id;
    private String nome;
    private String email;

    public Cliente(String nome, String email) {
        this.id = contador++;
        this.nome = nome;
        this.email = email;
    }

    public int getId() {
        return id;
    }

    public String getNome() {
        return nome;
    }

    public void setNome(String nome) {
        this.nome = nome;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    @Override
    public String toString() {
        return "Cliente ID: " + id + "Nome: " + nome + "Email: " + email;
    }
}

