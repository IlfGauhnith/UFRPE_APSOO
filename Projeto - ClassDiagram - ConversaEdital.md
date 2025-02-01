---
config:
  theme: mc
  look: neo
---
classDiagram
    direction TB
    
    namespace GUI {
        class TelaConversaEdital {
            + AbrirDetalhesEdital(int idEdital) : void
            + AbrirConversaEdital(int idEdital) : void
            + exibirErro(String mensagem) : void
        }  
    }

    namespace Negocio {
        class Fachada {
            - static instance : Fachada
            + static getInstance() : Fachada
            - Fachada()
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
            + setAdapterSubSistemaLLM(ISubSistemaLLM adapter) : void
            + createMemento() : MementoConversa
            + restoreMemento(MementoConversa memento) : void
        }

        class MementoConversa {
            - String estadoInterno
            + getEstado() : String
        }

        class HistoricoConversa {
            + addMemento(MementoConversa m) : void
            + getMemento(int index) : MementoConversa
            - MementoConversa[] : mementos
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

        class AdapterLLMGoogle {
            + validarPerguntaContexto(String pergunta, String contexto) : boolean
            + processarPergunta(String pergunta, String contexto) : String
        }

        class AdapterLLMAmazon {
            + validarPerguntaContexto(String pergunta, String contexto) : boolean
            + processarPergunta(String pergunta, String contexto) : String
        }

        class AdapterLLMOpenAI {
            + validarPerguntaContexto(String pergunta, String contexto) : boolean
            + processarPergunta(String pergunta, String contexto) : String
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
            - static instance : RepositorioEditalBDR
            + static getInstance() : RepositorioEditalBDR
            - RepositorioEditalBDR()
            + buscarEdital(int idEdital) : Edital
            + isEditalProcessado(int idEdital) : boolean
        }

        class RepositorioEditalArquivo {
            - static instance : RepositorioEditalArquivo
            + static getInstance() : RepositorioEditalArquivo
            - RepositorioEditalArquivo()
            + buscarEdital(int idEdital) : Edital
            + isEditalProcessado(int idEdital) : boolean
        }
    }

    TelaConversaEdital "0" --> "1" Fachada
    Fachada "0" --> "1" ControladorConversaEdital
    ControladorConversaEdital "0" --> CadastroEdital
    ControladorConversaEdital "0" --> "1" ISubSistemaLLM
    ControladorConversaEdital ..> ISubSistemaLLM
    CadastroEdital ..> Edital
    CadastroEdital "0" --> "1" IRepositorioEdital
    AdapterLLMGoogle ..|> ISubSistemaLLM
    AdapterLLMAmazon ..|> ISubSistemaLLM
    AdapterLLMOpenAI ..|> ISubSistemaLLM
    IRepositorioEdital ..> Edital
    RepositorioEditalBDR ..> Edital
    RepositorioEditalArquivo ..> Edital
    RepositorioEditalBDR ..|> IRepositorioEdital
    RepositorioEditalArquivo ..|> IRepositorioEdital

    ControladorConversaEdital "0" --> "1" HistoricoConversa
    ControladorConversaEdital ..> MementoConversa
    HistoricoConversa "0" o-- "1..*" MementoConversa
    note "Padrão Fachada na classe Fachada"
    note "Padrão Singleton nos repositórios e Fachada"
    note "Padrão Adapter (ISubSistemaLLM) para adaptar diferentes APIs de LLMs (Google, Amazon, OpenAI)"
    note "Padrão Memento: ControladorConversaEdital (Originator), MementoConversa (Memento), HistoricoConversa (Caretaker)"
