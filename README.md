# Reverse shell com metasploit e bypass de defesas

### AVISO IMPORTANTE: Este guia existe somente para fins didáticos e sobre hipotese alguma replique para fins maliciosos. Cibersegurança é um assunto sério e se usado para prejudicar alguém pode trazer consequências sérias.


Este guia resume os passos exatos para obter uma sessão
Meterpreter em uma VM Windows 10 a partir do Kali Linux , superando as barreiras de rede NAT e
defesas ativas (AV e SmartScreen).

## 1 VM windows 10
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









