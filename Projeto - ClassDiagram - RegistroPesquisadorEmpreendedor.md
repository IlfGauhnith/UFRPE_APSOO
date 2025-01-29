---
config:
  theme: mc
  look: neo
---
classDiagram
direction TB
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
	        + cadastrarPesquisa(Pesquisa pesquisa) : void
        }
        class RepositorioPesquisaArquivos {
	        + cadastrarPesquisa(Pesquisa pesquisa) : void
        }
        class RepositorioPesquisadorEmpreendedorBDR {
	        + cadastrarPesquisadorEmpreendedor(PesquisadorEmpreendedor pesquisadorEmpreendedor) : void
        }
        class RepositorioPesquisadorEmpreendedorArquivos {
	        + cadastrarPesquisadorEmpreendedor(PesquisadorEmpreendedor pesquisadorEmpreendedor) : void
        }
	}

    PesquisadorEmpreendedor "1..*" o-- "0..1" Pesquisa
    Pesquisa "0..1" *-- "1..*" PesquisadorEmpreendedor
    FachadaSubsistemaServicoEmail ..|> ISubsistemaServicoEmail
    RepositorioPesquisaBDR ..|> IRepositorioPesquisa
    RepositorioPesquisaArquivos ..|> IRepositorioPesquisa
    RepositorioPesquisadorEmpreendedorBDR ..|> IRepositorioPesquisadorEmpreendedor
    RepositorioPesquisadorEmpreendedorArquivos ..|> IRepositorioPesquisadorEmpreendedor
    CadastroPesquisadorEmpreendedor "0" --> "1" IRepositorioPesquisadorEmpreendedor
    CadastroPesquisa "0" --> "1" IRepositorioPesquisa
    TelaRegistroPesquisadorEmpreendedor "0" --> "1" Fachada
    Fachada "0" --> "1" ControladorRegistroPesquisadorEmpreendedor
    ControladorRegistroPesquisadorEmpreendedor "0" --> "1" CadastroPesquisadorEmpreendedor
    ControladorRegistroPesquisadorEmpreendedor "0" --> "1" CadastroPesquisa
    CadastroPesquisadorEmpreendedor "0" --> "1" PesquisadorEmpreendedor
    CadastroPesquisa "0" --> "1" Pesquisa
    ControladorRegistroPesquisadorEmpreendedor "0" --> "1" ISubsistemaServicoEmail


	note "Qual a relação entre IRepositorioPesquisa e Pesquisa? E RepositorioPesquisa e Pesquisa?"
    note "Subsistema de email está correto? Fachada implementa interface e controlador tem uma associacao com interface."