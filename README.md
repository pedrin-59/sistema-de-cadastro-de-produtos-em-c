# Sistema de Cadastro de Produtos

Aplicação desktop para Windows, escrita em C com a API Win32, que permite cadastrar produtos e listar os produtos já cadastrados. Os dados são salvos em um arquivo de texto.

## Funcionalidades

- **Cadastrar**: salva um novo produto (código, nome, quantidade e preço).
- **Limpar**: limpa todos os campos do formulário.
- **Listar**: exibe todos os produtos cadastrados em uma caixa de mensagem.
- **Sair**: fecha a aplicação.

## Estrutura de dados

Cada produto é representado pela estrutura:

```c
typedef struct {
    int codigo;
    char nome[100];
    int quantidade;
    float preco;
} Produto;
```

## Armazenamento

Os produtos são gravados no arquivo `produto_interface.txt`, criado automaticamente na pasta de onde o programa é executado. Cada linha segue o formato:

```
codigo;nome;quantidade;preco
```

Exemplo:

```
101;Teclado;15;89.90
```

## Requisitos

- Sistema operacional Windows
- Compilador C com suporte à API Win32 (Visual Studio/MSVC ou MinGW/GCC)

## Como compilar e executar

### Com MinGW (GCC)

```bash
gcc main.c -o produtos.exe -mwindows
./produtos.exe
```

### Com Visual Studio (MSVC)

Abra o projeto ou compile pelo "Developer Command Prompt":

```bat
cl main.c user32.lib
main.exe
```

## Observação sobre `#pragma comment(lib, "user32.lib")`

No código existe a linha:

```c
#pragma comment(lib, "user32.lib")
```

Ela serve **apenas para o ambiente do VS Code com o compilador MSVC**, instruindo o linker a incluir a biblioteca `user32.lib`, que contém funções como `CreateWindow`, `MessageBox` e `SetWindowText`.

- Com **MSVC**, essa linha resolve o link automaticamente, dispensando configurar a biblioteca manualmente.
- Com **MinGW/GCC**, a diretiva é ignorada (o compilador pode exibir um aviso sobre pragma desconhecido). Nesse caso, `user32` já é incluída por padrão, então a linha pode ser removida sem prejuízo.
- Se a linha for removida ao usar MSVC, é necessário passar `user32.lib` manualmente na linha de compilação, como mostrado acima.

## Estrutura da interface

| Elemento | Função |
|---|---|
| Campo Código | Aceita apenas números (`ES_NUMBER`) |
| Campo Nome | Texto livre, até 99 caracteres |
| Campo Quantidade | Aceita apenas números (`ES_NUMBER`) |
| Campo Preço | Texto, convertido para número decimal |

## Limitações conhecidas

- A listagem usa um buffer fixo de 5000 caracteres, então muitos produtos podem ultrapassar o limite.
- O campo de preço aceita texto livre, e valores inválidos são convertidos para `0`.
- Não há verificação de código duplicado.
- Não há opção de editar ou excluir produtos.
