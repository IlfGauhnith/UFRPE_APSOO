---
config:
  theme: neo-dark
---

classDiagram
    class TelaRegistro { 
        <<boundary>> 
        + abrirSecaoInfoPessoais() : void
        + abrirSecaoInfoAcademicas() : void
        + abrirSecaoInfoPesquisas() : void
        + abrirSecaoInteresses() : void
        + abrirConsentimentoLGPD() : void
        + efetuarRegistro() : void
        + mostrarErro(mensagem) : void
    }
    class ServicoEmail { 
        <<boundary>> 
        + enviarEmailConfirmacao(email) : void
    }

    class ControladorRegistro { 
        <<control>> 
        + validarDados(email, cpf) : boolean
        + efetuarRegistro() : void

    }

    class PesquisadorEmpreendedor { 
        <<entity>>

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
    class CadastroPesquisadorEmpreendedor { 
        <<entity collection>> 
        + cadastrarPesquisadorEmpreendedor() : void    
    }

    class Pesquisa { 
        <<entity>> 
        +String titulo
        +String descricao
        +String area
        +String maturidade
        +String impactos
        +String[] conexoesODS
    }
    class CadastroPesquisa { 
        <<entity collection>> 
        + cadastrarPesquisa() : void
    }

    TelaRegistro --> ControladorRegistro
    ControladorRegistro --> CadastroPesquisadorEmpreendedor
    ControladorRegistro --> CadastroPesquisa
    ControladorRegistro --> ServicoEmail
    PesquisadorEmpreendedor *-- Pesquisa
    CadastroPesquisa o-- Pesquisa
    CadastroPesquisadorEmpreendedor o-- PesquisadorEmpreendedor