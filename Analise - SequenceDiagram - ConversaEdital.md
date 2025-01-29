---
config:
  theme: mc
---
sequenceDiagram
    actor PesquisadorEmpreendedor
    participant TelaConversaEdital
    participant ControladorConversaEdital
    participant ServicoLLM
    participant CadastroEdital

    PesquisadorEmpreendedor ->>+ TelaConversaEdital: AbrirDetalhesEdital(int idEdital)
    PesquisadorEmpreendedor ->> TelaConversaEdital: AbrirConversaEdital(int idEdital)
    TelaConversaEdital ->> ControladorConversaEdital: isEditalProcessado(int idEdital)
    ControladorConversaEdital ->> CadastroEdital: isEditalProcessado(int idEdital)
    CadastroEdital -->> ControladorConversaEdital: Resultado
    ControladorConversaEdital -->> TelaConversaEdital: Resultado
    alt Edital ja esta processado
        loop Para cada pergunta
            TelaConversaEdital -->>- PesquisadorEmpreendedor: Campo para digitar pergunta
            PesquisadorEmpreendedor ->>+ TelaConversaEdital: Digita pergunta
            TelaConversaEdital ->>+ ControladorConversaEdital: promptLLM(String pergunta, int idEdital)
            ControladorConversaEdital ->>+ CadastroEdital: buscarEdital(int idEdital)
            CadastroEdital -->> ControladorConversaEdital: Edital encontrado
            ControladorConversaEdital ->>+ ServicoLLM: validarPerguntaContexto(String pergunta, String contexto)
            ServicoLLM -->> ControladorConversaEdital: Resultado validação
            alt Pergunta dentro do contexto
                ControladorConversaEdital ->> ServicoLLM:  processarPergunta(String pergunta, String contexto)
                ServicoLLM -->> ControladorConversaEdital: Resultado prompt
                alt Resposta obtida
                    ControladorConversaEdital -->> TelaConversaEdital: Resposta
                    TelaConversaEdital -->> PesquisadorEmpreendedor: Resposta
                else Erro no processamento
                    loop Tentativa de reenvio (max tres vezes)
                        ControladorConversaEdital ->> ServicoLLM: processarPergunta(String pergunta, String contexto)
                        ServicoLLM -->> ControladorConversaEdital: Resultado tentativa
                    end
                    alt Falha apos tentativas
                        ControladorConversaEdital ->> TelaConversaEdital: exibirErro("Não foi possível obter uma resposta. Tente novamente mais tarde.")
                    end
                end
            else Pergunta fora do contexto
                ControladorConversaEdital ->> TelaConversaEdital: exibirErro(String mensagem)
            end
        end
    else Edital nao esta processado
        ControladorConversaEdital ->> TelaConversaEdital: exibirErro("Funcionalidade não está disponível para este edital")
    end
