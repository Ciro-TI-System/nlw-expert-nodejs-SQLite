<p align="center">
  <img alt="Logo NLW Expert - Rocketseat" src=".github/logo-nlw-expert.svg" width="200px" />
</p>

<h1 align="center"> 
Real-Time Voting System
</h1>

<p align="center">
⚙️ Desenvolvimento de um sistema de votação em tempo real onde os usuários podem criar uma enquete e outros usuários podem votar. O sistema gera um ranking entre as opções e atualiza os votos em tempo real. ⚙️
</p>

<br>
  
![Cover](./.github/cover.png)
<br>

## 🛠 Tecnologias

Esse projeto foi desenvolvido com as seguintes tecnologias:

- **[SQLite](https://www.sqlite.org/docs.html)**
- **[NodeJS](https://nodejs.org/en/guides)**

<br>

## Setup

- Clone the repository;
- Install dependencies (`npm install`);
- Setup PostgreSQL and Redis (`docker compose up -d`);
- Copy `.env.example` file (`cp .env.example .env`);
- Run application (`npm run dev`);
- Test it! (I personally recommend testing with [Hoppscotch](https://hoppscotch.io/)).

<br>

## HTTP

### POST `/polls`

Create a new poll.

#### Request body

```json
{
  "title": "Qual a melhor linguagem de programação?",
  "options": ["JavaScript", "Java", "PHP", "C#"]
}
```

#### Response body

```json
{
  "pollId": "194cef63-2ccf-46a3-aad1-aa94b2bc89b0"
}
```

<br>

### GET `/polls/:pollId`

Return data from a single poll.

#### Response body

```json
{
  "poll": {
    "id": "e4365599-0205-4429-9808-ea1f94062a5f",
    "title": "Qual a melhor linguagem de programação?",
    "options": [
      {
        "id": "4af3fca1-91dc-4c2d-b6aa-897ad5042c84",
        "title": "JavaScript",
        "score": 1
      },
      {
        "id": "780b8e25-a40e-4301-ab32-77ebf8c79da8",
        "title": "Java",
        "score": 0
      },
      {
        "id": "539fa272-152b-478f-9f53-8472cddb7491",
        "title": "PHP",
        "score": 0
      },
      {
        "id": "ca1d4af3-347a-4d77-b08b-528b181fe80e",
        "title": "C#",
        "score": 0
      }
    ]
  }
}
```

<br>

### POST `/polls/:pollId/votes`

Add a vote to specific poll.

#### Request body

```json
{
  "pollOptionId": "31cca9dc-15da-44d4-ad7f-12b86610fe98"
}
```

<br>

## WebSockets

### ws `/polls/:pollId/results`

#### Message

```json
{
  "pollOptionId": "da9601cc-0b58-4395-8865-113cbdc42089",
  "votes": 2
}
```

<!--START_SECTION:footer-->

<br>

## 📝 License

<p align="center">
  <img alt="License" src="https://img.shields.io/static/v1?label=license&message=MIT&color=49AA26&labelColor=000000">
</p>
<p align="center">Esse projeto está sob a licença MIT.</p>

<br>

### 👨‍💻 Autor

 <img style="border-radius: 50%;" src="./public/TI-System.png" width="100px;" alt=""/>
 <em>Ciro Batista da Silva<em>
 
 <br/>

 <p>👋🏽 Entre em contato!</p>

[![Twitter Badge](https://img.shields.io/badge/-@CiroSilva2020-1ca0f1?style=flat-square&labelColor=1ca0f1&logo=twitter&logoColor=white&link=https://twitter.com/CiroSilva2020)](https://twitter.com/CiroSilva2020) [![Linkedin Badge](https://img.shields.io/badge/-Ciro-blue?style=flat-square&logo=Linkedin&logoColor=white&link=https://www.linkedin.com/in/ciro-batista-da-silva-8b6838205/)](https://www.linkedin.com/in/ciro-batista-da-silva-8b6838205/)
[![Gmail Badge](https://img.shields.io/badge/-cirofight@gmail.com-c14438?style=flat-square&logo=Gmail&logoColor=white&link=mailto:cirofight@gmail.com)](mailto:cirofight@gmail.com)
