---
config:
  theme: mc
---
classDiagram
    class TelaRegistroPesquisadorEmpreendedor { 
        <<boundary>> 
        + abrirSecaoInfoPessoais() : void
        + abrirSecaoInfoAcademicas() : void
        + abrirSecaoInfoPesquisas() : void
        + abrirSecaoInteresses() : void
        + abrirConsentimentoLGPD() : void
        + efetuarRegistro() : void
        + mostrarErro(String mensagem) : void
        + efetuarRegistro(PesquisadorEmpreendedor pesquisadorEmpreendedor) : void
    }
    class ServicoEmail { 
        <<boundary>> 
        + enviarEmailConfirmacao(String email) : void
    }
    class ControladorRegistroPesquisadorEmpreendedor { 
        <<control>> 
        + validarDados(String email, String cpf) : boolean
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
        + cadastrarPesquisadorEmpreendedor(PesquisadorEmpreendedor pesquisadorEmpreendedor) : void    
    }
    class Pesquisa { 
        <<entity>> 
        +String titulo
        +String descricao
        +String area
        +String maturidade
        +String impactos
        +String[] conexoesODS
        +PesquisadorEmpreendedor[] pesquisadores
    }
    class CadastroPesquisa { 
        <<entity collection>> 
        + cadastrarPesquisa(Pesquisa pesquisa) : void
    }
    TelaRegistroPesquisadorEmpreendedor "0" --> "1" ControladorRegistroPesquisadorEmpreendedor
    ControladorRegistroPesquisadorEmpreendedor "0" --> "1" CadastroPesquisadorEmpreendedor
    ControladorRegistroPesquisadorEmpreendedor "0" --> "1" CadastroPesquisa
    ControladorRegistroPesquisadorEmpreendedor "0" --> "1" ServicoEmail
    PesquisadorEmpreendedor "1..*" o-- "0..1" Pesquisa
    Pesquisa "0..1" *-- "1..*" PesquisadorEmpreendedor
    CadastroPesquisa "0" o-- "1" Pesquisa
    CadastroPesquisadorEmpreendedor "0" o-- "1" PesquisadorEmpreendedor
