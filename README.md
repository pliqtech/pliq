# pliq-api
https://www.pliq.io

Esse é um repositório para auxílio na API da PliQ - Plataforma de Gestão de Feedbacks.

Colocaremos materiais e webhooks para que a integração seja facilitada.
Nota: Qualquer erro ou sugestão, por favor nos contate.

## Instruções para realizar a integração

### URL de acesso

https://api.pliq.io/v2/api (Para clientes ambiente de produção)

ou

https://sandbox-api.pliq.io/v2/api (Para ambiente de sandbox)

## Requisição HTTP
A API da PliQ utiliza o padrão REST através do protocolo HTTPS. Os métodos variam de acordo com a função utilizada:

Método | Ação
------------ | ------------- 
GET | lista ou consulta dados 
POST | criação de dados 
PUT / PATCH | atualização de dados 
DELETE | remoção de dados 

Dica: quando estiver listando, você pode escolher os campos que deseja trazer enviando o parâmetro "attribute" na URL.

### Como autenticar
É necessário passar o token privado de autenticação para conseguir realizar as operações. Para conseguir esse token, acesse na PliQ a seção Integrações -> Tokens.

### Códigos de respostas

Código | Status | Descrição
------------ | ------------- | -------------
200 | OK | Sua requisição foi bem sucedida
400 | Bad Request | Algum parâmetro obrigatório não foi enviado ou é inválido. Neste caso a própria resposta indicará qual é o problema.
401 | Unauthorized | Não foi enviada Napikey ou ela é inválida. Verifique as credenciais de autenticação da requisição.
404 | Not Found | O endpoint ou o objeto solicitado não existe.
403 | Forbidden | Requisição não autorizada ou uso de parâmetros não permitidos podem gerar este código.
429 | Too Many Requests | Muitas requisições em um determinado período de tempo.
500 | Internal Server Error | Algo deu errado no servidor.

### Padrão de endpoints

```
Para listagem, use GET: /endpoint/
Para inserção, use POST: /endpoint/
```

## Header
A requisição deve conter no Header:

* Content-Type: application/json
* PLIQ_TOKEN: SEU_TOKEN_AQUI

### Seções (endpoints) disponíveis

Segue as seções que você pode acessar pela API

### Listar Pesquisas

* O método usado será : 
> GET
* A URL usada será : 
> <url_acesso>/surveys

Parâmetros opcionais de cabeçalho, ?survey_code=&showquestionsdeleted=

Para listar as pesquisas utilizaremos a seguinte configuração.

Esse método é responsável por listar todas as pesquisas, seus códigos e alguns detalhes, como por exemplo as perguntas usadas.

Parâmetros de cabeçalho

Parâmetro | Tipo | Obrigatório | Descrição
------------ | ------------- | ------------ | -------------
survey_code | String | Sim | Chave da pesquisa.
showquestionsdeleted | Bool | Não | Em caso de true o endpoint irá mostrar as perguntas que foram deletadas.

##### Retorno

```
[
    {
        "title": "survey_title",
        "description": "survey_description",
        "code": "survey_code",
        "active": "survey_active",
        "createdat": "survey_createdat",
        "questions": [
            {
                "title": "question_title",
                "sequence": "question_sequence",
		"id_question": "question_id_question",
		"fk_type_question": "question_fk_type_question",
		"low_score_label": "question_low_score_label",
		"high_score_label": "question_high_score_label",
		"required": "question_required",
		"title_neutral": "question_title_neutral",
		"title_detractor": "question_title_detractor",
		"answers": "question_answers",
		"deletedat": "deletedat",
		"answersOBJ": "question_answersOBJ"
            }
        ]
    }
] 
```

### Listar os tipos de perguntas
Fornecemos também um método que lista os tipos de perguntas.

* O método usado será : 
> GET
* A URL usada será : 
> <url_acesso>/typequestions

Exemplo de retorno:
```
{
    "id_type_question": "Chave primária do tipo de pergunta.",
    "title": "Título do tipo de pergunta",
    "type_object": "Estrutura de objetos html do tipo de pergunta.",
    "answers_default": "Lista possíveis respostas padrões do tipo de pergunta.",
    "notapply": "Boleano para identificar se o tipo de pergunta permite ativar a opção N/A (Não se Aplica).",
    "notapply_label": "Texto padrão usado para a opção N/A (Não se Aplica)."    
}
```

### Visualizar Respostas em Lote
Fornecemos também um método de visualização de respostas em lote por período. Esse método pode ser utilizado com os seguintes parâmetros.

O método será: GET

A URL a ser usada deve ser: __<url_acesso>/surveys/feedbacks__

Parâmetros opcionais de cabeçalho, ?survey_code=&days=&startedat=&finishedat=&withcontact=	

Parâmetros header

Parâmetro | Tipo | Obrigatório | Descrição
------------ | ------------- | ------------ | -------------
pliq_token | String | Sim | Chave do token da empresa. Obtida na tela de integrações.
	
Parâmetros de cabeçalho

Parâmetro | Tipo | Obrigatório | Descrição
------------ | ------------- | ------------ | -------------
survey_code | String | Sim | Chave da pesquisa. Obtida no método __<url_acesso>/surveys/all__.	
days | String | Não* | Quantidade de dias da resposta com base no dia atual.
startedat | String | Não* | Data de início de respostas.
finishedat | String | Não* | Data de final de respostas.
withcontact | Boolean | Não* | Ao ativar com true o endpoint inclui os dados do contato que deixou a resposta.

	
Exemplo de retorno:
```
[
    {
        "id_company": "company_id",
        "full_name": "company_fullname",
        "id_survey": "survey_id",
        "url_survey": "survey_code",
        "type_survey": "type_survey",
        "type_survey_label": "type_survey_label",
        "fk_survey_sample": "type_survey_sample",
        "title": "survey_title",
        "description": "survey_description",
        "id_survey_response": "survey_response_id",
        "participant_key": "participant_key",
        "abandonment": "abandonment",
        "closed_loop": "closes_loop",
        "name": "contact_name",
        "name_key": "contact_name_key",
        "respondedat": "survey_response_data",
        "createdat": "survey_response_created",
        "sendedat": "survey_response_send",
        "tags": "survey_response_tags",
		"tagsList": {
			"Tags": [
				"TagA",
				"TagB"
			]
		},
        "referral_share_code": "referral_share_code",
        "nps_value": "nps_value",
        "nps_feedback": "nps_feedback",
	"anonymous": "anonymous",
	"timezone","timezone",
"key_attendance": "key_attendance",
		"comments": "comments",
"properties": "[id_property, value]",
	"responses": "[fk_question, value, fk_type_question, answers, data_response]",
	"contact": {
		"id_contact": 50450,
		"first_name": "My first Name",
		"last_name": "My Last Name",
		"register_number": "111.222.333-44",
		"phone": "+551199889988",
		"phone_extra": "+551134445532",
		"email": "meuemail@minhaempresa.com",
		"website": null,
		"code_contact_client": null,
		"segment": null,
		"country": null,
		"region": null,
		"state": null,
		"city": null,
		"neighborhood": null,
		"address": null,
		"cep": null,
		"complement": null,
		"properties": [{
			"id_property": "id_property",
			"name_property": "Name of property",
			"internal_name": "name_property",
			"value": "Value of property"
		}]
	}
    }
]
```
	
### Base legal
Em conformidade com a LGPD nossa API de importação de contatos solicita que o nosso cliente informe qual é a base legal que está sendo utilizada.

Em resumo, as chamadas bases legais, são por que sua empresa poderá realizar o tratamento dos dados pessoais de terceiros. Conhecê-las é uma forma de entender como a LGPD funciona e como as organizações e negócios sociais precisam se adaptar para atender às determinações da Lei. 

Código | Base Legal | Detalhes
------------ | ------------- | -------------
0 | Consentimento | Quando uma pessoa consente com o tratamento dos seus dados, de forma livre, inequívoca e informada.
1 | Legítimo interesse | Quando a empresa possui um interesse legítimo para o processamento e a utilização dos dados está dentro das expectativas da pessoa, não sendo necessário obter consentimento.
2 | Contrato pré-existente | Quando os dados pessoais são processados por obrigação contratual ou para a validação e início de vigência de um acordo.
3 | Não sei dizer/Não possui base legal | Quando o tratamento de dados pessoais é justificável por Lei.
4 | Obrigação Legal, Processo Judicial ou Proteção ao crédito | Quando o tratamento de dados pessoais é feito com o intuito de proteger a vida, seja do titular do dado ou ainda de outra pessoa.
5 | Interesse vital ou Tutela da saúde | Muitas requisições em um determinado período de tempo.
6 | Interesse público | Quando o tratamento de dados pessoais é resguardado pelo interesse público ou por necessidade de uma autoridade oficial.

### Update Import
Em casos onde o contato já exista na base de dados de sua empresa o que deverá acontecer.

Em resumo, podem ser ignoradas ou atualizadas as novas informações no contato.

Código | Base Legal | Detalhes
------------ | ------------- | -------------
1 | Ignorar | Ignorar o novo registro e manter o existente intacto.
2 | Atualizar | Atualizar o contato existente com as mudanças ou novos campos.

# Importar contatos
Fornecemos também um método que importa contatos e já envia para uma jornada ou realiza agendamento da pesquisa para a listagem.

* O método usado será : 
> POST
* A URL usada será : 
> <url_acesso>/import

Parâmetros header

Parâmetro | Tipo | Obrigatório | Descrição
------------ | ------------- | ------------ | -------------
pliq_token | String | Sim | Chave do token da empresa. Obtida na tela de integrações.
	
Parâmetros Body

Parâmetro | Tipo | Obrigatório | Descrição
------------ | ------------- | ------------ | -------------
nameImport | String | Sim | Nome da importação, descreva com um título a listagem dessa pesquisa.
url_survey | String | Não | Chave da pesquisa. Obtida no método __<url_acesso>/surveys__.
update_import | Int | Sim | Informe o que fazer em caso de contato duplicado conforme instruções acima.
baseLegal | Int | Sim | Informe a base legal conforme instruções acima.
date_scheduling | String | Não | Em caso de agendamento da pesquisa, informar quando será a data e hora do envio.
fk_journey| Int | Não | Chave da jornada automatizada que os contatos serão incluídos. [Solicitar a tech da pliq o ID]
Customers | List[Customer] | Sim | Lista de objetos do tipo contatos.
flg_email | Boolean | Não | Identifica se será utilizado o envio de pesquisa no canal de email.
flg_popup | Boolean | Não | Identifica se será utilizado o envio de pesquisa no canal de popup.
flg_whatsapp | Boolean | Não | Identifica se será utilizado o envio de pesquisa no canal de whatsapp.
flg_sms | Boolean | Não | Identifica se será utilizado o envio de pesquisa no canal de sms.

Objeto Customer

Parâmetro | Tipo | Obrigatório | Descrição
------------ | ------------- | ------------ | -------------
name | String | Sim | Nome da importação, descreva com um título a listagem dessa pesquisa.
email | String | Sim* | Email do contato. Em caso de mais de um email separar por ";"
phone | String | Sim* | Telefone do contato, usar padrão internacional com DDI. Ex. 5511988887777. Em caso de mais de um email separar por ";"
identification | String | Não | Número de identificação do contato em seu CRM ou ERP.
tagsCustomer | Array | Não | Informe a listagem das Tags.
segment | String | Não | Nome do segmento que o contato pertence.
enterprise | String | Não | Nome da empresa do contato.
register_number | String | Não | Número de CPF do contato.
comments | String | Não | Comentários extras do atendimento.
key_attendance | String | Não | Número de um atendimento/protocolo que o contato foi atendido.
ticket_media | Decimal | Não | Ticket médio do contato mensal.
start_contract | String | Não | Data de início do primeiro contrato do contato.
last_name | String | Não | Sobrenome do contato.
country | String | Não | País do endereço do contato.
region | String | Não | Região do endereço do contato.
city | String | Não | Cidade do endereço do contato.
neighborhood | String | Não | Bairro do endereço do contato.
address | String | Não | Rua/Avenida do endereço do contato.
cep | String | Não | Cep do endereço do contato.
complement | String | Não | Complemento do endereço do contato.
number | String | Não | Número do endereço do contato.
profile_linkedin | String | Não | Link para o linkedin do contato.
profile_facebook | String | Não | Link para o facebook do contato.
profile_twitter | String | Não | Link para o twitter do contato.
profile_instagram | String | Não | Link para o instagram do contato.

Exemplo de Body:
```
{
   "nameImport":null,
   "url_survey":null,
   "update_import":null,
   "baseLegal":null,
   "date_scheduling":null,
   "fk_journey":null,
   "customers":[
      {
         "name":null,
         "email":null,
         "phone":null,
         "identification":null,
         "state":null,
         "tagsCustomer":null,
         "segment":null,
         "enterprise":null,
         "register_number":null,
         "comments":null,
         "key_attendance":null,
         "ticket_media":null,
         "start_contract":null,
         "last_name":null,
         "country":null,
         "region":null,
         "city":null,
         "neighborhood":null,
         "address":null,
         "cep":null,
         "complement":null,
         "number":null,
         "profile_linkedin":null,
         "profile_facebook":null,
         "profile_twitter":null,
         "profile_instagram":null
      }
   ],
   "flg_email":false,
   "flg_popup":false,
   "flg_whatsapp":false,
   "flg_sms":false
}
```

Exemplo de retorno:
```
{
	"message": "Import being carried out!"
}
```

### Objetos
Listagem dos objetos que possuem propriedades.

Código | Nome | Detalhes
------------ | ------------- | -------------
1 | Local | Ignorar o novo registro e manter o existente intacto.
2 | Pesquisa | Propriedades do objeto Pesquisa.
3 | Parceiro | Propriedades do objeto Parceiro [Vendas por indicação].
4 | Resposta | Propriedades do objeto Resposta.
5 | Contato | Propriedades do objeto Contato.

### Tipos de propriedades
Listagem dos tipos de propriedades.

Código | Nome | Detalhes
------------ | ------------- | -------------
1 | Texto | Propriedade que captura dados no formato de texto.
2 | Boleano | Propriedade que captura dados no formato boleano [True/False].
3 | Número | Propriedade que captura dados no formato de número.
4 | Data | Propriedade que captura dados no formato de data.
5 | Data e Hora | Propriedade que captura dados no formato de data e hora.

### Listar as propriedades da company [Property]
Fornecemos também um método que lista as propriedades dos objetos da sua empresa.

* O método usado será : 
> GET
* A URL usada será : 
> <url_acesso>/properties

Parâmetros opcionais de cabeçalho, ?fk_object=

Parâmetros de cabeçalho

Parâmetro | Tipo | Obrigatório | Descrição
------------ | ------------- | ------------ | -------------
fk_object | Int | Não | Chave da objeto.

Exemplo de retorno:
```
[
	{
		"id_property": "id_property",
		"fk_type_property": "fk_type_property",
		"fk_company": "fk_company",
		"fk_object": "fk_object,
		"label": "label",
		"description": "description",
		"group_name": "group_name",
		"internal_name": "internal_name",
		"read_only": "read_only",
		"sequence": "sequence"
	}
]
```

### Inserir Resposta
Fornecemos também um método de inserção de resposta. Esse método pode ser utilizado com os seguintes parâmetros.

* O método usado será : 
> POST
* A URL usada será : 
> <url_acesso>/answer

Parâmetros header

Parâmetro | Tipo | Obrigatório | Descrição
------------ | ------------- | ------------ | -------------
pliq_token | String | Sim | Chave do token da empresa. Obtida na tela de integrações.
	
Parâmetros Body

Parâmetro | Tipo | Obrigatório | Descrição
------------ | ------------- | ------------ | -------------
url_survey | String | Sim | Chave da pesquisa. Obtida no método __<url_acesso>/surveys__.
name | String | Sim* | Nome do contato que respondeu a pesquisa.
participant_key | String | Sim* | Email ou telefone do contato que respondeu a pesquisa.
responses | Array[Response] | Sim | Lista das respostas das perguntas da pesquisa.
tags | Array[String] | Não | Informe a listagem das Tags.
respondedat | String | Sim | Data/hora da resposta no padrão timestamp.
anonymous | Boolean | Não | Identifica se a resposta será anônima.
properties | Array[Property] | Não | Lista das propriedades do objeto [Pesquisa] para essa resposta.

Objeto Response

Parâmetro | Tipo | Obrigatório | Descrição
------------ | ------------- | ------------ | -------------
fk_question | Integer | Sim | Chave que identifica a pergunta da pesquisa.
value | String | Sim | Valor da resposta do cliente.
fk_type_question | Integer | Sim | Chave que identifica o tipo de pergunta.

Objeto Property

Parâmetro | Tipo | Obrigatório | Descrição
------------ | ------------- | ------------ | -------------
id_property | Integer | Sim | Chave que identifica a propriedade.
value | String | Sim | Valor da propriedade.


```
{
    "url_survey": "url_survey",
    "name": "name",
    "participant_key": "participant_key",
    "responses": [
        {
            "fk_question": fk_question,
            "value": "value",
            "fk_type_question": fk_type_question
        }
    ],
    "tags": ["tags1", "tags2", "tagsN"],
    "respondedat": "respondedat",
    "anonymous": anonymous,
    "properties": [
        {
            "id_property": "id_property",
            "value": "value"
        }
    ]
}
```

Exemplo de retorno: 200
```
{
	"message": "Answer saved successfully!"
}
```


Exemplo de retorno [Erro]: Código da question ou do fk_type_question não localizado.
```
{
	"error": "Responses not populated correctly: non-existent fk_question / fk_type_question."
}
```
