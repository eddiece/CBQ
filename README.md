# CBQ
Controle de Tráfego de Rede usando CBQ - Class Based Queueing

## Este repositório contém o trabalho desenvolvido para a 3ª Unidade de Sistemas Multimidia 

Foi criada uma rede virtual baseada em vagrant e virtualbox conforme o seguite link:


[Rede Vitual](https://gitlab.com/lssn.oliveira/sm171machines)





**Motivação**

A necessidade de gerenciar uma ou mais redes com usuários dos mais diversos tipos,
as vezes tendo que levar em consideração um range de redes, um deteminado horário
ou até mesmo um determinado tipo de aplicação.

Para solucionar este problemas vamos usar a ferramenta **shaper**.
Em versões mais antigas do Debian e em outras distros esse pacote pode ter o nome de CBQ.

**CBQ** vem de (Class Based Queueing) ou seja **Enfileiramento baseado em classe**.

O Shaper trabalha na camada de kernel do sistema controlado a velocidade de entrada e saída de  
carga para hosts ou redes conforme configurado.

Ele pode ser instalado em distribuições debian a partir do comando:

```
sudo apt-get install shaper

```
Ou pode ser baixado a partir deste [link](https://downloads.aprendendolinux.com/programas/shaper_2.2.12-0.7.3-2.2_all.deb ) e instalado usando:


```
sudo dpkg -i shaper_2.2.12-0.7.3-2.2_all.deb

```
A configuração é feita a partir de arquivos colocados no diretório **/etc/shaper/** contendo o nome da seguinte forma:

```
cbq-XXXX-identificação que preferir-down

```
`Detalhe:` "XXXX" deve ser um identificador em hexadecimal começando por 0002 e indo até ffff

Os arquivos de configuração podem ser organizados da seguinte forma:

**Para Download**

```
#DEVICE -> Placa de rede que será tratada nessa regra
DEVICE=eth0:1,100Mbit,10Mbit

#RATE – -> Velocidade de Download total dessa Placa
RATE=30720Kbit ## 30MB de Download (1024x30)

#WEIGHT –> Velocidade de Download total dessa Placa (dividida por 10)
WEIGHT=3072Kbit ## 30kbit de Download (30/10)

#PRIO – Prioridade vai de 1 à 8 quanto mais alto menor a prioridade, o valor padrão é 5
PRIO=5

#RULE – Range de IP que sera tratada nessa regra
RULE=192.168.1.0/24 ## Range da minha rede local/interna

#BOUNDED – Não permite compartilhar banda entre as redes mesmo se tiver banda excedente
BOUNDED=yes

#ISOLATED – Banda excedente não sera compartilhada
ISOLATED=yes

```

**Para Upload**
```
#DEVICE -> Placa de rede que será tratada nessa regra
DEVICE=eth0:1,100Mbit,10Mbit

#RATE – -> Velocidade de Upload total dessa Placa
RATE=30720Kbit ## 30MB de Upload (1024x30)

#WEIGHT –> Velocidade de Upload total dessa Placa (dividida por 10)
WEIGHT=3072Kbit ## 30kbit de Upload (30/10)

#PRIO – Prioridade vai de 1 à 8 quanto mais alto menor a prioridade, o valor padrão é 5
PRIO=5

#RULE – Range de IP que sera tratada nessa regra
RULE=192.168.1.0/24, ## Range da minha rede local/interna

#BOUNDED – Não permite compartilhar banda entre as redes mesmo se tiver banda excedente
BOUNDED=yes

#ISOLATED – Banda excedente não sera compartilhada
ISOLATED=yes

```
Existem também configurações mais detalhadas, como por exemplo

```
#Essa regra controla o trafego apenas na  porta 80 da rede local/interna
RULE=144.14.4.0/24:80 # Não esqueça de colocar "," na regra de upload do outro arquivo

#Essa regra controla o trafego de um IP apenas na porta 80
RULE=144.14.4.35:80 # Não esqueça de colocar "," na regra de upload do outro arquivo

#Essa regra seleciona apenas na minha rede local/interna
RULE=144.14.4.0/24 # Não esqueça de colocar "," na regra de upload do outro arquivo

#Controla o trafego vindo de qualquer origem, com destino entre a porta 50 a porta 5000
RULE=:25,144.14.4.0/24:5000

#Limita a banda em 5MB das 18:00 da noite até as 06:00 da manha
#TIME=(hora inicial)-(hora final);(velocidade)/(velocidade/10)
TIME=18:00-06:00;5120Kbit/512Kbit ## (1024z5=5120), (5120/10=512) 

```
# Testes de desempenho

Foi usado o software **speedometer** para verificar o desempenho da limitação de largura de banda
transferindo um arquivo de cerca de 4Gb e os limites estipulados foram respeitados e a largura de 
banda era dividida por igual pelos usuários no range estipulado.

Usamos também o **iperf** tendo um servidor e um cliente para gerar trafego e depois medi-lo  
O resultado foi que os limites eram sempre respeitados.

# Observações

O CBQ apresenta outros recursos, alguns que valem citar:

*controle sobre a banda excedente;  
*possibilidade de criação e utilização de classes;

# Referências

http://opentech.etc.br/controle-de-banda-de-internet-no-linux-com-o-shaper/  
https://br-linux.org/artigos/dicas_cbq.htm  
https://www.linuxdescomplicado.com.br/2014/12/10-ferramentas-para-monitorar-largura.html  
https://blog.da2k.com.br/2015/02/08/aprenda-markdown/  



