# Especificações técnicas do sistema de gerenciamento de Propriedade Intelectual

## Interface Administrativa

Deverá ser implementada uma interface administrativa que permita aos administradores de propriedade intelectual cadastrar informações sobre propriedades intelectuais e gerenciar registros de direitos autorais. Para isso, utilize uma ferramenta de geração de interface administrativa automática, como o Django Admin (consulte a [documentação](https://docs.djangoproject.com/en/4.0/ref/contrib/admin/)).

A interface administrativa deve conter as seguintes funcionalidades:

### Cadastrar Propriedade Intelectual

Permita o cadastro de informações sobre propriedades intelectuais, incluindo:

- **Título:** Título da propriedade intelectual (obrigatório)
- **Tipo de Propriedade:** Categoria ou tipo da propriedade intelectual (patente, marca, desenho industrial) (obrigatório)
- **Número de Registro:** Número de registro da propriedade intelectual (obrigatório)
- **Descrição:** Descrição da propriedade intelectual (opcional)
- **titular:** ICT titular da propriedade intelectual (obrigatório, pode ser mais de uma)


#### Restrições:

- Não deve ser possível cadastrar propriedades intelectuais com o mesmo número de registro
- Garanta que o número de registro seja exclusivo para cada tipo de propriedade intelectual
- Não pode ser cadastrado um tipo de propriedade intelectual diferente dos já definidos 


### Cadastrar ICT

Permita o cadastro de informações sobre instituições de ciência e tecnologia titulares das patente, incluindo:

- **CNPJ:** CNPJ do titular (obrigatório)
- **Razão Social:** Razão social (obrigatório)
- **Abreviatura:** Abreviação do razão social (obrigatório)


#### Restrições:

- Não deve ser possível cadastrar ict com o mesmo cnpj de uma já cadastrada

### Gerenciar Direitos Autorais

Permita o gerenciamento de direitos autorais relacionados às propriedades intelectuais, incluindo a criação e edição de registros de direitos autorais. Cada registro de direitos autorais deve conter as seguintes informações:

- **Propriedade Intelectual:** A propriedade intelectual à qual os direitos autorais estão associados (obrigatório)
- **Data de Registro:** Data de registro dos direitos autorais (obrigatório)
- **Autor:** Nome do autor dos direitos autorais (obrigatório)
- **Detentor dos Direitos:** Nome da ICT detentor dos direitos autorais (obrigatório)
- **Descrição:** Descrição dos direitos autorais (opcional)

#### Restrições:

- Não deve ser possível criar mais de um registro de direitos autorais para a mesma propriedade intelectual na mesma data
- Não deve ser possível criar registros de direitos autorais para propriedades intelectuais que não existam no sistema

## :gear: API

Deverá ser construída uma API seguindo os padrões e boas práticas REST, que permita a interação com as informações sobre propriedade intelectual e direitos autorais. A API deve ser privada e requerer autenticação para acessar os seguintes endpoints:

### Listar Propriedades Intelectuais

Lista todas as propriedades intelectuais registradas.

#### Requisição

```
GET /propriedades_intelectuais/
```

#### Retorno

```json
[
  {
    "id": 1,
    "titulo": "Invenção Revolucionária",
    "tipo_propriedade": "Patente",
    "numero_registro": "PT123456",
    "descricao": "Uma invenção que vai mudar o mundo",
    "titular": [
                {
                  "cnpj": 3423495320001,
                  "razao_social": "Universidade Federal de Minas Gerais",
                  "abvt": "UFMG"
                },
                {
                  "cnpj": 639574860001,
                  "razao_social": "Instituto de Ciência e Tecnologia do Piauí",
                  "abvt": "IFPI"
                }
    ]
  },
  {
    "id": 2,
    "titulo": "Obra Literária",
    "tipo_propriedade": "Direitos Autorais",
    "numero_registro": "DA987654",
    "descricao": "Um romance emocionante",
    "titular": [
                {
                  "cnpj": 639574860001,
                  "razao_social": "Instituto de Ciência e Tecnologia do Piauí",
                  "abvt": "IFPI"
                }
    ]
  }
]
```

### Listar ICT

Lista todas as ict's registradas.

#### Requisição

```
GET /icts/
```

#### Retorno

```json
[
  {
    "cnpj": 3423495320001,
    "razao_social": "Universidade Federal de Minas Gerais",
    "abvt": "UFMG"
  },
  {
    "cnpj": 639574860001,
    "razao_social": "Instituto de Ciência e Tecnologia do Piauí",
    "abvt": "IFPI"
  }
]
```

### Listar Direitos Autorais

Lista todos os registros de direitos autorais registrados.

#### Requisição

```
GET /direitos_autorais/
```

#### Retorno

```json
[
  {
    "id": 1,
    "propriedade_intelectual": {
        "id": 2,
        "titulo": "Obra Literária",
        "tipo_propriedade": "Direitos Autorais",
        "numero_registro": "DA987654",
        "descricao": "Um romance emocionante",
        "titular": [
                    {
                      "cnpj": 639574860001,
                      "razao_social": "Instituto de Ciência e Tecnologia do Piauí",
                      "abvt": "IFPI"
                    }
        ]
    },
    "data_registro": "2022-03-15",
    "autor": "Inventor Genial",
    "detentor_direitos": [{
        "cnpj": 639574860001,
        "razao_social": "Instituto de Ciência e Tecnologia do Piauí",
        "abvt": "IFPI"
      } ],
    "descricao": "Direitos autorais sobre a invenção"
  }
]
```

### Registrar Propriedade Intelectual

Registra uma nova propriedade intelectual.

#### Requisição

```
POST /propriedades_intelectuais/
{
  "titulo": "Nova Obra Musical",
  "tipo_propriedade": "Direitos Autorais",
  "numero_registro": "DA123789",
  "descricao": "Uma nova composição musical",
  "ict": "639574860001"
}
```

#### Retorno

```json
{
  "id": 3,
  "titulo": "Nova Obra Musical",
  "tipo_propriedade": "Direitos Autorais",
  "numero_registro": "DA123789",
  "descricao": "Uma nova composição musical",
  "proprietario": [
                    {
                      "cnpj": 639574860001,
                      "razao_social": "Instituto de Ciência e Tecnologia do Piauí",
                      "abvt": "IFPI"
                    }
        ]
}
```

### Registrar Direitos Autorais

Registra um novo conjunto de direitos autorais.

#### Requisição

```
POST /direitos_autorais/
{
  "propriedade_intelectual_id": 1,
  "data_registro": "2022-04-20",
  "autor": "Autor Experiente",
  "detentor_direitos": "639574860001",
  "descricao": "Direitos autorais sobre a invenção aprimorada"
}
```

#### Retorno

```json
{
  "id": 4,
  "propriedade_intelectual": {
    "id": 1,
    "titulo": "Invenção Revolucionária",
    "tipo_propriedade": "Patente",
    "numero_registro": "PT123456",
    "descricao": "Uma invenção que vai mudar o mundo",
    "proprietario": [
                    {
                      "cnpj": 639574860001,
                      "razao_social": "Instituto de Ciência e Tecnologia do Piauí",
                      "abvt": "IFPI"
                    }
        ]
  },
  "data_registro": "2022-04-20",
  "autor": "Autor Experiente",
  "detentor_direitos": [
                    {
                      "cnpj": 639574860001,
                      "razao_social": "Instituto de Ciência e Tecnologia do Piauí",
                      "abvt": "IFPI"
                    }
        ],
  "descricao": "Direitos autorais sobre a invenção aprimorada"
}
```

### Editar Propriedade Intelectual

Permite a edição das informações de uma propriedade intelectual existente.

#### Requisição

```
PUT /propriedades_intelectuais/<propriedade_intelectual_id>/
{
  "titulo": "Obra Musical Atualizada",
  "descricao": "Uma versão atual

izada da composição musical"
}
```

#### Retorno

```json
{
  "id": 1,
  "titulo": "Obra Musical Atualizada",
  "tipo_propriedade": "Direitos Autorais",
  "numero_registro": "DA123789",
  "descricao": "Uma versão atualizada da composição musical",
  "proprietario": [
                    {
                      "cnpj": 639574860001,
                      "razao_social": "Instituto de Ciência e Tecnologia do Piauí",
                      "abvt": "IFPI"
                    }
        ]
}
```

### Excluir Propriedade Intelectual

Permite excluir uma propriedade intelectual e seus registros de direitos autorais associados.

#### Requisição

```
DELETE /propriedades_intelectuais/<propriedade_intelectual_id>/
```

#### Retorno

Não há retorno (vazio)

### Excluir Direitos Autorais

Permite excluir um registro de direitos autorais.

#### Requisição

```
DELETE /direitos_autorais/<direitos_autorais_id>/
```

#### Retorno

Não há retorno (vazio)

### Filtros

A API deve permitir a filtragem de propriedades intelectuais e direitos autorais com base em vários critérios, incluindo:

- Tipo de propriedade intelectual
- Proprietário
- Autor
- Detentor dos direitos
- Data de registro

Exemplo de filtro:

```
# Retorna todas as propriedades intelectuais do tipo "Patente" de um determinado proprietário
GET /propriedades_intelectuais/?tipo_propriedade=Patente&proprietario=Empresa Inovadora Ltda.
```

### Regras de Negócio

- As propriedades intelectuais não devem ter o mesmo número de registro para o mesmo tipo de propriedade.
- Os registros de direitos autorais não devem ser duplicados para a mesma propriedade intelectual na mesma data.
- Não deve ser possível criar registros de direitos autorais para propriedades intelectuais que não existam no sistema.
- As propriedades intelectuais e os registros de direitos autorais devem ser listados em ordem crescente de acordo com a data de registro.
- Propriedades intelectuais e registros de direitos autorais para datas passadas devem ser excluídos da listagem.
- As informações de propriedade intelectual e registros de direitos autorais devem ser atualizadas com precisão quando solicitadas.
- A exclusão de uma propriedade intelectual deve incluir a exclusão de todos os registros de direitos autorais associados a ela.
- Não deve ser possível excluir um registro de direitos autorais que nunca foi criado ou que já ocorreu.
- A rotas GET devem ser paginadas.


### :clock1: Extras

Estes itens não são obrigatório para a entrega do desafio, mas seria interessante se você conseguisse fazer dentro do prazo estipulado com o seu recrutador

##### Integração com Trello

- Possuir testes unitários
- Utilizar Mysql ao invés do sqlite
