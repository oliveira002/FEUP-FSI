## Buffer Overflow Attack Lab (Set-UID Version)

## Week 5 CTF Challenge 
### Challenge 1
Inicialmente, é-nos dado um ficheiro .zip que contém um script python, um ficheiro chamado flag.txt, um pedaço de codigo em C
um ficheiro de texto mem.txt e um ficheiro chamado 'program' que é a versão compilada do codigo em C.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1032673693932998666/unknown.png">

Fazendo a análise de permissões de compilação do programa podemo-nos aperceber que a arquitetura do ficheiro é x86 (Arch), não existe um cannary a proteger o return address (Stack), a stack tem permisssão de execução (NX), e as posições do binário não estão randomizadas (PIE), por fim existem regiões de memória com permissões de leitura, escrita e execução (RWX), neste caso referindo-se à stack.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1032702405499965490/unknown.png">

Analizando o scrip python, reparamos que o seu comportamento varia, dependendo do valor da flag booleana DEBUG.
Se DEBUG = True, o script executa o programa program, mas, se DEBUG = False, o script conecta-se ao servidor ctf-fsi.fe.up.pt através da porta 4003. Isto permite-nos testar as nossas estratégias, antes de as por em prática no servidor. Logo após, uma string à nossa escolha é enviada para o programa/servidor.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1032673528614486046/unknown.png">

Após perceber o funcionamento do script python, passamos à análize do pedaço de código C.
Inicialmente, são criados dois arrays de char, meme_file e buffer, cada um contendo, no máximo, 8 e 20 chars, respetivamente. O array meme_file contém a string "mem.txt\0".
De seguida, é lida uma string da consola, que vai ser alocada na buffer, o conteúdo alocado vai ser impresso no ecrã e um ficheiro cujo nome está contido no array meme_file é aberto e o seu conteudo impresso.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1032698359703687299/unknown.png">

Aquando da execução do programa program, normalmente, o conteúdo do ficheiro mem.txt seria mostrado ao utilizador, porém, é possível realizar um ataque de buffer overflow, devido à falta de meios de prevenção da função scanf(), que não tem em atenção o tamanho da string de input quando tenta escrever na buffer, resultando em overwrite de outros endereços de memória exteriores à buffer, neste caso, os do array meme_file. Deste modo, é possivel indicar ao programa qual ficheiro abrir, independentemente do ficheiro inicialmente escolhido. Isto pode ser usado a nosso favor para ler os conteudos do ficheiro flag.txt, dando ao programa uma string qualquer de 20 chars imediatamente seguida do nome do ficheiro pretendido.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1032699870991110194/unknown.png">

Agora que sabemos o metodo de ataque, utilizando o script python fornecido, com a devida string de input, basta apenas executar o script para obter a flag. Ans: flag{d32ff4f65fae804137cfef1fff0fdc5a}

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1032701222798508062/unknown.png">

<br> TLDR: <br>
Existe algum ficheiro que é aberto e lido pelo programa?<br>
- Sim, o ficheiro mem.txt.<br>

Existe alguma forma de controlar o ficheiro que é aberto?<br>
- Sim, alterando o conteúdo do array meme_file.<br>

Existe algum buffer-overflow? Se sim, o que é que podes fazer?<br>
- Sim, utilizando a falta de proteções da função scanf() é possivel reescrever endereços de memória que não pertençam à buffer indicada. Deste modo podemos alterar o conteudo do array meme_file para ser o nome do ficheiro que desejamos ler.

### Challenge 2

O segundo desafio é semelhante ao primeiro, na medida em que o metodo e o objetivo são os mesmos, apenas com pequenas alterações.
A estrutura do ficheiro .zip que nos é dado é a mesma, sendo o código em C a maior alteração.
Fazendo a análise de permissões de compilação do programa chegamos às mesmas conclusões que anteriormente.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1032704513112551465/unknown.png">

Passando à análize às alterações feitas ao código C, deparamo-nos com uma buffer secundária, cujo objetivo unico é verificar alterações feitas utilizando buffer overflow. Utilizando a mesma técnica do desafio anterior, alterando o seu conteúdo para o valor 0xfefc2223, é possível dar bypass à verificação imposta, lendo assim o ficheiro desejado, ao invés do ficheiro mem.txt.

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1032710222453669979/unknown.png">

Assim sendo, introduzindo uma string the 20chars, seguido do valor pretendido (0xfefc2223), tendo em atenção que a arquitetura utilizada é little-endian, o que significa que o byte menos significativo é guardado primeiro, sendo necessário inverter a ordem de introdução dos bytes, seguido do nome do ficheiro pretendido, é possível obter a flag. Ans: flag{919a6b96138dac079a9fb72efe6e8985}

<img src="https://cdn.discordapp.com/attachments/799728570825179213/1032711160581070988/unknown.png">

<br> TLDR: <br>
Que alterações foram feitas?<br>
- Foi criada uma buffer secundária que verifica se houve alterações do seu conteúdo.<br>

Mitigam na totalidade o problema?<br>
- Não, pois é possível reescrever o conteúdo da mesma de forma a entrar no *if* de verificação.<br>

É possivel ultrapassar a mitigação usando uma técnica similar à que foi utilizada anteriormente?<br>
- Sim, basta escrever 20 chars na buffer original, seguidos do valor de verificação da buffer secundária, seguido do nome do ficheiro prentendido.<br>

