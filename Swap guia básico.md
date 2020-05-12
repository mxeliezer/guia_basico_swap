Guia Básico Swap
================
### O que é swap? ###
**Swap** é uma área num disco rígido que foi designada como um local 
onde o sistema operativo pode armazenar temporariamente dados que
já não pode guardar na **RAM**. Isto aumenta a quantidade de informação
que o seu servidor pode manter na sua "memória" funcional.

Neste documento apresento alguns comandos importantes para configuração
de sua memória swap, especialmente para OS: **Ubuntu 18.04**.
As linhas de comando usam shell scrpting: **BASH**.
Os tópicos estão apresentados nesta ordem:

1. Verificação dos espaços de memória;
2. Criação de um novo swapfile (se for necessário);
3. Configuração básica do seu swap.

### Verfificando espaços de memória ###
Para ver a memória total, usada e livre: **RAM** e **swap**.
```sh
free -h
```
Para ver os arquivos **swap**, tamanho e uso.
```sh
sudo swapon --show
```
### Checando espaços nas partições do HD ###
Mostra espaços em disco sda1 e sda2, ocupado e livre.
```sh
df -h
```
### Criando um novo swap file ###
Este comando cria instantaneamente um ficheiro de tamanho pré-alocado.
O tamanho varia conforme cada caso. Para um PC com RAM >= 4 G,
um swap com 1G deve ser suficiente. Mas aqui deves pesquisas qual 
a melhor opção para si.
```sh
sudo fallocate -l 1G /path/to/swapfile/
```
Depois disto é preciso bloquear as permissões de acesso ao ficheiro, 
para apenas root (por questões de segurança):
```sh
sudo chmod 600 /path/to/swapfile/
```
Verificando as permissões alteradas:
```sh
ls -lh /path/to/swapfile/
```
Marcando o novo swapfile como uma swap válida:
```sh
sudo mkswap /path/to/swapfile/
```
Ligando o novo swap:
```sh
sudo swapon /path/to/swapfile/
```
Para tornar o swapfile permamente, é preciso adicionar o 
ficheiro swap ao nosso ficheiro **/etc/fstab**:
```sh
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```
Desligando o novo swap, se necessário:
```sh
sudo swapoff /path/to/swapfile/
```
### Configurando o swap ###

São dois parâmetros principais de configuração:

1. **swappiness**: configura a frequência com que o seu sistema
troca os dados da RAM para o espaço swap. Sempre um valor entre 0 e 100.
O valor ideal varia caso a caso. Ubuntu por padrão tem o valor 60.
2. **vfs_cache_pressure**: configura o quanto o sistema irá escolher
para armazenar informações inode e dentry em vez de outros dados. 
Por padrão o Ubuntu tem o valor 100.

#### 1. swappiness: ####

Para verificar o valor atual:
```sh
cat /proc/sys/vm/swappiness
```
Para alterar temporariamemte o valor:
```sh
sudo sysctl vm.swappiness=novo valor
```
A alteração permanente exige editar o arquivo **/etc/sysctl.conf**
```sh
sudo nano /etc/sysctl.conf
```
Adiciona ao fim do arquivo: 
```sh
vm.swappiness=novo valor
```

#### 2. vfs_cache_pressure: ####

Para verificar o valor atual:
```sh
cat /proc/sys/vm/vfs_cache_pressure
```
Para alterar temporariamemte o valor:
```sh
sudo sysctl vm.vfs_cache_pressure=novo valor
```
Alteração permanente exige editar o arquivo **/etc/sysctl.conf**
```sh
sudo nano /etc/sysctl.conf
```
Adiciona fim do arquivo: 
```sh
vm.vfs_cache_pressure=novo valor
```
## Considerações finais ##

É preciso usar o **swap** com parcimônia, pois podem haver riscos
de ataques à memória ali alocada. Há bastate informação documentada
na web, e recomendo a leitura da documentação de referência.

Referências:

[DigitalOcean community](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-16-04)

[Ubuntu documentation: swap faq](https://help.ubuntu.com/community/SwapFaq#What_is_swap.3F)