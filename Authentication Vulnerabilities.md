<div align=center>
  <img src="https://i.imgur.com/3H4vMoD.png" width=15%;>
</div>
<br>

<h1 align=center> Vulnerabilidades de Autenticação </h1>

<br>
<br>
<div align=center>
  <img src=https://portswigger.net/web-security/images/password-reset-poisoning.svg width=50%;>
</div>
<br>

### O que é autenticação?

A autenticação é o processo de validação de algo ou alguém, por exemplo, em um sistema de login a autenticação tem a função de verificar se você realmente é quem diz ser. Na internet o processo de autenticação é fundamental para criar ambientes controlados, muitas pessoas têm uma visão limitada sobre a autenticação, limitando-se apenas a formulários de login. A autenticação é muito mais que só um formulário de login, a autenticação esta presente em cartões de crédito, chaves de autenticidade, controles físicos, como leitor de impressão digital, e várias outras coisas.

### O que é autorização?
A autorização é um processo em que verifica se você pode ou não realizar tal ação. A autorização assim como a autenticação são essenciais para manter um ambiente seguro, a autorização geralmente está relacionada a permissões. Para exemplificar vamos imaginar o cenário de um site de notícias, imagine que você possui uma conta neste site de notícias e você está devidamente autenticado no mesmo, ou seja, você realmente é quem diz ser, porém você é apenas um leitor comum, você não é um assinante, sendo assim, neste cenário caso você consiga obter acesso a algum conteúdo destinado a assinantes do jornal temos uma vulnerabilidade de autorização. Você obteve acesso a algo que não estava autorizado a obter, mesmo passando pelo processo de autenticação.
### Fatores de autenticação

Hoje existem vários fatores de autenticação, e não só um fator primário como uma senha. A verdade é que com a evolução do mundo digital desenvolvemos várias maneiras de verificar a autenticidade de algo ou alguém. Podemos classificar os cinco principais fatores para realizar a validação de autenticidade, sendo eles:
- **Fator de conhecimento:** algo que você sabe, como uma senha, códigos de recuperação ou respostas para perguntas de segurança.
- **Fator de posse:** algo que você tem, ou seja, objetos físicos, podendo ser um celular ou uma chave de segurança (YubiKey).
- **Fator de localização:** esse fator se baseia em sua localização geográfica para realizar uma autenticação, esse fator assim como o fator de tempo não são comumente utilizados, mas em teoria, só permitiria autenticações em determinada localização geográfica.
- **Fator de tempo:** esse fator limita autenticações em determinados períodos de tempo, assim como o fator de localização esse fator não é comumente utilizado.
---
### Como surgem as vulnerabilidades de autenticação?

Geralmente as vulnerabilidades de autenticação ocorrem de duas maneiras, sendo elas:

1. **O mecanismo de autenticação não protege contra: ataques de força bruta**, isso significa que, não há nenhum bloqueio sendo realizado por parte da aplicação para limitar o número de tentativas que podem ser realizadas por um usuário. Caso uma aplicação seja vulnerável a ataques de força bruta um invasor pode enviar várias requisições até acertar a senha ou código que a aplicação pede. Este cenário é muito prejudicial em aplicações que geram uma senha fraca para usuários ou em autenticação baseada em dois fatores que exigem um código OTP.
2. **Falhas de lógica ou a má implementação do sistema de autenticação** podem levar invasores a contornarem totalmente o sistema de autenticação, quando isso ocorre geralmente temos uma vulnerabilidade de "Broken Authentication".

### Qual impacto de uma vulnerabilidade de autenticação?

Vulnerabilidades de autenticação apresentam em muitos casos, risco **alto** ou **crítico**, isso tudo depende de quais dados você consegue obter acesso através da falha de autenticação, ou seja, caso você consiga obter acesso a uma conta com altos privilégios o risco pode ser crítico, pois em muitos casos você poderá modificar dados da aplicação e terá alguns dados para construir uma nova superfície de ataque. A questão para classificar o risco de uma vulnerabilidade é saber o que você conseguiria fazer com os dados obtidos e quais danos poderiam ser causados através daquela vulnerabilidade.

---

<h1 align=center>Exploração</h1>

<h3 align=center>Vulnerabilidades de login baseadas em senha</h3>

Muitas aplicações utilizam usuário e senha como método principal de autenticação, isso significa que, caso um invasor conseguisse obter o usuário (podendo ser um nome de usuário ou um e-mail) e a senha referente a esse usuário, o invasor conseguiria se autenticar com sucesso. Há algumas maneiras de realizar isso, o que veremos abaixo:

**Ataques de força bruta:**<br>
O ataque de força bruta se baseia em tentativa e erro para identificar credenciais válidas de um usuário. Nem sempre os ataques de força bruta são realizados de forma aleatória, invasores podem utilizar uma lista de senhas padrões ou uma lista de senhas vazadas para tentar obter a senha de um usuário. Os ataques de força bruta geralmente são realizados a partir de uma ferramenta como o Hydra, que pode realizar milhares de requisições por minuto para tentar obter as credências válidas de um usuário. É importante ressaltar que para um ataque de força bruta ser bem-sucedido é necessário conhecer a aplicação que esta sendo explorada, em muitos casos as aplicações geram uma senha, padrão para os novos usuários ou não aplicam políticas de segurança para a construção de uma senha, isso acaba fazendo com que usuários criem senhas fracas o que pode ser um sério risco em ataques de força bruta.

**Políticas de senha:**<br>
Como falado acima é extremamente importante que as aplicações exijam uma política de senha forte, isto é, a aplicação deve exigir aos usuários criarem uma senha forte com caracteres especiais, números, letras maiúsculas e minúsculas. Isso irá prevenir ataques de força bruta e no caso das senhas serem criptografadas no banco de dados, também irá prevenir ataques de descriptografia através de algorítimos de descriptografia baseados em força bruta. Aqui vai algumas dicas para estabelecer uma boa política de senhas:

- Exija um número mínimo de caracteres para a criação de uma senha, muitas aplicações utilizam no mínimo 8 caracteres para criar uma senha, isso pode ser suficiente, mas em casos de ataques de força bruta, 8 caracteres ainda podem ser quebrados e descobertos.
- Exija uma mistura de caracteres maiúsculas e minúsculas na criação de uma senha.
- Exija pelo menos, um caractere especial na criação de uma senha.

**Ataques de força bruta em nomes de usuário:**<br>
Os ataques de força bruta em nomes de usuário são mais complicados, pois, você precisará ter um conhecimento prévio sobre a vítima que deseja atacar para montar uma lista com possíveis nomes de usuário que sua vítima utiliza. No caso de um administrador isso se torna um pouco mais fácil caso tenham e-mails de contato no conteúdo do site, ou se a aplicação utilize usuários comuns como "Administrador" ou "Admin" para realizar login em seus sistemas administrativos. Ainda há outros métodos para descobrir o nomes de usuários, esses métodos serão apresentados ao decorrer do artigo.

**Ataques de força bruta em senhas:**<br>
Os ataques de força bruta em senhas requerem uma boa atenção e inteligencia do atacante, para um ataque bem-sucedido deve-se observar se a aplicação: exige uma política de senha forte, teve credenciais vazadas (o que pode se tornar uma wordlist) e/ou gera senhas (padrões) para os usuários. O maior vetor para um ataque de força bruta é o comportamento humano, geralmente usuários criam senhas em que possam se lembrar, com basse nisso que as wordlists são criadas. Por exemplo, aqui estão alguns dos dados mais comuns que são inseridos em uma senha:

- Datas de nascimento;

- Número de residências;

- Nomes de pessoas ou animais de estimação;

- Nome de cidades, estados ou países;

> _Wordlist:_ uma wordlist é uma lista de palavras para serem utilizadas em um ataque de força bruta, geralmente as wordlists contém as senhas mais utilizadas por usuários e/ou vazadas em ataques crackers. A maioria das vezes os ataques de força bruta são automatizados por uma ferramenta de forma que, é apenas especificado a wordlist a ser utilizada no ataque.

**Enumeração de usuários:**<br>
A enumeração de usuários consiste em observar uma alteração no comportamento da aplicação ao inserir um usuário existente, através dessa alteração é possível determinar se um usuário existe ou não na aplicação. A diversos métodos para realizar esta enumeração, alguns locais em que podem ser observados alterações no comportamento da aplicação são:

- **Formulário de redefinição de senha:** geralmente os formulários de redefinição de senha exibem uma mensagem ao não encontrar o usuário para que devem enviar o link de redefinição, gerando uma alteração no comportamento da aplicação.
- **Formulário de login:** em algumas situações ao tentar realizar o login com um usuário inexistente, a aplicação pode retornar uma mensagem dizendo que o usuário não existe, gerando uma alteração no comportamento da aplicação.
-  **Formulário de Cadastros**: ao tentarmos cadastrar um novo usuário que já exista na aplicação, ela pode nos retornar uma mensagem dizendo que o usuário inserido já existe, portanto, esta mensagem gera uma alteração no comportamento da aplicação.

Essas pequenas alterações podem resultar na exploração da enumeração de usuários, é muito importante ficar atento a cada alteração que a aplicação sofre ao inserir um usuário, essas alterações podem ser observadas em **códigos de status**, **mensagens de erro** e **tempos de resposta** diferentes dos demais. Para prevenir a enumeração de usuários:

- Sempre utilize os mesmos códigos de status para não gerar uma alteração no comportamento da aplicação, evite utilizar códigos de status diferentes ao inserir um dado correto, por exemplo, caso um dado esteja correto exibir um código 302 (Found), isso fará com que seja perceptível a diferença entre um usuário correto (302 Found) e um incorreto (200 OK), portanto evite.
- Utilize mensagens de erros padrões, por exemplo: "Usuário e/ou senha, incorretos." ao em vez de "Usuário incorreto".
- Evite diferenças no tempo de resposta, muitas aplicações verificam primeiro se o usuário está correto e caso ele esteja, aplicação valida a senha, isso pode causar uma diferença no tempo de resposta entre um usuário inexistente e um usuário existente.

---

<h3 align=center>Vulnerabilidades na autenticação de multi-fator</h3>

A medida que a tecnologia evolui os sistemas de segurança também devem seguir esse mesmo caminho, atualmente muitas aplicações estão disponibilizando para seus usuários a opção de autenticar-se utilizando dois fatores (2FA) para melhorar a segurança. A autenticação de dois fatores utiliza como seu primeiro fator sua senha (algo que você sabe), e como segundo fator, códigos OTP (algo que você sabe) ou chaves de segurança (algo que você tem). Infelizmente ainda não chegamos ao ponto de utilizarmos impressões digitais ou reconhecimento facial (algo que você é) como um segundo fator em aplicações WEB.

É importante ressaltar que o intuito da autenticação de dois fatores é utilizar dois fatores destintos para autenticar um usuário, há um grande debate entre o primeiro fator (senha) e o segundo fator (códigos OTP), pois, os dois são tecnicamente o mesmo fator (algo que você conhece). Sabendo disso devemos evitar utilizar o e-mail e SMS para realizar uma validação do segundo fator, isso porque tanto o e-mail quanto o SMS se baseiam no mesmo fator (algo que você conhece), isso significa que, caso um invasor tenha acesso ao e-mail da vítima o segundo fator não será eficaz. Isso também serve para o SMS, além dos mais devemos evitar ao máximo utilizar o SMS como fator de autenticação já que, as comunicações (SMS) podem ser interceptadas por um invasor. No caso dos códigos OTP eles são mais seguros, pois, possuem um tempo curto para um código expirar e outro código válido ser gerado no lugar. Lembre-se: "é tão seguro quanto a sua implementação".

**Ignorando a autenticação de dois fatores (Forced Browsing):**<br>
Muitas aplicações fazem a verificação da autenticação de dois fatores em outra página, e muitas vezes a autenticação já é concluída antes mesmo de você passar pelo segundo fator. Para maior entendimento vamos criar um exemplo em que você se autenticou corretamente utilizando seu e-mail e a sua senha em 'https://site.com/login' após isso você foi redirecionado para realizar a autenticação de dois fatores em 'https://site.com/2fa', porém, quando você foi redirecionado a aplicação já validou sua autenticação e lhe enviou os cookies de sessão, isso significa que mesmo você estando em 'https://site.com/2fa' você pode ir diretamente para 'https://site.com/painel' sem inserir nenhum código ou chave para autenticar-se no segundo fator. Em resumo a aplicação não valida completamente o processo de autenticação por dois fatores, validando somente o primeiro fator, no caso seu usuário e senha. Esta vulnerabilidade é fruto de uma má implementação que pode ser explorada ignorando totalmente o segundo fator de autenticação.

**Ignorando a autenticação de dois fatores (Brute-Force):**<br>
Assim como nas senhas é essencial criarmos métodos de proteção contra, ataques de força bruta no segundo fator da autenticação, nesta situação caso a aplicação não realize nenhum bloqueio após determinadas tentativas o segundo fator pode ser facilmente ignorado através de um ataque de força bruta. No caso da autenticação de dois fatores baseados em códigos OTP por se tratar de códigos numéricos de 4 a 6 dígitos, um ataque de força bruta seria certeiro e o segundo fator poderia ser facilmente ignorado.

**Ignorando a autenticação de dois fatores (Broken Logic):**<br>
A vulnerabilidade de Broken Logic é basicamente um problema na implementação da autenticação de dois fatores. Depois que um usuário concluí corretamente o primeiro fator da autenticação a aplicação não valida se o mesmo usuário está concluindo o segundo fator. Para melhor compreensão veja o exemplo abaixo:

Um invasor utilizando sua conta, concluí o primeiro fator da autenticação (usuário e senha) e é redirecionado para realizar o segundo fator.

Request:
> POST /login HTTP/2.0
Host: site.com  
...  
username=invasor&password=123

Response:
> HTTP/2.0 200 OK  
Set-Cookie: account=invasor

>GET /2fa HTTP/2.0
Cookie: account=invasor

Após ser redirecionado, o atacante insere seu código OTP de verificação, porém, muda o seu cookie de 'invasor' para 'vitima'. Confira:

De:
>POST /2fa HTTP/2.0  
Host: site.com  
Cookie: account=invasor  
...  
verification-code=123456

Para:
>POST /2fa HTTP/2.0
Host: site.com 
Cookie: account=vitima  
...  
verification-code=123456

Neste caso o invasor concluiu o primeiro fator, e no segundo fator ele modificou seu usuário para o usuário em que gostaria de obter acesso. Essa vulnerabilidade está atrelada a força bruta, caso o invasor consiga realizar um ataque de força bruta no código OTP ele será capaz de acessar qualquer conta, precisando apenas do nome de usuário da conta que deseja acessar. Esta vulnerabilidade com a combinação de um ataque de força bruta no código OTP representa um risco **Alto** ou **Crítico**, pois, compromete a conta de qualquer usuário apenas sabendo seu nome de usuário.

---

<h3 align=center>Referências</h3>

Este artigo foi baseado nos contéudos da [PortSwigger](https://portswigger.net/). O contéudo deste artigo foi retirado de [Authentication vulnerabilities](https://portswigger.net/web-security/authentication), traduzido e adaptado por mim. Todos os créditos vão para a [PortSwigger](https://portswigger.net/) que desenvolveu um contéudo incrível e possibilitou a criação deste artigo.
<br>
<br>
<p align=right>Publicado por D1G0 ás <strong>21:45 13/10/2021</strong></p>
