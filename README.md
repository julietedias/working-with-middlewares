# Desafio Ignite: Trabalhando com Middlewares

## 📝 Sobre o desafio

Nesse desafio será trabalhado mais a fundo com middlewares no Express.
É uma aplicação para gerenciar tarefas (ou *todos*).

Será permitida a criação de um usuário com `name` e `username`, bem como fazer o CRUD de *todos*:

- Criar um novo *todo*;
- Listar todos os *todos*;
- Alterar o `title` e `deadline` de um *todo* existente;
- Marcar um *todo* como feito;
- Excluir um *todo*;

Tudo isso para cada usuário em específico. Além disso, teremos um plano grátis onde o usuário só pode criar até dez *todos* e um plano Pro que irá permitir criar *todos* ilimitados, isso tudo usando middlewares para fazer as validações necessárias.

A seguir veremos com mais detalhes o que e como precisa ser feito 🚀

## Middlewares da aplicação

Aqui teremos uma breve descrição do que cada middleware deve fazer.

### checksExistsUserAccount

Esse middleware é responsável por receber o username do usuário pelo header e validar se existe ou não um usuário com o username passado. Caso exista, o usuário deve ser repassado para o request e a função next deve ser chamada.

### checksCreateTodosUserAvailability

Esse middleware deve receber o **usuário** já dentro do request e chamar a função next apenas se esse usuário ainda estiver no **plano grátis e ainda não possuir 10 *todos* cadastrados** ou se ele **já estiver com o plano Pro ativado**.

### checksTodoExists

Esse middleware deve receber o **username** de dentro do header e o **id** de um *todo* de dentro de `request.params`. Você deve validar o usuário, validar que o `id` seja um uuid e também validar que esse `id` pertence a um *todo* do usuário informado.

Com todas as validações passando, o *todo* encontrado deve ser passado para o `request` assim como o usuário encontrado também e a função next deve ser chamada.

### findUserById

Esse middleware possui um funcionamento semelhante ao middleware `checksExistsUserAccount`, mas a busca pelo usuário deve ser feita através do **id** de um usuário passado por parâmetro na rota. Caso o usuário tenha sido encontrado, o mesmo deve ser repassado para dentro do `request.user` e a função next deve ser chamada.

## Específicação dos testes

Em cada teste, tem uma breve descrição no que sua aplicação deve cumprir para que o teste passe.

Para esse desafio, temos os seguintes testes:

### Testes dos middlewares

- **Should be able to find user by username in header and pass it to request.user**
    
    Para que esse teste passe, deve-se permitir que o middleware **checksExistsUserAccount** receba um username pelo header do request e caso um usuário com o mesmo username exista, ele deve ser colocado dentro de `request.user` e, ao final, retorne a chamada da função `next`.
    
    Atente-se bem para o nome da propriedade que armazenará o objeto `user` no request.
    
- **Should not be able to find a non existing user by username in header**
    
    Para que esse teste passe, no middleware **checksExistsUserAccount** deve-se retornar uma resposta com status `404` caso o username passado pelo header da requisição não pertença a nenhum usuário. É possível também retornar uma mensagem de erro, mas isso é opcional.

  - **Should be able to let user create a new todo when is in free plan and have less than ten todos**
    
    Para que esse teste passe, deve-se permitir que o middleware **checksCreateTodosUserAvailability** receba o objeto `user` (considere sempre que o objeto existe) da `request` e chame a função `next` somente no caso do usuário estar no **plano grátis e ainda não possuir 10 *todos* cadastrados** ou se ele **já estiver com o plano Pro ativado**.
    
    <aside>
    💡 É possível verificar se o usuário possui um plano Pro ou não a partir da propriedade `user.pro`. Caso seja `true` significa que o plano Pro está em uso.
    </aside>
    
- **Should not be able to let user create a new todo when is not Pro and already have ten todos**
    
    Para que esse teste passe, no middleware **checksCreateTodosUserAvailability** deve-se retornar uma resposta com status `403` caso o usuário recebido pela requisição esteja no **plano grátis** e **já tenha 10 *todos* cadastrados**. Pode-se também retornar uma mensagem de erro, mas isso é opcional.

  - **Should be able to let user create infinite new todos when is in Pro plan**
    
    Para que esse teste passe, deve-se permitir que o middleware **checksCreateTodosUserAvailability** receba o objeto `user` (considere sempre que o objeto existe) da `request` e chame a função `next` caso o usuário já esteja com o plano Pro. 
    
- **Should be able to put user and todo in request when both exits**
    
    Para que esse teste passe, o middleware **checksTodoExists** deve receber o `username` de dentro do header e o `id` de um *todo* de dentro de `request.params`. Deve-se validar que o usuário exista, validar que o `id` seja um uuid e também validar que esse `id` pertence a um *todo* do usuário informado.
    
    Com todas as validações passando, o *todo* encontrado deve ser passado para o `request` assim como o usuário encontrado também e a função next deve ser chamada.
    
    É importante que seja colocado dentro de `request.user` o usuário encontrado e dentro de `request.todo` o *todo* encontrado.

  - **Should not be able to put user and todo in request when user does not exists**
    
    Para que esse teste passe, no middleware **checksTodoExists** deve-se retornar uma resposta com status `404` caso não exista um usuário com o `username` passado pelo header da requisição.
    
- **Should not be able to put user and todo in request when todo id is not uuid**
    
    Para que esse teste passe, no middleware **checksTodoExists** deve-se retornar uma resposta com status `400` caso o `id` do *todo* passado pelos parâmetros da requisição não seja um UUID válido (por exemplo `1234abcd`).
    
- **Should not be able to put user and todo in request when todo does not exists**
    
    Para que esse teste passe, no middleware **checksTodoExists** deve-se retornar uma resposta com status `404` caso o `id` do *todo* passado pelos parâmetros da requisição não pertença a nenhum *todo* do usuário encontrado.
    
- **Should be able to find user by id route param and pass it to request.user**
    
    Para que esse teste passe, o middleware **findUserById** deve receber o `id` de um usuário de dentro do `request.params`. Deve-se validar que o usuário exista, repassar ele para `request.user` e retornar a chamada da função next.

  - **Should not be able to pass user to request.user when it does not exists**
    
    Para que esse teste passe, no middleware **findUserById** deve-se retornar uma resposta com status `404` caso o `id` do usuário **passado pelos parâmetros da requisição não pertença a nenhum usuário cadastrado.

## 🗔 Para rodar o projeto:

`yarn`

`yarn dev`

## 🗔 Para rodar os testes:

`yarn test`

## 💻 Tecnologia
- JavaScript

## 📩 Contato
juliete_dias@outlook.com.br

## 📜 Licença
Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

