## Classes e Objetos

Uma classe é um tipo de dado, assim como int, float, bool e string. A diferença é que nós podemos definir as informações contidas ali e os comportamentos que ele tem. As informações ai dentro são como variáveis, mas elas estão vinculadas ao escopo da classe.

```c#
class Pessoa {
  public string nome;
  public int idade;
  public bool estaVivo;
}
```

Por exemplo, a classe acima representa as informações de uma pessoa. Nós podemos adicionar métodos nessa classe.

```c#
class Pessoa {
  public string nome;
  public int idade;
  public bool estaVivo;
  
  public bool MaiorDeIdade()
  {
    if (idade >= 18) {
      return true;
    }
    
    return false;
  }
}
```

Agora temos um método que verifica se a pessoa em questão é maior de idade, chegando se a idade dela é maior ou igual a 18.

#### Objetos

Objetos são instâncias de uma classe. Enquanto a classe do exemplo acima representa as informações de uma pessoa, criar uma variáve desse tipo é criar uma instância dela, ou seja, um objeto.

Para criar instâncias usamos a palavra chave `new`.

```c#
Pessoa joao = new Pessoa();
```

Podemos então acessar as propriedades desse objeto usando o `.`.

```c#
joao.nome = "João";
joao.idade = 19;
joao.estaVivo = true;
```

Os métodos também podem ser acessados da mesma forma

```c#
bool joaoMaiorDeIdade = joao.MaiorDeIdade();
```

#### Objetos dentro de objetos?

Já que classes atuam como um tipo de dado e elas possuem propriedades dentro delas é possível ter uma propriedade que é um outro objeto. O uso do `.` pode ser encadeado para acessar as propriedades do objeto.

```c#
class Pessoa {
  public string nome;
  public Pessoa mae;
}
```

```c#
Pessoa joao = new Pessoa();
joao.nome = "João";
joao.mae = new Pessoa();
joao.mae.nome = "Maria";
```

#### Valores padrões para propriedades

Ás vezes queremos que algumas propriedades tenham valores padrão para todos os objetos que forem criados a partir dela, nesse caso, podemos definir esses valores na hora de declararar as propriedades, assim como damos valores a variáveis.

```c#
class Pessoa {
  public string nome;
  public int idade;
  public bool estaVivo = true;
}
```

#### Métodos Construtores

Assim como tempos valores padrões, podemos determinar um comportamento padrão que deverá ser executado toda vez que criamos um novo objeto. Esse comportamento fica dentro do que chamamos de método construtor. Para criar um, basta declararmos um método com o nome da classe, mas sem declararmos o retorno do método, pois métodos construtores não possuem retorno, já que só podem ser usados em situações bem específicas.

```c#
class Pessoa {
  public string nome;
  
  public Pessoa(string nomeDaPessoa) {
    nome = nomeDaPessoa;
  }
}
```

```c#
Pessoa joao = new Pessoa("João");
```

Uma classe pode ter mais de um método construtor, ás vezes queremos isso para que algum valor possa ser opcional na hora de criar ela. A regra aqui é que a assinatura do método de ser diferente, isto é, eles não podem ter parâmetros do mesmo tipo.

```c#
class Pessoa {
  public string nome;
  public int idade;
  public bool estaVivo = true;
  
  public Pessoa(string nomeDaPessoa, int idade) {
    nome = nomeDaPessoa;
  }
  
  public Pessoa(string nomeDaPessoa, int idade, bool estaVivo) {
    this.estaVivo = estaVivo;
    Pessoa(nomeDaPessoa, idade);
  }
}
```

#### Propriedades estáticas

Quando criamos um objeto, ele é uma instância de uma classe. Então todos as propriedades dele estão vinculadas aquela instância e não são compartilhadas entre objetos. Caso queria criar uma variável que tem o mesmo valor para todos os objetos, criamos uma variável estática, ela ficará atrelada a classe e poderá ser chamada através da mesma.

Considere o seguinte exemplo. Temos um classe `Bola`, que contém apenas uma string que é o identificador de cada uma. E um método que é chamado quando a bola toca no jogador, para exibir uma mensagem de tutorial.

```c#
class Bola {
    public string identificador;
    
    public void TocouNoJogador()
    {
        //Exibe uma mensagem para o player
    }
}
```

Agora, se tivermos vários bolas dentro do nosso jogo o método `TocouNoJogador` será chamado toda vez que qualquer bola tocar o jogador, mas como é uma mensagem de tutorial, só queremos que ela apareça uma vez. Para isso, podemos criar uma propriedade estática para a classe bola que poderá ser acessada por qualquer bola. Para criar uma propriedade estática usamos a palavra chave `static` antes do tipo da propriedade.

```c#
class Bola {
	private static bool jaTocouNoJogador = false;
    public string identificador;
    
    public void TocouNoJogador()
    {
        if (jaTocouNoJogador == false) {
            jaTocouNoJogador = true; //Aqui mudamos para true, para que a próxima bola a tocar o jogador,
            //Exibe uma mensagem para o player
        }
    }
}
```



Pronto, como a propriedade `jaTocouNoJogador` é estática, todas as instâncias da classe Bola tem acesso a ela e o valor é compartilhado. Por isso, quando a primeira bola tocar o jogador, as outras não exibiram a mensagem.

##### Acessando propriedades estáticas em outras classes

Caso a propriedade estática seja `public` também é possível acessar ela de outras classes. Considere o exemplo a seguir onde temos uma classe Inimigo e um contador nela de quantos inimigos existem no jogo. Nesse exemplo, temos também um método `Morrer` que será chamado quando cada inimigo morrer, diminuindo o contador.

```c#
class Inimigo {
    public static int contador = 0;
    
    public Inimigo() //Método construtor
    {
        contador++; //Quando for criado um novo Inimigo queremos aumentar o contador
    }
    
    public void Morrer()
    {
        contador--;
    }
}
```

Enquanto isso, em uma outra classe chamada `GerenciadorDoJogo` temos uma verificação para saber quando todos os inimigos morrerem, acessando a variável estática da classe Inimigo.

```c#
class GerenciadorDoJogo {
    public void Update()
    {
        if (Inimigo.contador == 0) {
            //Jogador ganhou o jogo
        }
    }
}
```

#### Métodos estáticos

Assim como propriedades estáticas, uma classe também pode ter métodos estáticos que podem ser chamados sem a necessidade de um objeto, esse métodos são úteis quando temos um cálculo ou funcionalidade que deve estar disponível a qualquer momento em qualquer classe. Considere o seguinte exemplo de um gerador de palavras com temas.

```c#
class Gerador {
    public static string[] nomes = new string[]{ "Fulano", "Ciclano", "Beltrano" };
    public static string[] cores = new string[]{ "Azul", "Vermelho", "Amarelo" };
    
    public static string Nome()
    {
        return nomes[Random.Range(0, nomes.Length)];
    }
    
    public static string Cor()
    {
        return cores[Random.Range(0, cores.Length)];
    }
}
```

Tendo a classe com esses métodos estáticos, podemos chamar eles em outros scripts.

```c#
class Jogador {
    private string nome;
    private string cor;
    
    public Jogador() { //Método construtor
        nome = Gerador.Nome(); //Chama o método estático Nome da classe Gerador
        cor = Gerador.Cor(); //Chama o método estático Cor da classe Gerador
    }
}
```

A Unity tem diversos métodos estáticos que ajudam em várias situações.

```c#
Vector3 vetorZero = Vector3.zero; //Equivalente à Vector3 vetorZero = new Vector3(0, 0, 0);
Vector3 vetorFrente = Vector3.forward; //Equivalente à vector3 vetorFrente = new Vector3(0, 0, 1);

Color preto = Color.black; //Equivalente à Color preto = new Color(0, 0, 0, 1);
Color azul = Color.blue; //Equivalente à Color azul = new Color(0, 0, 1, 1);
```