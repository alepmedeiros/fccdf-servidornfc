# Servidor de geração de arquivos fiscais NFC da Formação Completa em Criação de APIs para Documentos Fiscais

```plaintext
src/
├── domain/
│   ├── entities/
│   │   └── domain.entities.NFCe.pas               # Entidade principal para emissão de NFC-e
│   ├── value-objects/
│   │   └── domain.valueobjects.CNPJ.pas               # Objeto de valor para CNPJ
│   ├── repositories/
│   │   └── domain.repositories.iNFCeRepository.pas     # Contrato para repositório de NFC-e
│   └── services/
│       └── domain.services.NFCeValidationService.pas # Regras de validação de NFC-e
│
├── application/
│   ├── use-cases/
│   │   └── application.usecases.EmitNFCeUseCase.pas    # Caso de uso: Emitir NFC-e
│   └── dto/
│       └── application.dto.NFCeDTO.pas            # DTO para transferência de dados da NFC-e
│
├── infrastructure/
│   ├── persistence/
│   │   └── infrastruture.persistence.NFCeRepositoryImpl.pas # Implementação do contrato de repositório
│   ├── http/
│   │   └── infrastructure.http.Router.pas             # Configuração das rotas HTTP
│   ├── config/
│   │   └── infrastructure.configAppConfig.pas          # Configurações gerais
│   └── database/
│       └── infrastructure.database.Connection.pas         # Gerenciamento de conexão com o banco
│
├── presentation/
│   ├── controllers/
│   │   └── presentation.controllers.NFCeController.pas     # Controller para NFC-e
│   └── middlewares/
│       └── presentation.middlewares.AuthMiddleware.pas     # Middleware de autenticação
│
├── tests/
│   ├── unit/
│   │   └── unit.EmitNFCeUseCaseTest.pas # Teste unitário para o caso de uso
│   └── integration/
│       └── integration.NFCeRepositoryTest.pas # Teste de integração para repositório
│
└── main.dpr                        # Ponto de entrada principal
```

### Explicação Detalhada de Cada Camada

1. **Camada** `domain/`
    
    A camada de domínio encapsula as regras de negócio e abstrações principais. Deve ser independente de frameworks e bibliotecas externas.

* `NFCe.pas` (**Entidade Principal**): Contém os atributos da NFC-e, como `CNPJ`, `Emitente`, `Produtos` e `Total`, com validações básicas.

    * **Testes TDD**:
        * Validação de CNPJ (uso de `CNPJ.pas`).
        * Garantir que todos os campos obrigatórios estejam preenchidos.

* `CNPJ.pas` (**Objeto de Valor**): Implementa a validação de CNPJ como um tipo imutável.

    * **Testes TDD**:
        * Validar se um CNPJ é válido.
        * Retornar erro em caso de CNPJ inválido.

* `NFCeValidationService.pas` (**Serviço de Validação**): Aplica as regras de validação específicas da NFC-e (ex.: consistência dos produtos e valores).

2. **Camada** `application/`

    A camada de aplicação coordena os fluxos de caso de uso e interage com o domínio.

* `EmitNFCeUseCase.pas` (**Caso de Uso**): Responsável por orquestrar a emissão da NFC-e, validando os dados e chamando o repositório para persistência.

    * **Testes TDD**:
        * Garantir que uma NFC-e válida seja emitida com sucesso.
        * Garantir que erros sejam lançados em caso de falhas na validação.

* `NFCeDTO.pas` (**DTO**): Representa os dados de entrada para o caso de uso.

3. **Camada** `infrastructure/`

    Essa camada contém detalhes técnicos e implementações específicas.

* `NFCeRepositoryImpl.pas` (**Repositório**): Implementa o contrato de persistência, interagindo com o banco de dados para salvar as NFC-e emitidas.

    * **Testes TDD**:
        * Verificar se os dados são salvos corretamente no banco.
        * Validar falhas de conexão ou inconsistências de dados.

4. **Camada** `presentation/`

    A camada de apresentação expõe a API HTTP para interação com a aplicação.

* `NFCeController.pas` (**Controller**): Recebe requisições, valida entradas e encaminha para o caso de uso.

    * **Testes TDD**:
        * Garantir que os endpoints retornem os códigos HTTP corretos.
        * Validar as respostas com base nas entradas fornecidas.

