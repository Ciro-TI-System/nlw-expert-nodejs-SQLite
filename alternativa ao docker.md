# Alternativa ao Docker

Infelizmente o Docker pode ser um problema para alguns usuários, principalmente aqueles que estão utilizando o Windows. Nesse guia rápido, vamos mostrar pra vocês a como trocar do PostgreSQL para o SQLite (banco em arquivo) e utilizar um serviço online para o Redis.

## Configurando o SQLite

1. Se tiver a pasta `prisma/migrations`, apague ela. (está tudo bem se não tiver)
2. No arquivo `prisma/schema` , troque o valor de `provider` para `sqlite` 
    
    ```sql
    datasource db {
      provider = "sqlite" // Essa linha
      url      = env("DATABASE_URL")
    }
    ```
    
3. No arquivo `.env` troque a URL de conexão para um novo caminho:
    
    ```tsx
    DATABASE_URL="file:./app.db"
    ```
    
    Recomendamos colocar no `.gitignore` esses valores:
    
    ```
    *.db
    *.db-journal
    ```
    
4. Executar o comando `npx prisma migrate dev` para criar as migrations. Informe o nome que preferir.
5. Por fim, no arquivo `src/http/routes/create-poll.ts`, não é possível utilizar o `createMany` com o SQLite, vamos então trocar para:
    
    ```tsx
    // ...
    // Primeiro, criar a poll, sem options.
    const poll = await prisma.poll.create({
      data: {
        title,
      }
    })
    
    // Em seguida, utilizar um Promise.all para criar todas as options:
    await Promise.all(options.map((option) => {
      return prisma.pollOption.create({
        data: {
          title: option,
          pollId: poll.id
        }
      })
    }))
    ```
    

## Configurando o Upstash + Redis

Antes de tudo, o que é o Upstash? Upstash é uma plataforma com diversos serviços para ambiente serverless (outro tipo de aplicação que não precisa se preocupar agora, foco jovem gafanhoto), e um desses serviços é um Banco Redis. O plano free é bem generoso.

1. Crie uma conta no site: [Upstash: Serverless Data Platform](https://upstash.com/)
2. No menu ao topo da página, selecione `Redis` e clique em “Create Database”.
    
    Esse modal deverá ser aberto:
    
    ![Untitled](https://efficient-sloth-d85.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F08f749ff-d06d-49a8-a488-9846e081b224%2F1b8daaec-62bb-4a07-908a-5db301a75e90%2FUntitled.png?table=block&id=ec7d0f53-a491-4815-9987-aba2b6e0d8c0&spaceId=08f749ff-d06d-49a8-a488-9846e081b224&width=2000&userId=&cache=v2)
    
    Coloque o nome de sua preferência, mantenha a opção `Regional` selecionada e no select abaixo escolha `us-east-1`.
    
    Não precisa marcar nenhuma das outras opções. Clique em `Create`.
    
3. Ao criar sua Database, acesse ela e terá uma página parecida com essa:
    
    ![bcffb95ba92461382b864aec819abf913f8846d3b48c344b2b7f9ff04a96c46e copy.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/08f749ff-d06d-49a8-a488-9846e081b224/24d79027-6fb8-4411-8148-0fde919371df/bcffb95ba92461382b864aec819abf913f8846d3b48c344b2b7f9ff04a96c46e_copy.jpg)
    
    Clique no botão informado e copie toda a URL de conexão.
    
4. De volta ao seu projeto, no arquivo `.env`, crie uma nova variável com o nome `REDIS_URL` 
    
    ```
    DATABASE_URL="file:./app.db"
    REDIS_URL="URL_AQUI"
    ```
    
5. Por fim, no arquivo `src/lib/redis.ts`, vamos configurar para utilizar essa variável:
    
    ```tsx
    import { Redis } from "ioredis";
    
    if (!process.env.REDIS_URL) {
      throw new Error("Missing REDIS_URL env var");
    }
    
    export const redis = new Redis(process.env.REDIS_URL)
    ```
    
    *Você consegue visualizar os dados do banco na aba “Data Browser”*
    

Concluindo todos esses passos a aplicação já irá funcionar sem precisar do Docker. Nice! 🚀

Segue o link do commit com todas essas alterações para caso queira comparar:

https://github.com/rocketseat-education/nlw-expert-nodejs/tree/0f1a0f1e8343963ff72b523c5105de3d375e2b7f