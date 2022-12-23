<div id="readme-top"></div>


<!-- PROJECT SHIELDS -->
<div align="center">

[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![MIT License][license-shield]][license-url]

<!--
[![LinkedIn][mariana-linkedin-shield1]][mariana-linkedin-url]
[![Contributors][mariana-github-shield]][mariana-github-url]
<strong>☜(⌒ᵕ⌒)☞</strong>
[![Contributors][miguel-github-shield]][miguel-github-url]
-->


<!--
☜(⌒▽⌒)☞
<strong>☜(⌒▽⌒)☞</strong>
<strong>☜(⌒ᵕ⌒)☞</strong>
<strong>☜( ˊᵕˋ )☞</strong>
ଘ(੭*ˊᵕˋ)੭* ̀ˋ 
-->
</div>


<!-- PROJECT LOGO 
<br />
<div align="center">
  <a href="https://github.com/github_username/repo_name">
    <img src="images/logo.png" alt="Logo" width="80" height="80">
  </a>
  -->

<div align="center">


<h3 align="center">Programming Language for Interactive Quizzes</h3>

  <p align="center">
    The final project of the <a href="https://www.ua.pt/en/uc/12272">Compilers</a> course, lectured at University of Aveiro, in the academic year of 2019/2020, as part of my BSc in Informatics Engineering.

  </p>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#introduction">Introduction</a></li>
    <li><a href="#whats_done">What's done</a></li>
    <li><a href="#whats_to_improve">What's to improve</a></li>
    <li><a href="#try_it_on_your_machine">Try it on your machine</a></li>
    <li><a href="#files_overview">Files Overview</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
<!-- ## About The Project -->

<h2 id="introduction">Introduction</h2>
A <strong>transpiler</strong> can be seen as a <i>translator</i> of the source language to a destination language. During this process, not only the syntactic validity of the program should be checked, but also its semantic correctness.

### Goals
The goal of this work involved developing two languages: one for the transpiler -- which is the main language of the project, and one to read structured information.

This process included all phases of construction of programming languages:
- Conception and definition of a programming language (sintax and semantics)
- Lexical analysis implementation in ANTLR4
- Definition of the semantic rules that are to apply to the language, and their application, in regards to the previous point
- Writing documentation on the language
- Critical choice on the destination language
- Definition of the code generation patterns
- Concratization of the transpiler as a whole


<strong>Note</strong>: the information presented on both <strong>Information</strong> and <strong>Goals</strong> are described with more detail in the project guide, which is in Portuguese.
<p align="right">(<a href="#readme-top">back to top</a>)</p>

<h2 id="whats_done">What's done</h2>

### Idea
The programming language we designed and developed for this project is oriented to creating and manipulating interactive quizzes.

The language supports 4 different types of questions: multiple-choice, true/false, short answer or long answer questions. It also allows the programmer to configure the way the quizz will be delivered, for example, through defining the order that the questions are shown, or defining a time limit for it. 

<div style="color: red">TODO: isto é só para o questionário ou para as perguntas???</div>

The questions can be uploaded from files written in a second language developed in the scope of this projct, or can be created along with the other code. Both options coexist.

When executed, the program will present the user with the quizz defined. It will wait for the users input and store the results, so that it can show a results report at the end of the quiz.

#### Database Language
This is the secondary language, and it has the goal of allowing questions to be easily stored and uploaded to the quiz without the need of writing them in the main code.

Examples:
```
questao:EscolhaMultipla medio pontuacao=20 [CIENCIA->PLANTAS->Q1] "A fotossíntese..."
inicio
  "é composta pela fase fotoquímica onde ocorre o ciclo de calvin " false pontuacao=-20;
  "é composta pela fase química onde ocorre o ciclo de calvin" true pontuacao=70; 
  "permite obter Oxigênio e glicose" false pontuacao=-20; 
  "permite obter Dóxido de Carbono e glicose" true pontuacao=30;
  "permite obter luz solar" false pontuacao=-40;
fim 
```

```
questao:CurtaTextual facil pontuacao=10 [HISTORIA->MUNDIAL->Q5]  "Em que ano foi descoberto o caminho marítimo para a Índia?"
inicio
  "1498" pontuacao=100;
fim
```

Every question described with this language has information on:
- the type of question (in the example `EscolhaMultipla` -- multiple-choice; `CurtaTextual` -- short answer)
- difficulty (optional) (in the example `medio` -- medium)
- points (optional) (in the example `pontuacao=20` -- 20 points)
- topic (hirerarchic) (in the example `[CIENCIA->PLANTAS->Q1]` -- the topic `Q1` which is within the topic `PLANTAS` (plants) which is within the topic `CIENCIA` (science))
- the question string (optional) (in the example `A fotossíntese...` -- fotossinteses...)
- the answers (not optional, but can be none..)

The answers have the following information:
- answer string 
- true/false (optional) <span style="color: red"> only to be used in multiple-choice questions???</span>
- points (optional) <span style="color: red"> TODO: confirmar como é que isto funciona</span>

### Quiz language
This is the main language of the project, looking more like a general programming language. It is responsible for the creation of the quizz itself. It can be used alone or with the previous language. 

---

#### Types
The following class diagram schemes the types supported by the language. Note that this diagram doesn't represent with complete accuracy the structure, since the types aren't classes _per se_.

![Diagram](./diagram.svg)

##### Questionário
`Questionário` is the type of data that defines some properties about the quiz the user wants to create. It has information about the questions that are going to be considered for the quiz, as well as other customization options, such as the `tempomaximo` -- which alows the user to define a time limit for the quiz, after which the answers won't be stored nor considered for the scoring.

After the quiz is complete, the variable `duracao` will store how long it took to complete, while `pontuacao` will contain the scoring of the quiz.

It has 2 functios associated with it:
- adicionarquestao, which accepts as argument a `Questao`, and is used to add a question to it
- importar, which is accepts a String with a path to a file written with the previous language. The questions will then be imported to this quiz

<span style="color:red"> pois. adiciona ou substitui? </span>

##### Grupo
This type is derived from the `Questionario`, supporting all of its functionalities, but going a bit further.
This group of questions can be characterized by a topic (`tema`), type and difficulty. This is saved when the function `adicionarquestoes` is used. It can be used with these 3 attributes, using the following revolutionary syntax:
```
g2 % adicionarquestoes(q1, tema = [Tema_C], dificuldade = FACIL, tipo = "VerdadeiroFalso").
```
Furthermore, the programmer can also define the minimum  number of questions that the user must complete before submitting the quiz, `minperguntasaresponder`, and also the number of questions to present in the quiz `nrperguntasaapresentar`.


##### Questao
This type defines a question. `pergunta` stores the text of the question, while `tema` defines the topic, `dificuldade` the difficulty, `pontuacao` its points, `respostas` is a list of possible answers, useful for all the answer types except the `LongaTextual`. Furthermore, there's `tempomaximo`, storing the maxmimum time the user has to answer this question. To finish, there's also a String to present the type of Question, so that the programmer can filter.

Furthermore, after a quiz is complete, the `respostadada` attribute will have information on the answer that the user chose for that question.

##### TODO: cobrir os diferentes tipos de Questao

##### Resposta
This type is used to store a possible answer to a question.
It has information on how much it values, if the user were to use it.

TODO: separar isto por cada tipo especifico, e vermelhor os argumentos
We can also define Resposta objects using this AMAZING sintaxe:
```
aaa as Resposta:EscolhaMultipla.
aaa := "Não tenho fome.", 60. 

bbb as Resposta:VerdadeiroFalso.
bbb := "Não tenho fome.", false, 60, -30.
```


##### TODO: falar dos outros tipos de Resposta

##### Lista
`Lista` is an array-like type that permits the aggregation of multiple objects. It has the method `baralhar`, useful to mix the order of the questions in the quiz, for example:
```
quiz->questoes->baralhar(). # mix the questions in the quiz
```
It allows the initialization with the use of brackets, and allows for objects to be initialized at the same time, for example:
```
lr1 as Lista<Resposta:EscolhaMultipla>.
lr1 := { "Não tenho fome.", 60 ; "Estou cansada.", -20}. 


lr2 as Lista<Resposta:VerdadeiroFalso>.
lr2 := { "Não tenho fome.", false, 60, -30 ; "Estou cansada.", true, 40, -20}.
```

##### Tema
This type is used to define the topic of the question or question group, filtering the questions that are included in it. It can be used in an hierarchical fashion. It's sintax is very intuitive:
```
q->tema := [TECH->PROGRAMMING->PYTHON]
```
This way, when creating a Grupo, we can say that we only want questions with tema \[TECH->PROGRAMMING], and in this case, our example would fit the criteria.

##### Dificuldade
To finish, there's this type, used to store the difficulty level of the question, and has 3 possible values: `FACIL`, `MEDIO` and `DIFICIL`. It can also be used to filter questions, just like Tema.

---



This language supports multiple functionalities commonly present in general-purpose programming languages, such as (but not exclusive to):
- #### variable declaration and initialization
This language allows for the declaration of quizzes (Questionario?), questions (Pergunta?), lists (Lista?), topics (Temas), difficulty levels (Dificuldade) and answers (Resposta), besides other symple types such as boolean, strings, integers and double.
`q as Questionário.`
`tx1 as Questão:CurtaTextual.`

- #### attribuition of values to variables
Some types such as `Questão` have some variables that can be changed to define the question:
`tx1->tema := [TEMA_A].`
`tx1->dificuldade := FACIL.`
The parser? can identify which type the value is. In this example `FACIL` is of type Dificuldade, but we do not need to state specifically this.

```
lr2 as Lista<Resposta:CurtaTextual>.
lr2 := { "Nee", 20 ; "Ja", 30 ; "Ik", 30; "outra", 20 } .
```
Answers can be inserted in lists using this nomencleature. Each answer will be `<string>, <points>`, and they are separated with a `;`.
This is too much for me.


- #### desicion structures
The language supports decision structures such as `if`, `else if` and `else`, but with different names (who would say.).

```
se (g1->pontuacao < p) # p: int variable previously defined
inicio
  apresentar(“Não pode prosseguir! Teve: ” & str(g1->pontuação) & “ pontos”)
fim
senao
inicio
  apresentar(“Parabéns, concluiu a primeira fase”).
fim
```

- #### cicles
This language supports cycles, more specifically for-each cycles.
```
para (q as Questao em q1->questoes)
inicio
  t as String.
  t := “LongaTextual”.
  se (q->tipo == t)
  inicio
    apresentar(“Respostas: “ & str(q->respostadada)).
  fim
fim
```

<br /><br />

It also has some native methods, useful for the use-case. These are:
- #### baralhar (mix)
  Allows the user to vary randomly the order of the answers to be shown in a question. It can only be used in List-type objects.
  ```
  em1->respostas := lr.
  em1->respostas%baralhar().
  ```


- #### apresentarMenu (present menu)
  Used to present the menu correspondent to a group of questions - can be either a Grupo or Questionario. Can also have an input for the number of seconds it will be possible to answer all questions presented. After this number has passed, no answer will be saved.

  Better said, this will present the quiz.
  ```
  apresentarMenu(g2).
  ```

- #### apresentar
  Used to print something in the consol.
  ```
  apresentar(“Hello world").
  ```

- ### str
  Used to convert number values to String.
  ```
  apresentar(“Pontos” & str(g1->pontuação)).
  ```

<div style="red">
Specific types of Resposta have different attributes, such as the True/False ones:
- pontuacao
- desconto
- correcao

TODO: check
</div>


The quotation is as follows:
```
para cada resposta da questao:
pontuacao += pontuacao questao * cotacao da resposta /100;
<=> pontuacao = pontuacao questao * ( soma cotacoes das respostas )/100
```

Se o tempo max individual da pergunta passar, então essa pergunta não é guardada, mas o resto do questionario continua



<!--
The programming language developed in this project allows the design of a quiz. When executed, the program will present questions of multiple choice, wait for the answer and store the results, so that, in the end, it can show a results report.

There should be a questions data base. Each question should be characterized by a topic, one or more key-words and various answers, right and wrong. The database can be distributed among one or more files. For example, there ca be a file per topic.
Topics, the questions themselves and even the answers can have a diffeerent difficult level associated, so that tests of different difficulty can be generated.

A languagem secundária é usada para descrever o conteúdo dos ficheiros de perguntas, usado para carregar a base de dados para memória.

A primária é para desenhar um teste. Esta possui: 
- constantes e variaveis, escalares e nao escalares
- estruturas repetitivas
- estruturas condicionais
- estruturas de monitorização dos tempos, seja de cada resposta, seja do total

A compilação deve gerar um programa q qdo executado realiza o teste iterativamnete, pergunta a pergunta.
-->




<p align="right">(<a href="#readme-top">back to top</a>)</p>

<h2 id="whats_to_improve">What's to improve</h2>
A lot.


<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->
<h2 id="try_it_on_your_machine">Try it on your machine</h2>
Yet to be ~ <strong>ଘ(੭*ˊᵕˋ)੭* ̀ˋ</strong> ~ written!


<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- LICENSE -->
<h2 id="license">License</h2>

Distributed under the MIT License. See `LICENSE` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- ACKNOWLEDGMENTS -->
<h2 id="acknowledgments">Acknowledgments</h2>

[DETI - Departamento de Eletrónica, Telecomunicações e Informática](https://www.ua.pt/pt/deti)

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

[forks-shield]: https://img.shields.io/github/forks/immarianaas/c-quiz-language.svg?style=for-the-badge
[forks-url]: https://github.com/immarianaas/c-quiz-language/network/members

[stars-shield]: https://img.shields.io/github/stars/immarianaas/c-quiz-language.svg?style=for-the-badge
[stars-url]: https://github.com/immarianaas/c-quiz-language/stargazers

[issues-shield]: https://img.shields.io/github/issues/immarianaas/c-quiz-language.svg?style=for-the-badge
[issues-url]: https://github.com/immarianaas/c-quiz-language/issues

[license-shield]: https://img.shields.io/github/license/immarianaas/c-quiz-language.svg?style=for-the-badge
[license-url]: https://github.com/immarianaas/c-quiz-language/blob/master/LICENSE

<!--
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/linkedin_username
-->

[product-screenshot]: images/screenshot.png
[Next.js]: https://img.shields.io/badge/next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white
[Next-url]: https://nextjs.org/
[React.js]: https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB
[React-url]: https://reactjs.org/
[Vue.js]: https://img.shields.io/badge/Vue.js-35495E?style=for-the-badge&logo=vuedotjs&logoColor=4FC08D
[Vue-url]: https://vuejs.org/
[Angular.io]: https://img.shields.io/badge/Angular-DD0031?style=for-the-badge&logo=angular&logoColor=white
[Angular-url]: https://angular.io/
[Svelte.dev]: https://img.shields.io/badge/Svelte-4A4A55?style=for-the-badge&logo=svelte&logoColor=FF3E00
[Svelte-url]: https://svelte.dev/
[Laravel.com]: https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white
[Laravel-url]: https://laravel.com
[Bootstrap.com]: https://img.shields.io/badge/Bootstrap-563D7C?style=for-the-badge&logo=bootstrap&logoColor=white
[Bootstrap-url]: https://getbootstrap.com
[JQuery.com]: https://img.shields.io/badge/jQuery-0769AD?style=for-the-badge&logo=jquery&logoColor=white
[JQuery-url]: https://jquery.com 





[mariana-github-shield1]: https://img.shields.io/badge/--black.svg?style=for-the-badge&logo=github&colorB=555
[mariana-linkedin-shield1]: https://img.shields.io/badge/--black.svg?style=for-the-badge&logo=linkedin&colorB=0e76a8

[mariana-github-shield]: https://img.shields.io/badge/-Mariana-black.svg?style=for-the-badge&logo=github&colorB=555
[mariana-linkedin-shield]: https://img.shields.io/badge/-Mariana-black.svg?style=for-the-badge&logo=linkedin&colorB=555

[mariana-github-url]: https://github.com/immarianaas
[mariana-linkedin-url]: https://www.linkedin.com/in/immarianaas



[miguel-github-shield1]: https://img.shields.io/badge/--black.svg?style=for-the-badge&logo=github&colorB=555
[miguel-linkedin-shield1]: https://img.shields.io/badge/--black.svg?style=for-the-badge&logo=linkedin&colorB=0e76a8

[miguel-github-shield]: https://img.shields.io/badge/-Miguel-black.svg?style=for-the-badge&logo=github&colorB=555
[miguel-linkedin-shield]: https://img.shields.io/badge/-Miguel-black.svg?style=for-the-badge&logo=linkedin&colorB=555

[miguel-github-url]: https://github.com/Miguel17297
[miguel-linkedin-url]: https://github.com/immarianaas/c-quiz-language

[Python-logo]: https://img.shields.io/badge/Python-306998?style=for-the-badge&amp;logo=python&amp;logoColor=white
[Python-url]: https://python.org