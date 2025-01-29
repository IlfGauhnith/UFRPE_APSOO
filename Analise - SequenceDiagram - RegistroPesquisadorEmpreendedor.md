---
config:
  theme: mc
---
sequenceDiagram
    actor PesquisadorEmpreendedor
    participant TelaRegistroPesquisadorEmpreendedor
    participant ControladorRegistroPesquisadorEmpreendedor
    participant CadastroPesquisadorEmpreendedor
    participant CadastroPesquisa
    PesquisadorEmpreendedor ->>+ TelaRegistroPesquisadorEmpreendedor: abrirSecaoInfoPessoais()
    TelaRegistroPesquisadorEmpreendedor -->>- PesquisadorEmpreendedor: Seção de Informações Pessoais
    PesquisadorEmpreendedor ->> TelaRegistroPesquisadorEmpreendedor: Preencher as informações pessoais nomeCompleto, cpf, email, telefone e endereco
    PesquisadorEmpreendedor ->>+ TelaRegistroPesquisadorEmpreendedor: abrirSecaoInfoAcademicas()
    TelaRegistroPesquisadorEmpreendedor -->>- PesquisadorEmpreendedor: Seção de Informações Academicas
    PesquisadorEmpreendedor ->> TelaRegistroPesquisadorEmpreendedor: Preencher as informações acadêmicas detalhesGraduacao, detalhesPosgraduacao, expProfissional e curLates
    PesquisadorEmpreendedor ->>+ TelaRegistroPesquisadorEmpreendedor: abrirSecaoInfoPesquisas()
    TelaRegistroPesquisadorEmpreendedor -->>- PesquisadorEmpreendedor: Seção de Informações Pesquisas
    loop Para cada pesquisa
        PesquisadorEmpreendedor ->> TelaRegistroPesquisadorEmpreendedor: Preencher as informações da pesquisa titulo, descricao, area, maturidade, impactos e conexoesODS
    end
    PesquisadorEmpreendedor ->>+ TelaRegistroPesquisadorEmpreendedor: abrirSecaoInteresses()
    TelaRegistroPesquisadorEmpreendedor -->>- PesquisadorEmpreendedor: Seção de Capacidades e Interesses
    PesquisadorEmpreendedor ->> TelaRegistroPesquisadorEmpreendedor: Preencher as informações de capacidades e interesses expEmpreen, interesseEmpresa, interesseTransferenciaTecnologia e disponibilidadeCapacitacao
    PesquisadorEmpreendedor ->>+ TelaRegistroPesquisadorEmpreendedor: abrirConsentimentoLGPD()
    TelaRegistroPesquisadorEmpreendedor -->>- PesquisadorEmpreendedor: Termo de Consentimento LGPD
    PesquisadorEmpreendedor ->> TelaRegistroPesquisadorEmpreendedor: Consentir com o termo de LGPD
    PesquisadorEmpreendedor ->>+ TelaRegistroPesquisadorEmpreendedor: Efetua o registro
    TelaRegistroPesquisadorEmpreendedor ->>+ ControladorRegistroPesquisadorEmpreendedor: validarDados(String email, String cpf)
    ControladorRegistroPesquisadorEmpreendedor -->>- TelaRegistroPesquisadorEmpreendedor: resultado
    deactivate TelaRegistroPesquisadorEmpreendedor
    alt Dados validos
        TelaRegistroPesquisadorEmpreendedor ->>+ ControladorRegistroPesquisadorEmpreendedor: efetuarRegistro(PesquisadorEmpreendedor pesquisadorEmpreendedor)
        ControladorRegistroPesquisadorEmpreendedor ->>+ CadastroPesquisadorEmpreendedor: cadastrarPesquisadorEmpreendedor(PesquisadorEmpreendedor pesquisadorEmpreendedor)
        loop Para cada pesquisa
            CadastroPesquisadorEmpreendedor ->>+ CadastroPesquisa: cadastrarPesquisa(Pesquisa pesquisa)
        end
        ControladorRegistroPesquisadorEmpreendedor ->> ServicoEmail: enviarEmailConfirmacao(String email)
        deactivate ControladorRegistroPesquisadorEmpreendedor
        deactivate CadastroPesquisadorEmpreendedor
        deactivate CadastroPesquisa
    else Dados invalidos
        TelaRegistroPesquisadorEmpreendedor ->> PesquisadorEmpreendedor:mostrarErro(String mensagem)
    end
