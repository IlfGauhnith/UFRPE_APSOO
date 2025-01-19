---
config:
  theme: neo
  look: handDrawn
---
classDiagram
    namespace GUI {
        class TelaRegistro { 
            + abrirSecaoInfoPessoais() : void
            + abrirSecaoInfoAcademicas() : void
            + abrirSecaoInfoPesquisas() : void
            + abrirSecaoInteresses() : void
            + abrirConsentimentoLGPD() : void
        }
    }
    namespace Negocio {
        class Fachada {
            + efetuarRegistro() : void
            + mostrarErro(mensagem) : void
        }   
        class ControladorRegistro {  
            + validarDados(email, cpf) : boolean
            + efetuarRegistro() : void
        }
        class CadastroPesquisa {
            + cadastrarPesquisa() : void
        }
        class CadastroPesquisadorEmpreendedor { 
            + cadastrarPesquisadorEmpreendedor() : void    
        }
        class Pesquisa { 
            +String titulo
            +String descricao
            +String area
            +String maturidade
            +String impactos
            +String[] conexoesODS
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
            + enviarEmailConfirmacao(email) : void
        }
    }
    namespace Interfaces {
        class ISubsistemaServicoEmail {  
            + enviarEmailConfirmacao(email) : void
        }
        class IRepositorioPesquisa {
            + cadastrarPesquisa() : void
        }
        class IRepositorioPesquisadorEmpreendedor {
            + cadastrarPesquisa() : void
        }
    }
    namespace Dados {
        class RepositorioPesquisaBDR {
            + cadastrarPesquisa() : void
        }
        class RepositorioPesquisaArquivos {
            + cadastrarPesquisa() : void
        }
        class RepositorioPesquisadorEmpreendedorBDR {
            + cadastrarPesquisa() : void
        }
        class RepositorioPesquisadorEmpreendedorArquivos {
            + cadastrarPesquisa() : void
        }
    }
    PesquisadorEmpreendedor *-- Pesquisa
    FachadaSubsistemaServicoEmail ..|> ISubsistemaServicoEmail
    RepositorioPesquisaBDR ..|> IRepositorioPesquisa
    RepositorioPesquisaArquivos ..|> IRepositorioPesquisa
    RepositorioPesquisadorEmpreendedorBDR ..|> IRepositorioPesquisadorEmpreendedor
    RepositorioPesquisadorEmpreendedorArquivos ..|> IRepositorioPesquisadorEmpreendedor
    CadastroPesquisadorEmpreendedor --> IRepositorioPesquisadorEmpreendedor 
    CadastroPesquisa --> IRepositorioPesquisa
    TelaRegistro --> Fachada
    Fachada --> ControladorRegistro
    ControladorRegistro --> CadastroPesquisadorEmpreendedor
    ControladorRegistro --> CadastroPesquisa
    CadastroPesquisadorEmpreendedor --> PesquisadorEmpreendedor
    CadastroPesquisa --> Pesquisa
    ControladorRegistro --> ISubsistemaServicoEmail
