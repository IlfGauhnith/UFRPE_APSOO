---
config:
  theme: neo-dark
---
sequenceDiagram
    actor PesquisadorEmpreendedor
    participant TelaRegistro
    participant ControladorRegistro
    participant CadastroPesquisadorEmpreendedor
    participant CadastroPesquisa


    PesquisadorEmpreendedor ->>+ TelaRegistro: abrirSecaoInfoPessoais()
    TelaRegistro -->>- PesquisadorEmpreendedor: Seção de Informações Pessoais
    PesquisadorEmpreendedor ->> TelaRegistro: preencherInfoPessoais(nomeCompleto, cpf, email, telefone, endereco)


    PesquisadorEmpreendedor ->>+ TelaRegistro: abrirSecaoInfoAcademicas()
    TelaRegistro -->>- PesquisadorEmpreendedor: Seção de Informações Academicas
    PesquisadorEmpreendedor ->> TelaRegistro: preencherInfoAcademicas(detalhesGraduacao, detalhesPosgraduacao, expProfissional, curLates)


    PesquisadorEmpreendedor ->>+ TelaRegistro: abrirSecaoInfoPesquisas()
    TelaRegistro -->>- PesquisadorEmpreendedor: Seção de Informações Pesquisas
    loop Para cada pesquisa
        PesquisadorEmpreendedor ->> TelaRegistro: preencherInfoPesquisa(titulo, descricao, area, maturidade, impactos, conexoesODS)
    end
    
    PesquisadorEmpreendedor ->>+ TelaRegistro: abrirSecaoInteresses()
    TelaRegistro -->>- PesquisadorEmpreendedor: Seção de Capacidades e Interesses
    PesquisadorEmpreendedor ->> TelaRegistro: preencherInteresses(expEmpreen, interesseEmpresa, interesseTransferenciaTecnologia, disponibilidadeCapacitacao)

    PesquisadorEmpreendedor ->>+ TelaRegistro: abrirConsentimentoLGPD()
    TelaRegistro -->>- PesquisadorEmpreendedor: Termo de Consentimento LGPD
    PesquisadorEmpreendedor ->> TelaRegistro: consentirTermoLGPD()

    
    PesquisadorEmpreendedor ->>+ TelaRegistro: efetuarRegistro()
    TelaRegistro ->>+ ControladorRegistro: validarDados(email, cpf)
    ControladorRegistro -->>- TelaRegistro: resultado
    deactivate TelaRegistro

    alt Dados validos

        TelaRegistro ->>+ ControladorRegistro: efetuarRegistro()
        ControladorRegistro ->>+ CadastroPesquisadorEmpreendedor: cadastrarPesquisadorEmpreendedor()
        ControladorRegistro ->>+ CadastroPesquisa: cadastrarPesquisa()
        ControladorRegistro ->> ServicoEmail: enviarEmailConfirmacao(email)
        deactivate ControladorRegistro
        deactivate CadastroPesquisadorEmpreendedor
        deactivate CadastroPesquisa

    else Dados invalidos
        TelaRegistro ->> PesquisadorEmpreendedor:mostrarErro()
    end