---
config:
  theme: mc
  look: neo
---
classDiagram
direction TB
    
    class TelaConversaEdital {
        <<boundary>>
        + AbrirDetalhesEdital(int idEdital) : void
        + AbrirConversaEdital(int idEdital) : void
        + exibirErro(String mensagem) : void
    }

    class ControladorConversaEdital {
        <<control>>
        + isEditalProcessado(int idEdital) : boolean
        + promptLLM(String pergunta, int idEdital) : String
        + exibirErro(String mensagem) : void
    }

    class ServicoLLM {
        <<boundary>>
        + validarPerguntaContexto(String pergunta, String contexto) : boolean
        + processarPergunta(String pergunta, String contexto) : String
    }

    class CadastroEdital {
        <<entity collection>>
        + isEditalProcessado(int idEdital) : boolean
        + buscarEdital(int idEdital) : Edital
    }

    class Edital {
        <<entity>>
        +int id
        +String texto
    }

    TelaConversaEdital "0" --> "1" ControladorConversaEdital
    ControladorConversaEdital "0" --> "1" CadastroEdital
    ControladorConversaEdital "0" --> "1" ServicoLLM
    CadastroEdital "0" o-- "1" Edital