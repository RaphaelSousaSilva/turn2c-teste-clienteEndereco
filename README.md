## Descrição do Teste

Criar um CRUD  para a entidade cliente, esta entidade deverá ter os seguintes atributos:

- Nome (varchar),
- Email (chave única) (varchar)
- CPF (varchar)

<br>
Criar um CRUD para cadastrar os endereços do cliente, essa entidade cliente_endereco terá os seguintes atributos:

- Cliente (FK)
- Bairro (varchar)
- Logradouro (varchar)
- Cep   (int)
- Numero (int)
- Cidade (varchar)
- Uf (varchar)

## Tencologias utilizadas

- Maven
- Java 17
- Spring Boot
- Lombok
- Spring data jpa
- Banco de dados H2
- Bean Validation

<br>

## Informações

### Banco de Dados
Estou usando o h2, e deixei configurado para subir a interface gráfica dele em /h2, caso seja necessário checar algo direto no banco.

Também configurei a população do banco com alguns dados na inicialização do sistema.

<br>


### Respostas
Estou devolvendo respostas para as requisições com ResponseEntity e status code de acordo com a requisição.
Também há um ExceptionHandler configurado para algumas exceções, devolvendo mensagens personalizadas.

<br>

## Endpoints:

<br>

### **Cliente endpoints**

- #### GET /cliente (Listar todos os clientes e seus endereços)

- #### POST /cliente (Adicionar novo cliente com corpo da requisição)
    O corpo da requisição deve conter as seguintes informações:

        {

        "nome" : "clienteTeste",
        "email": "cliente@teste.com",
        "cpf" : "12345678",
        "enderecos" : [
            {
                "bairro" : "Bairro Teste",
                "logradouro" : "Logradouro Teste",
                "cep" : "12345678",
                "numero" : "12",
                "cidade" : "cidade teste",
                "uf" : "uf"
            
            },
        ]
        }

<br>

- #### GET /cliente/{id} (Listar cliente por id e seus endereços)
<br>

 - #### PUT /cliente/{id} (Atualizar informações do cliente com corpo da requisição)

    Os campos para atualização são os mesmos usados para cadastro. É possível, também, editar campos individualmente, sem alterar os não informados. Ou seja, se enviarmos a requisição abaixo, o retorno será a entidade salva, somente o campo passado alterado:

    Requisição PUT

        {
        "nome" : "clienteTesteAlterado"
        }

    <br>

    A resposta deverá ser:

        {
            "nome" : "clienteTesteAlterado",
            "email": "cliente@teste.com",
            "cpf" : "12345678",
            "enderecos" : [
                {
                    "bairro" : "Bairro Teste",
                    "logradouro" : "Logradouro Teste",
                    "cep" : "12345678",
                    "numero" : "12",
                    "cidade" : "cidade teste",
                    "uf" : "uf"
                },
            ]
        }



    Entretanto, se algum parametro for passado para **enderecos**, os endereços salvos serão apagados e um novo endereço com as informações passadas será salvo. Seguindo os exemplos antreriores:

    Se tivermos um cliente salvo no banco com as seguintes informações:

        {
            "nome" : "clienteTeste",
            "email": "cliente@teste.com",
            "cpf" : "12345678",
            "enderecos" : [
                {
                    "id" : 1,
                    "bairro" : "Bairro Teste",
                    "logradouro" : "Logradouro Teste",
                    "cep" : "12345678",
                    "numero" : "12",
                    "cidade" : "cidade teste",
                    "uf" : "uf"
                
                },
                {
                    "id" : 2,
                    "bairro" : "Bairro Teste 2",
                    "logradouro" : "Logradouro Teste 2",
                    "cep" : "12345678",
                    "numero" : "12",
                    "cidade" : "cidade teste",
                    "uf" : "uf"
                
                },
            ]
        }

    E enviarmos a seguinte requisição PUT:

        {
            "nome" : "clienteTesteAlterado",
            "email": "cliente@teste.com",
            "cpf" : "12345678",
            "enderecos" : [
                {
                    "id": 1,
                    "bairro" : "Bairro Teste ALTERADO",
                    "logradouro" : "Logradouro Teste",
                    "cep" : "12345678",
                    "numero" : "12",
                    "cidade" : "cidade teste",
                    "uf" : "uf"
                
                },
            ]
        }

    A resposta será:

        {
            "nome" : "clienteTesteAlterado",
            "email": "cliente@teste.com",
            "cpf" : "12345678",
            "enderecos" : [
                {
                    "id": 3,
                    "bairro" : "Bairro Teste ALTERADO",
                    "logradouro" : "Logradouro Teste",
                    "cep" : "12345678",
                    "numero" : "12",
                    "cidade" : "cidade teste",
                    "uf" : "uf"
                
                },
            ]
        }

    Note que um novo id é gerado, e um novo endereço com as informações passadas criadas. O que acontece é que ao enviar informações em **enderecos** nas nossas requisições para UPDATE de usuário, na verdade, no banco de dados um comando DELETE é feito com os enderecos antigos e um INSERT novo é feito.


    Caso queira editar algum endereço, sem apagá-lo do sistema, use o endpoint:<br>
    **PUT /cliente/{idCliente}/endereco/{idEndereco}**

<br>

 - #### DEL /cliente/{id} (Deletar cliente e todos os seus endereços)

 <br>

 ### **Endereco endpoints**

 - #### GET /cliente/{idCliente}/endereco (Listar somente os endereços de um usuário)

<br>

- #### POST /cliente/{idCliente}/endereco (Salvar novo endereço para um cliente)

<br>

- #### GET /cliente/{idCliente}/endereco/{idEndereco} (Listar somente um endereço especifico de um usuário)

<br>

- #### PUT /cliente/{idCliente}/endereco/{idEndereco} (Editar endereco de um cliente)

    Assim como no endpoint de edição de cliente, aqui também é possível editar campos individualmente.

<br>

- #### DEL /cliente/{idCliente}/endereco/{idEndereco} (Deletar endereco de um cliente)

