---
config:
  theme: mc
  look: neo
---
classDiagram
    direction RL
    namespace GUI {
        class TelaConversaEdital {
            + AbrirDetalhesEdital(int idEdital) : void
            + AbrirConversaEdital(int idEdital) : void
            + exibirErro(String mensagem) : void
        }  
    }

    namespace Negocio {
        class Fachada {
            + isEditalProcessado(int idEdital) : boolean
            + promptLLM(String pergunta, int idEdital) : String
            + exibirErro(String mensagem) : void
        }

        class Edital {
            +int id
            +String texto
        }

        class ControladorConversaEdital {
            + isEditalProcessado(int idEdital) : boolean
            + promptLLM(String pergunta, int idEdital) : String
            + exibirErro(String mensagem) : void
        }

        class FachadaSubSistemaLLMGoogle {
            + validarPerguntaContexto(String pergunta, String contexto) : boolean
            + processarPergunta(String pergunta, String contexto) : String
        }

        class FachadaSubSistemaLLMAmazon {
            + validarPerguntaContexto(String pergunta, String contexto) : boolean
            + processarPergunta(String pergunta, String contexto) : String
        }

        class FachadaSubSistemaLLMOpenAI {
            + validarPerguntaContexto(String pergunta, String contexto) : boolean
            + processarPergunta(String pergunta, String contexto) : String
        }

        class CadastroEdital {
            + buscarEdital(int idEdital) : Edital
            + isEditalProcessado(int idEdital) : boolean
        }
    }

    namespace Interfaces {
        class ISubSistemaLLM {
            + validarPerguntaContexto(String pergunta, String contexto) : boolean
            + processarPergunta(String pergunta, String contexto) : String
        }

        class IRepositorioEdital {
            + buscarEdital(int idEdital) : Edital
            + isEditalProcessado(int idEdital) : boolean
        }
    }

    namespace Dados {
        class RepositorioEditalBDR {
            + buscarEdital(int idEdital) : Edital
            + isEditalProcessado(int idEdital) : boolean
        }

        class RepositorioEditalArquivo {
            + buscarEdital(int idEdital) : Edital
            + isEditalProcessado(int idEdital) : boolean
        }
    }

    TelaConversaEdital "0" --> "1" Fachada
    Fachada "0" --> "1" ControladorConversaEdital
    ControladorConversaEdital "0" --> CadastroEdital
    ControladorConversaEdital "0" --> "1" ISubSistemaLLM
    CadastroEdital "0" --> "1" Edital
    CadastroEdital "0" --> "1" IRepositorioEdital
    FachadaSubSistemaLLMGoogle ..|> ISubSistemaLLM
    FachadaSubSistemaLLMAmazon ..|> ISubSistemaLLM
    FachadaSubSistemaLLMOpenAI ..|> ISubSistemaLLM
    RepositorioEditalBDR ..|> IRepositorioEdital
    RepositorioEditalArquivo ..|> IRepositorioEdital