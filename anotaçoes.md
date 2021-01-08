LIFECYCLE HOOKS PARA HTTP REQUEST PRIMARIA, para abastecimento de dados

```javascript
componentDidMount() {
    console.log("montei");
    axios.get("https://jsonplaceholder.typicode.com/posts").then((res) => {
      const posts = res.data.slice(0, 4);
      const updatePosts = posts.map((e) => {
        return { ...e, author: "william" };
      });
      this.setState({ posts: updatePosts });
    });
  }
```

LIFECYCLE HOOKS PARA HTTP REQUEST SECUNDÁRIA, para atualizaçao de dados

AXIOS DEFAULTS

```javascript
axios.defaults.baseURL = "https://jsonplaceholder.typicode.com";
axios.defaults.headers.common["Authorization"] = "AUTH TOKEN";
axios.defaults.headers.post["Content-Type"] = "application/json";
```

```javascript ATENÇAO, setSTATE Atualiza, e dentro de um ciclo de vida que executa apos qualquer coisa atualizar, nao vai ficar legal
componentDidUpdate() {
    if (
      this.props.selectedPost &&
      this.state.loadedPost !== this.props.selectedPost.id
    )
      Axios.get(
        "https://jsonplaceholder.typicode.com/posts/" + this.props.selectedPost
      ).then((e) => this.setState({ loadedPost: e.data }));
  }
```

```javascript ===INTERCEPTORS, pegando erros globais de conexao http
axios.interceptors.request.use(
  (res) => {
    //posso editar a resposta antes de enviar, como incluir headers
    console.log(res);
    return res;
  },
  (error) => {
    //esse erro é sobre enviar requisiçoes, por exemplo falha de internet
    //posso enviar uma segunda funçao apos a primeira, para manusear os erros
    console.log(error);
    return Promise.reject(error); //aqui é util caso precise passar dados de falta conexao para ui
  }
);
```

```javascript
axios.interceptors.request.use(
  (req) => {
    return req;
  },
  (error) => {
    console.log(error);
    return Promisse.reject(error);
  }
);
```

Removing Interceptors

You learned how to add an interceptor, getting rid of one is also easy. Simply store the reference to the interceptor in a variable and call eject with that reference as an argument, to remove it (more info: https://github.com/axios/axios#interceptors):

    var myInterceptor = axios.interceptors.request.use(function () {/*...*/});
    axios.interceptors.request.eject(myInterceptor);

```javascript
<Link
  to={{
    pathname: "new-post",
    hash: "#submit",  ==== LOCAL QUE ELE VAI PULAR QUANDO ABRIR
    search: "?quick-sumit=true",   ==== PASSA PARAMETROS PELO URL
  }}
>
  New Post
</Link>
```

3 FORMAS DE PASSAR PROPS VIA ROUTER

```javascript Passando props atraves de router
 return (
          <Post
            key={post.id}
            clicked={() => this.selectedPostHandler(post.id)}
            author={post.author}
            title={post.title}
            // 1)  match={this.props.match} // uma das maneiras de passar props
            // 2)  {...this.props} uma das maneiras de passar props
          />
            // 3)  export default withRouter(post);


```

forma correta de passar props para componentes é atraves do HOC withRouter
envovlido com o withRouter, ele recebe os props do pai, sem o pai precisar passar explicitamente

---

Uso o NavLink para melhorar o menu, adicionando active automaticamente
porem preciso usar o exact para ativar o prefixo. tambem preciso da barra antes, senao nao ativa

  <li>
                <NavLink to="/" exact>
                  HOME
                </NavLink>
  </li>
posso definir minha propria classe active com
   <NavLink activeClassName="ativo" to="/" exact>
                  HOME
   </NavLink>
--------------------
se colocarmos somente route abaixo de route, o  /:id vai entender o /novoposto como um id,
pra isso usamos o switch, que permite carregar somente uma rota de cada vez
tbm posso abrir novas rotas por fora do switch e abrri junto, se quiser
  <Switch>  a ordem é mto importante
          <Route path="/" exact component={Posts} />
          <Route path="/new-post" component={NewPost} />
          <Route path="/:id" exact component={FullPost} />
        </Switch>
-------------------
Outra forma de carregar uma pagina do post, sem criar um link para cada post.
Os objetos do router possuem methodos, goBack, goAfter, history, push etc
podemos criar uma funçao que ao clicar, faz push para uma nova pagina, atraves do id do post clicado, manja?

```javascript
selectedPostHandler = (id) => {
  this.props.history.push({ pathname: "/" + id });
  console.log(this.props);
};
```

---

LINK DO NAVEGADOR ATIVO
só é possivel, ate entao, so com um subdiretorio, como /posts/id, ai posso colocar /posts, todos ficaram ativis

```javascript
<NavLink to="/" exact>
  HOME
</NavLink>
```

---

+uma formad de chamar rota. pegando o caminho que foi passado

```javascript
<Route path={(this.props.match.url = "/:id")} exact component={FullPost} />
```
