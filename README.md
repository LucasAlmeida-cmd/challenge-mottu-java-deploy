# DOCUMENTAÇÃO CHALLENGE - JAVA

Este projeto é referente nossa solução para a empresa **Mottu**.


## Proposta do projeto: 

  O projeto MotoFindr surge para resolver um desafio crítico enfrentado pela Mottu: a dificuldade de gerenciar e localizar motos dentro dos pátios 
devido à imprecisão do GPS em espaços curtos e à falta de um sistema eficiente de registro. Atualmente, as entradas e saídas das motos são registradas 
de forma simplificada (apenas com dados como chassi, placa e data), o que gera desorganização, ineficiência operacional e até riscos de perdas.

 Para esse sprint nós criamos uma API REST, para fazer o CRUD dos: Motoqueiros, Pátios, Seções, Vagas e Motos , para que possamos chegar a nossa solução do projeto, 
 que seria um sistema de localização indoor baseado em triangulação de antenas Wi-Fi. Cada moto possui um módulo IoT (como um ESP32) fixado em um local de difícil acesso, 
 garantindo que não seja removido facilmente. Ao entrar no pátio, a moto é rastreada por meio de múltiplas antenas Wi-Fi posicionadas estrategicamente, formando uma malha de cobertura total. 
 Quando um usuário busca uma moto no sistema, o MotoFindr identifica quais antenas estão recebendo o sinal do dispositivo IoT, 
 calcula a intersecção desses sinais e determina a posição com precisão de 1 a 3 metros, exibindo-a em um mapa digital do pátio.


O sistema permite:
- Cadastrar Motoqueiros, Pátios, Seções, Vagas e Motos
- Consultar todas as entidades
- Editar todas as entidades
- Integração com banco de dados Oracle (SQL Developer)
- Testes de endpoints via Postman

## Nome Integrantes
<div align="center">

| Nome | RM |
| ------------- |:-------------:|
| Arthur Eduardo Luna Pulini|554848|
|Lucas Almeida Fernandes de Moraes| 557569     |
|Victor Nascimento Cosme|558856|

</div>

## Informações
**1-** 
No arquivo: *application.properties*, peço que adicione no campos: 
- *URL: jdbc:sqlserver://{nome do servidor}.database.windows.net:1433;database={nome da base de dados};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;*
- *username: Início de sessão de administrador do servidor*
- *password: FIAP@2tdspy2025*
- *driver-class-name: com.microsoft.sqlserver.jdbc.SQLServerDriver*
  
Credenciais do banco Azure SQL para persistência em banco se não ocorrerá um erro. 

**2-**
Peço que verifique as informações pois as que eu coloquei a baixo são exemplos e podem não funcionar com os mesmos valores.

**3-**
Nos pacotes do projeto onde esta o *records_DTOs*, são as classes DTOs, mas eu usei records, foi conversado em sala sobre e o senhor autorizou.

**4-**
No pom.xml deve ser adicionada a dependência:
```bash
  	<dependency>
			<groupId>com.microsoft.sqlserver</groupId>
			<artifactId>mssql-jdbc</artifactId>
			<version>13.2.0.jre11</version>
			<scope>compile</scope>
		</dependency>
```

## Deploy

**1-**
Começamos fazendo o primeiro build da apicação para seja gerada a pasta target e o arquivo war:
```bash
    mvn package -DskipTests
```

**2-**
Em seguida vamos até a pasta raiz e clicamos com o botão direito e localizamos a sessão Azure.

**3-**
Na sessão da Azure entramos em *Deploy to Azure Web Apps*

**4-**
Em Deploy to Azure acessamos *Web App* clicamos em + e dedicamos um nome para o nosso web app. Em plataform precisamos escolher a opção *Windows-Java 17-Tomcat 10.1*

**5-**
Clicamos em *apply* e em seguida damos o *run*

**6-**
Assim que o deploy é finalizado, no canto direito inferior vai aparecer um pop-up e é necessário clicar em *deploy*

**7-**
Com o deploy finalizado, aparecerá a url que conseguimos usar o web app



## 🚀 Começando

⚙️ Executando os testes via *POSTMAN*

Os testes foram realizados via postman a seguir vou falar dos testes realizados em todas as entidades, seguindo o *CRUD* *(Create, Read, Update e Delete)*.
Aconselho seguir a ordem de criação abaixo pois se fizer em ordem aleatória poderá gerar erro.

# Motoqueiro:

Essa entidade tem alguns pontos, ele precisa ter um CPF válido, por isso eu índico esse site para gerar CPF, somente para testes.

```
https://www.4devs.com.br/gerador_de_cpf
```

## Create

Para criar ele tem que está no método *POST* e o comando para criar foi: 

```bash
  http://[endpoint do web app]/motoqueiro
```

E um corpo para persistência de exemplo: 

```bash
{
    "nomeUser": "Lucas Almeida",
    "dataAniversario": "2002-06-11",
    "cpfUser": "818.835.630-11",
    "cnh": "123456789",
    "endereco":{
        "cep": "02241140"
    }
}
```

## Read

Lembrando que o método é o *GET*.

```bash
  http://[endpoint do web app]/motoqueiro
```

E um retorno: 

```bash
[
    {
        "nomeUser": "Lucas Almeida",
        "dataAniversario": "2002-06-11T03:00:00.000+00:00",
        "cpfUser": "81883563011",
        "id": 44,
        "cnh": "123456789",
        "endereco": {
            "cep": "02241-140",
            "logradouro": "Rua Carmina Pasqui",
            "complemento": null,
            "bairro": "Vila Dom Pedro II",
            "localidade": "São Paulo",
            "uf": "SP"
        },
        "cpfUserFormatado": "818.835.630-11"
    }
]
```

## Update

Para fazer a atualização ele tem que está no método *PUT*, nele tem que passar pela URL um *CPF* que serve para localizar o motoqueiro 
que vai ser modificado e um corpo que vai ser o novo motoqueiro, exemplo de chamada: 

Na URL tem dois jeitos de passar o CPF:

1-
```bash
  http://[endpoint do web app]/motoqueiro/818.835.630-11
```

2-
```bash
  http://[endpoint do web app]/motoqueiro/81883563011
```


No corpo: 
```bash
{
    "nomeUser": "NomeAtualizado1",
    "dataAniversario": "2002-06-11",
    "cpfUser": "818.835.630-11",
    "cnh": "123456789",
    "endereco":{
        "cep": "02201-002"
    }
}
```

## Delete

Para fazer o *Delete* o método que ele tem que estar é o *DELETE*,  nele tem que passar pela URL um *CPF* que serve para localizar o motoqueiro que vai ser deletado, exemplo: 

```bash
  http://[endpoint do web app]/motoqueiro/818.835.630-11
```

E nele não vai ter resposta, só se ele não achar o motoqueiro com esse *CPF*.

---

# Patio:


## Create

Para criar ele tem que está no método *POST*, da e o comando para criar foi: 

```bash
  http://[endpoint do web app]/patio
```

E um corpo para persistência de exemplo: 

```bash
{
    "identificacao": "Patio 1",
    "largura": 100.00,
    "comprimento": 12344.00
}
```

## Read

Lembrando que o método é o *GET*.

```bash
  http://[endpoint do web app]/patio
```

E um retorno: 

```bash
[
    {
        "idPatio": 1,
        "identificacao": "Patio 1",
        "largura": 999.99,
        "comprimento": 99.99,
        "secoes": []
    },
    {
        "idPatio": 2,
        "identificacao": "Patio 2",
        "largura": 100.0,
        "comprimento": 12344.0,
        "secoes": []
    }
]
```

## Update

Para fazer a atualização ele tem que está no método *PUT*, nele tem que passar pela URL a identificacao que você colocou no CREATE que serve para localizar o patio que vai ser 
modificado e um corpo que vai ser o novo patio, exemplo de chamada: 


```bash
  http://[endpoint do web app]/patio/Patio 1
```

No corpo: 
```bash
{
    "largura": 999.99,
    "comprimento": 99.99
}
```

## Delete

Para fazer o *Delete* o método que ele tem que estar é o *DELETE*,  nele tem que passar pela URL a identificacao que serve para localizar o patio que vai ser deletado, exemplo: 

```bash
  http://[endpoint do web app]/patio/Patio 2
```

E nele não vai ter resposta, só se ele não achar o patio com essa identificação.

---

# Seção:

## Create

Para criar ele tem que está no método *POST*, da e o comando para criar foi: 

```bash
  http://[endpoint do web app]/secao
```

E um corpo para persistência de exemplo: 

E no corpo precisa passar a identificação de um Patio que ja foi criado.

```bash
{
    "identificacao": "Parte 1",
    "identificacaoPatio": "Patio 1"
}
```

## Read

Lembrando que o método é o *GET*.

```bash
  http://[endpoint do web app]/secao
```

E um retorno: 

```bash
[
    {
        "id": 61,
        "identificacao": "Parte 1",
        "vagas": []
    },
    {
        "id": 62,
        "identificacao": "Parte 2",
        "vagas": []
    }
]
```

## Delete

Para fazer o *Delete* o método que ele tem que estar é o *DELETE*,  nele tem que passar pela URL a parte que você quer deletar e em qual pátio você quer deletar, exemplo:  

```bash
  http://[endpoint do web app]/secao/Parte 1/Patio 3
```

E nele não vai ter resposta, só se ele não achar a seção.

---

# Vaga:

## Create

Para criar ele tem que está no método *POST*, da e o comando para criar foi: 

```bash
  http://[endpoint do web app]/vaga
```

E um corpo para persistência de exemplo: 

E no corpo precisa passar a identificação de um Patio que ja foi criado e uma Parte ou seção que foi criada também.

```bash
{
    "numeroVaga": 1,
    "disponivel": true,
    "secaoIdentificacao": "Parte 1",
    "padioIdentificacao": "Patio 1"
}


```

## Read

Lembrando que o método é o *GET*.

```bash
  http://[endpoint do web app]/vaga
```

E um exemplo de retorno: 

```bash
[
    {
        "id": 61,
        "identificacao": "Parte 1",
        "vagas": []
    },
    {
        "id": 62,
        "identificacao": "Parte 2",
        "vagas": []
    }
]
```

## Update

Para fazer a atualização ele tem que está no método *PUT*, nele tem que passar pela URL o número da vaga, a parte ou seção que você deseja alterar e em qual patio ela esta, exemplo:

```bash
  http://[endpoint do web app]/vaga/1/Parte 2/Patio 1
```

No corpo: 
```bash
{
    "disponivel": false
}
```

---

# Moto:

## Create

Para criar ele tem que está no método *POST*, da e o comando para criar foi: 

```bash
  http://[endpoint do web app]/moto
```

E um corpo para persistência de exemplo: 

E no corpo precisa passar um *CPF* existente de um motoquerio, uma vaga, um patio e uma seção, para que ele fique bem localizado, exemplo de corpo:

```bash
{
    "modeloMoto": "CB500F",
    "anoMoto": 2022,
    "chassi": "12344",
    "status": "VERDE",
    "motoqueiroCpf": "636.755.590-09",
    "vagaIdentificacao": 1,
    "patioIdentificacao":"Patio 1",
    "secaoIdentificacao": "Parte 1"
}
```

## Read

Lembrando que o método é o *GET*.

```bash
  http://[endpoint do web app]/moto
```

E um exemplo de retorno: 

```bash
[
    {
        "id": 41,
        "modeloMoto": "CB500F",
        "anoMoto": 2022,
        "chassi": "12344",
        "status": "VERDE",
        "motoqueiro": {
            "nomeUser": "Ismael Figueiredo",
            "dataAniversario": "2002-06-11T03:00:00.000+00:00",
            "cpfUser": "63675559009",
            "id": 41,
            "cnh": "2",
            "endereco": {
                "cep": "02241-140",
                "logradouro": "Rua Carmina Pasqui",
                "complemento": null,
                "bairro": "Vila Dom Pedro II",
                "localidade": "São Paulo",
                "uf": "SP"
            },
            "cpfUserFormatado": "636.755.590-09"
        }
    }
]
```
