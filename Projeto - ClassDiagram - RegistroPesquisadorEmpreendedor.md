---
config:
  theme: mc
  look: neo
---
classDiagram
direction LR
	namespace GUI {
        class TelaRegistroPesquisadorEmpreendedor {
	        + abrirSecaoInfoPessoais() : void
	        + abrirSecaoInfoAcademicas() : void
	        + abrirSecaoInfoPesquisas() : void
	        + abrirSecaoInteresses() : void
	        + abrirConsentimentoLGPD() : void
	        + efetuarRegistro() : void
	        + mostrarErro(String mensagem) : void
	        + efetuarRegistro(PesquisadorEmpreendedor pesquisadorEmpreendedor) : void
        }
	}
	namespace Negocio {
        class Fachada {
            - static instance : Fachada
            - Fachada()
            + static getInstance() : Fachada
	        + efetuarRegistro(PesquisadorEmpreendedor pesquisadorEmpreendedor) : void
	        + mostrarErro(mensagem) : void
        }
        class ControladorRegistroPesquisadorEmpreendedor {
	        + validarDados(String email, String cpf) : boolean
	        + efetuarRegistro() : void
        }
        class CadastroPesquisa {
	        + cadastrarPesquisa(Pesquisa pesquisa) : void
        }
        class CadastroPesquisadorEmpreendedor {
	        + cadastrarPesquisadorEmpreendedor(PesquisadorEmpreendedor pesquisadorEmpreendedor) : void
        }
        class Pesquisa {
	        +String titulo
	        +String descricao
	        +String area
	        +String maturidade
	        +String impactos
	        +String[] conexoesODS
	        +PesquisadorEmpreendedor[] pesquisadores
        }

        class PesquisaBuilder {
            + setTitulo(String titulo) : PesquisaBuilder
            + setDescricao(String descricao) : PesquisaBuilder
            + setArea(String area) : PesquisaBuilder
            + setMaturidade(String maturidade) : PesquisaBuilder
            + setImpactos(String impactos) : PesquisaBuilder
            + setConexoesODS(String[] conexoesODS) : PesquisaBuilder
            + setPesquisadores(PesquisadorEmpreendedor[] pesquisadores) : PesquisaBuilder
            + build() : Pesquisa
        }

        class PesquisadorEmpreendedor {
	        +String nomeCompleto
	        +String cpf
	        +String email
	        +String telefone
	        +String endereco
	        +String detalhesGraduacao
	        +String detalhesPosGraduacao
	        +String expProfissional
	        +String curLates
	        +boolean expEmpreen
	        +boolean interesseEmpresa
	        +boolean interesseTransferenciaTecnologia
	        +boolean disponibilidadeCapacitacao
	        +Pesquisa[] pesquisas
        }

        class PesquisadorEmpreendedorBuilder {
            + setNomeCompleto(String nome) : PesquisadorEmpreendedorBuilder
            + setCpf(String cpf) : PesquisadorEmpreendedorBuilder
            + setEmail(String email) : PesquisadorEmpreendedorBuilder
            + setTelefone(String telefone) : PesquisadorEmpreendedorBuilder
            + setEndereco(String endereco) : PesquisadorEmpreendedorBuilder
            + setDetalhesGraduacao(String detalhesGraduacao) : PesquisadorEmpreendedorBuilder
            + setDetalhesPosGraduacao(String detalhesPosGraduacao) : PesquisadorEmpreendedorBuilder
            + setExpProfissional(String expProfissional) : PesquisadorEmpreendedorBuilder
            + setCurLates(String curLates) : PesquisadorEmpreendedorBuilder
            + setExpEmpreen(boolean expEmpreen) : PesquisadorEmpreendedorBuilder
            + setInteresseEmpresa(boolean interesseEmpresa) : PesquisadorEmpreendedorBuilder
            + setInteresseTransferenciaTecnologia(boolean interesseTransferenciaTecnologia) : PesquisadorEmpreendedorBuilder
            + setDisponibilidadeCapacitacao(boolean disponibilidadeCapacitacao) : PesquisadorEmpreendedorBuilder
            + setPesquisas(Pesquisa[] pesquisas) : PesquisadorEmpreendedorBuilder
            + build() : PesquisadorEmpreendedor
        }

        class FachadaSubsistemaServicoEmail {
	        + enviarEmailConfirmacao(String email) : void
        }
	}
	namespace Interfaces {
        class ISubsistemaServicoEmail {
	        + enviarEmailConfirmacao(String email) : void
        }
        class IRepositorioPesquisa {
	        + cadastrarPesquisa(Pesquisa pesquisa) : void
        }
        class IRepositorioPesquisadorEmpreendedor {
	        + cadastrarPesquisadorEmpreendedor(PesquisadorEmpreendedor pesquisadorEmpreendedor) : void
        }
	}
	namespace Dados {

        class RepositorioPesquisaBDR {
            - static instance : RepositorioPesquisaBDR
	        + static getInstance() : RepositorioPesquisaBDR
            - RepositorioPesquisaBDR()
	        + cadastrarPesquisa(Pesquisa pesquisa) : void
        }
        class RepositorioPesquisaArquivos {
            - static instance : RepositorioPesquisaArquivos
	        + static getInstance() : RepositorioPesquisaArquivos
	        - RepositorioPesquisaArquivos()
            + cadastrarPesquisa(Pesquisa pesquisa) : void
        }
        class RepositorioPesquisadorEmpreendedorBDR {
            - static instance : RepositorioPesquisadorEmpreendedorBDR
	        + static getInstance() : RepositorioPesquisadorEmpreendedorBDR
	        - RepositorioPesquisadorEmpreendedorBDR()
            + cadastrarPesquisadorEmpreendedor(PesquisadorEmpreendedor pesquisadorEmpreendedor) : void
        }
        class RepositorioPesquisadorEmpreendedorArquivos {
            - static instance : RepositorioPesquisadorEmpreendedorArquivos
	        + static getInstance() : RepositorioPesquisadorEmpreendedorArquivos
	        - RepositorioPesquisadorEmpreendedorArquivos()
            + cadastrarPesquisadorEmpreendedor(PesquisadorEmpreendedor pesquisadorEmpreendedor) : void
        }
	}

    PesquisadorEmpreendedor "1..*" o-- "0..1" Pesquisa
    Pesquisa "0..1" *-- "1..*" PesquisadorEmpreendedor
    PesquisaBuilder ..> Pesquisa
    PesquisadorEmpreendedorBuilder ..> PesquisadorEmpreendedor
    
    FachadaSubsistemaServicoEmail ..|> ISubsistemaServicoEmail

    RepositorioPesquisaBDR ..|> IRepositorioPesquisa
    RepositorioPesquisaArquivos ..|> IRepositorioPesquisa
    RepositorioPesquisadorEmpreendedorBDR ..|> IRepositorioPesquisadorEmpreendedor
    RepositorioPesquisadorEmpreendedorArquivos ..|> IRepositorioPesquisadorEmpreendedor

    IRepositorioPesquisa ..> Pesquisa
    IRepositorioPesquisadorEmpreendedor ..> PesquisadorEmpreendedor
    
    CadastroPesquisadorEmpreendedor "0" --> "1" IRepositorioPesquisadorEmpreendedor
    CadastroPesquisa "0" --> "1" IRepositorioPesquisa
    
    TelaRegistroPesquisadorEmpreendedor "0" --> "1" Fachada
    TelaRegistroPesquisadorEmpreendedor ..> PesquisadorEmpreendedor
    Fachada "0" --> "1" ControladorRegistroPesquisadorEmpreendedor
    Fachada ..> PesquisadorEmpreendedor
    Fachada ..> Pesquisa
    
    ControladorRegistroPesquisadorEmpreendedor "0" --> "1" CadastroPesquisadorEmpreendedor
    ControladorRegistroPesquisadorEmpreendedor "0" --> "1" CadastroPesquisa
    CadastroPesquisadorEmpreendedor ..> PesquisadorEmpreendedor
    CadastroPesquisadorEmpreendedor "0" --> "1" PesquisadorEmpreendedorBuilder
    CadastroPesquisa ..> Pesquisa
    CadastroPesquisa "0" --> "1" PesquisaBuilder
    ControladorRegistroPesquisadorEmpreendedor "0" --> "1" ISubsistemaServicoEmail


	note "Padrão Fachada na classe Fachada."
    note "Padrão Singleton nos repositórios e Fachada."
    note "Padrão Builder para PesquisadorEmpreendedor e Pesquisa."
