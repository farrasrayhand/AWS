# Módulo 4 – Laboratório guiado: Apresentação do Amazon Elastic File System (Amazon EFS)
[//]: # "SKU: ILT-TF-200-ACACAD-2    Source Course: SPL-151"

## Visão geral e objetivos do laboratório
Este laboratório apresenta o Amazon Elastic File System (Amazon EFS) usando o Console de Gerenciamento da AWS.

Depois de concluir este laboratório, você deverá ser capaz de:

- Fazer login no Console de Gerenciamento da AWS

- Criar um sistema de arquivos do Amazon EFS

- Fazer login em uma instância do Amazon Elastic Compute Cloud (Amazon EC2) que executa o Amazon Linux

- Montar seu sistema de arquivos na instância do EC2

- Examinar e monitorar a performance do sistema de arquivos

<br/>

## Duração

O laboratório requer aproximadamente **20 minutos** para ser concluído.

<br/>

## Restrições do produto da AWS

Neste ambiente de laboratório, o acesso aos serviços e ações de serviço da AWS pode estar restrito àqueles necessários para concluir as instruções do laboratório. Você poderá encontrar erros se tentar acessar outros serviços ou executar ações além do que está descrito neste laboratório.

<br/>

## Acessar o Console de Gerenciamento da AWS

1. Na parte superior destas instruções, escolha <span id="ssb_voc_grey">Start Lab</span> (Iniciar laboratório) para iniciar o laboratório.

   Um painel **Start Lab** (Iniciar laboratório) é aberto com o status do laboratório.

   <i class="fas fa-info-circle"></i> **Dica**: se você precisar de mais tempo para concluir o laboratório, escolha novamente o botão <span id="ssb_voc_grey">Start Lab</span> (Iniciar laboratório) para reiniciar o cronômetro do ambiente.

2. Aguarde até que o painel **Start Lab** (Iniciar laboratório) exiba a mensagem *Lab status: ready (Status do laboratório: pronto)* e, em seguida, escolha **X** para fechar o painel.

3. Na parte superior destas instruções, escolha <span id="ssb_voc_grey">AWS</span>.

   Essa ação abrirá o Console de Gerenciamento da AWS em uma nova guia do navegador. O sistema fará seu login automaticamente.

   <i class="fas fa-exclamation-triangle"></i> **Dica**: se uma nova guia do navegador não for aberta, normalmente você verá uma faixa ou um ícone na parte superior do navegador com uma mensagem informando que o navegador está impedindo que o site abra janelas pop-up. Escolha a faixa ou o ícone e depois **Allow pop ups** (Permitir pop-ups).

4. Organize a guia do **Console de Gerenciamento da AWS** para que ela seja exibida com essas instruções. O ideal é ver as duas guias do navegador abertas ao mesmo tempo, para facilitar o acompanhamento das etapas do laboratório.

   <i class="fas fa-exclamation-triangle"></i> **Não altere a região, a menos que seja especificamente instruído a fazer isso**.

<br/>

## Tarefa 1: criar um grupo de segurança para acessar o sistema de arquivos do EFS

O grupo de segurança que você associa a um destino de montagem *deve permitir acesso de entrada para TCP na porta 2049 para NFS (Network File System)*. Este é o grupo de segurança que você criará, configurará e associará aos destinos de montagem do EFS.


5. No **Console de Gerenciamento da AWS**, no menu <span id="ssb_services">Services (Serviços)</span>, escolha **EC2**.

6. No painel de navegação à esquerda, escolha **Security Groups** (Grupos de segurança).

7. Copie o **ID do grupo de segurança** do grupo de segurança *EFSClient* para o editor de texto.

   O ID do grupo será semelhante a *sg-03727965651b6659b*.

8. Escolha <span id="ssb_orange">Create security group (Criar grupo de segurança)</span> e depois configure:

   <a id='securitygroup'></a>

   * **Security group name** (Nome do grupo de segurança): `EFS Mount Target (Destino de montagem do EFS)`
   * **Description** (Descrição): `Inbound NFS access from EFS clients (Acesso NFS de entrada de clientes do EFS)`
   * **VPC:** *Lab VPC*

9. Na seção **Inbound rules** (Regras de entrada), escolha <span id="ssb_white">Add rule</span> (Adicionar regra) e configure:

   * **Type** (Tipo): *NFS*
   * **Source** (Origem):
      * *Custom (Personalizado)*
      * Na caixa *Custom (Personalizado)* cole o **ID do grupo de segurança** que você copiou para o editor de texto
   * Escolha <span id="ssb_orange">Create security group (Criar grupo de segurança)</span>.

<br/>

## Tarefa 2: criar um sistema de arquivos do EFS

Os sistemas de arquivos do EFS podem ser montados em várias instâncias do EC2 que são executadas em diferentes zonas de disponibilidade na mesma região. Essas instâncias usam *destinos de montagem* criados em cada *zona de disponibilidade* para montar o sistema de arquivos usando a semântica NFSv4.1 padrão. Você pode montar o sistema de arquivos em instâncias em apenas uma Virtual Private Cloud (VPC) por vez. Tanto o sistema de arquivos quanto a VPC precisam estar na mesma região.


10. No menu <span id="ssb_services">Services (Serviços)</span>, escolha **EFS**.

11. Escolha <span id="ssb_orange">Create file system</span> (Criar sistema de arquivos)

12. Na janela **Create file system** (Criar sistema de arquivos), escolha <span id="ssb_white">Customize</span> (Personalizar)

13. Na **Etapa 1**:

    - Desmarque <i class="far fa-square"></i>Enable automatic backups (Habilitar backups automáticos).
    - **Lifecycle management** (Gerenciamento do ciclo de vida): Selecione *None (Nenhum)*
    - Na seção **Tags**, configure:
       - **Key** (Chave): `Name (Nome)`
       - **Value** (Valor): `My First EFS File System (Meu primeiro sistema de arquivos do EFS)`

14. Escolha <span id="ssb_orange">Next</span> (Próximo)

15. Para **VPC**, selecione *Lab VPC*.

16. Desassocie o grupo de segurança padrão de cada destino de montagem da *zona de disponibilidade* marcando a caixa de seleção <i class="fas fa-times"></i> em cada grupo de segurança padrão.

17. Associe o grupo de segurança **EFS Mount Target** (Destino de montagem do EFS) a cada destino de montagem da *zona de disponibilidade* da seguinte maneira:

    * Marque cada caixa de seleção **Security groups** (Grupos de segurança).
    * Escolha **EFS Mount Target** (Destino de montagem do EFS).

    Um destino de montagem é criado para cada sub-rede.

    Seus alvos de montagem devem ser parecidos com o exemplo a seguir. O diagrama mostra dois alvos de montagem na **Lab VPC** que usam o grupo de segurança **EFS Mount Target**. Neste laboratório, você deve usar o Lab VPC.

    <img src="images/mount-targets-security-groups.png" alt="Grupos de segurança de destino" width="600" >

18. Escolha <span id="ssb_orange">Next</span> (Próximo)

19. Na **Etapa 3**, escolha <span id="ssb_orange">Next</span> (Próximo)

20. Na **Step 4** (Etapa 4):

    * Revise sua configuração.
    * Escolha <span id="ssb_orange">Create</span> (Criar)

    <i class="far fa-thumbs-up"></i> Parabéns! Você criou um novo sistema de arquivos do EFS na Lab VPC e montou destinos em cada sub-rede da Lab VPC. Em alguns segundos, o **File system state** (Estado do sistema de arquivos) mudará para *Available (Disponível)*, seguido pelos destinos de montagem 2 a 3 minutos depois.

    Prossiga para a próxima etapa após o **Mount target state** (Estado do destino de montagem) de cada destino de montagem mudar para *Available (Disponível)*. Escolha o botão de atualização de tela após 2 a 3 minutos para verificar o progresso.

<br/>

## Tarefa 3: conectar-se à instância do EC2 via SSH

Nesta tarefa, você se conectará à instância do EC2 usando SSH (Secure Shell).

### <i class="fab fa-windows"></i> Usuários do Microsoft Windows

<i class="fas fa-comment"></i> Estas instruções são especificamente para usuários do Microsoft Windows. Se você estiver usando macOS ou Linux, <a href="#ssh-MACLinux">vá para a próxima seção</a>.
​

21. Acima destas instruções que você está lendo no momento, escolha o menu suspenso <span id="ssb_voc_grey">Details</span> (Detalhes) e, em seguida, selecione <span id="ssb_voc_grey">Show</span> (Mostrar)

Uma janela **Credentials** (Credenciais) é aberta.

22. Escolha o botão **Download PPK** (Fazer download do PPK) e salve o arquivo **labsuser.ppk**.

    **Observação:** normalmente, o navegador salva o arquivo no diretório **Downloads**.

23. Observe o endereço **EC2PublicIP**, se for exibido.

24. Saia do painel **Details** (Detalhes) escolhendo o **X**.

25. Para usar o SSH para acessar a instância do EC2, você deve usar o ***\*PuTTY\****. Se não tiver o PuTTY instalado no seu computador, <a href="https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe">faça download dele</a>.

26. Abra o **putty.exe**.

27. Configure o tempo limite do PuTTY para manter a sessão do PuTTY aberta por mais tempo:

    * Escolha **Connection** (Conexão)
    * **Seconds between keepalives** (Segundos entre os keepalives): `30`

28. Defina a sessão do PuTTY com as configurações a seguir.

    * Escolha **Session** (Sessão)
    * **Host Name** (or IP address) (Nome do host [ou endereço IP]): cole o **EC2PublicIP** para a instância que você anotou anteriormente
       * Como alternativa, volte para o console do Amazon EC2 e escolha **Instances** (Instâncias)
       * Selecione a instância à qual deseja se conectar
       * Na guia *Description (Descrição)*, copie o valor **IPv4 Public IP** (IP público IPv4)
    * De volta ao PuTTY, na lista **Connection** (Conexão), expanda o <i class="far fa-plus-square"></i> **SSH**
    * Escolha **Auth** (Autenticação) (mas não expanda)
    * Escolha **Browse** (Procurar)
    * Navegue até o arquivo *labsuser.ppk* obtido por download, selecione-o e escolha **Open** (Abrir)
    * Escolha **Open** (Abrir) novamente


29. Para confiar e se conectar ao host, escolha **Yes** (Sim).

30. Quando for solicitado que você faça login como (**login as**), digite: `ec2-user`

    Essa ação conecta você à instância do EC2.

31. Usuários do Microsoft Windows: <a href="#ssh-after">Escolha este link para avançar para a próxima tarefa.</a>



<a id='ssh-MACLinux'></a>

### Usuários de macOS <span style="font-size: 30px; color: #808080;"><i class="fab fa-apple"></i></span> e Linux <span style="font-size: 30px; "><i class="fab fa-linux"></i></span>

Essas instruções são especificamente para usuários de macOS ou Linux. Se você é um usuário do Windows, <a href="#ssh-after">avance para a próxima tarefa.</a>

32. Acima destas instruções que você está lendo no momento, escolha o menu suspenso <span id="ssb_voc_grey">Details</span> (Detalhes) e, em seguida, selecione <span id="ssb_voc_grey">Show</span> (Mostrar)

    Uma janela **Credentials** (Credenciais) é aberta.

33. Escolha o botão **Download PEM** (Fazer download do PEM) e salve o arquivo **labsuser.pem**.

34. Observe o endereço **EC2PublicIP**, se for exibido.

35. Saia do painel **Details** (Detalhes) escolhendo o **X**.

36. Abra uma janela do terminal e altere o diretório para aquele no qual foi feito download do arquivo *labsuser.pem* usando o comando `cd`.

    Por exemplo, se o arquivo *labsuser.pem* foi salvo no diretório **Downloads**, execute este comando:

    ```bash
    cd ~/Downloads
    ```

37. Altere as permissões na chave para que se tornem somente leitura executando este comando:

    ```bash
    chmod 400 labsuser.pem
    ```

38. Execute o comando a seguir (substitua **<public-ip\>** pelo endereço **EC2PublicIP** que você copiou anteriormente).

    * Como alternativa, para localizar o endereço IP da instância on-premises, retorne ao console do Amazon EC2 e selecione **Instances** (Instâncias)
    * Selecione a instância à qual você deseja se conectar
    * Na guia **Description (Descrição)**, copie o valor **IPv4 Public IP** (IP público IPv4)

    ```bash
    ssh -i labsuser.pem ec2-user@<public-ip>
    ```

39. Quando solicitado a permitir a primeira conexão com este servidor SSH remoto, insira `yes (sim)`.

    Como você está usando um par de chaves para autenticação, não é necessário fornecer uma senha.

<a id='ssh-after'></a>

<br/>
## Tarefa 4: criar um novo diretório e montar o sistema de arquivos do EFS

<i class="fas fa-info-circle" aria-hidden="true"></i> O Amazon EFS é compatível com os protocolos NFSv4.1 e NFSv4.0 quando monta os sistemas de arquivos em instâncias do EC2. Embora o NFSv4.0 seja compatível, recomendamos que você use o NFSv4.1. Quando você monta um sistema de arquivos do EFS na instância do EC2, também deve usar um cliente NFS compatível com o protocolo NFSv4 escolhido. A instância do EC2 que foi iniciada como parte deste laboratório inclui um cliente NFSv4.1, que já está instalado nele.

40. No **Console de Gerenciamento da AWS**, no menu <span id="ssb_services">Services (Serviços)</span>, clique em **EFS**.

41. Escolha **My First EFS File System** (Meu primeiro sistema de arquivos do EFS).

42. No **Console do Amazon EFS**, no canto superior direito da página, escolha <span id="ssb_orange">Attach</span> (Associar) para abrir as instruções de montagem do Amazon EC2.

43. Na sessão SSH, siga as instruções da seção **To set up your EC2 instance** (Configurar sua instância do EC2). Copie e cole (ou digite manualmente) o comando para instalar os utilitários necessários.

    O comando deve ser parecido com este exemplo:

    ```
    sudo yum install -y amazon-efs-utils
    ```

    *Dica:* para colar no terminal do navegador, posicione o cursor à direita do prompt de comando e clique com o botão direito do mouse para ver a opção de colar.

44. Role para baixo até a seção **Mounting your file system** (Montar o sistema de arquivos).

45. Na sessão SSH, crie um novo diretório digitando `sudo mkdir efs`

46. Copie todo o comando na seção **Using the NFS client** (Usar o cliente NFS).

    O comando de montagem deve ser parecido com este exemplo:

    `sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-bce57914.efs.us-west-2.amazonaws.com:/ efs`

    <i class="fas fa-comment" aria-hidden="true"></i> O comando `sudo mount...` fornecido usa as opções de montagem padrão do Linux.

47. Na sessão SSH do Linux, monte o sistema de arquivos do Amazon EFS da seguinte maneira:

    * Cole o comando
    * Pressione ENTER


48. Consiga um resumo completo do espaço em disco disponível e utilizado digitando:

    `sudo df -hT`

    A captura de tela a seguir é um exemplo do resultado do seguinte comando *disk filesystem*:

    `df -hT`
    
    Observe o *tipo* e o *tamanho* do sistema de arquivos do EFS montado.

    <img src="images/disk-space.png" alt="espaço em disco" width="600">


<br/>
## Tarefa 5: examinar o comportamento da performance do novo sistema de arquivos do EFS


### Examinar a performance com o uso do Flexible IO

<i class="fas fa-info-circle"></i> O Flexible IO (fio) é um utilitário sintético de benchmarking de E/S para Linux. Ele é utilizado para comparar e testar subsistemas de E/S do Linux. Durante a inicialização, o *fio* foi instalado automaticamente em sua instância do EC2.

49. Examine as características da performance de gravação do sistema de arquivos digitando:

    ```
    sudo fio --name=fio-efs --filesize=10G --filename=./efs/fio-efs-test.img --bs=1M --nrfiles=1 --direct=1 --sync=0 --rw=write --iodepth=200 --ioengine=libaio
    ```

    <i class="fas fa-comment"></i> O comando `fio` levará de 5 a 10 minutos para ser concluído. O resultado deve ser parecido com o exemplo da captura de tela a seguir. Examine o resultado do comando `fio`, especificamente as informações de status resumidas para este teste de GRAVAÇÃO.

    <img src="images/fio.png" alt="fio" width="600" >

<br/>
### Monitorar a performance usando o Amazon CloudWatch

50. No **Console de Gerenciamento da AWS**, no menu <span id="ssb_services">Services (Serviços)</span>, escolha **CloudWatch**.

51. No painel de navegação à esquerda, escolha **Metrics** (Métricas).

52. Na guia **All metrics** (Todas as métricas), escolha **EFS**.

53. Escolha **File System Metrics** (Métricas do sistema de arquivos).

54. Selecione a linha que tem o nome da métrica **PermittedThroughput**.

    <i class="fas fa-comment"></i> Talvez seja necessário aguardar de dois a três minutos e atualizar a tela várias vezes antes de todas as métricas disponíveis, inclusive **PermittedThroughput**, serem calculadas e preenchidas.

55. No gráfico, escolha e arraste ao redor da linha de dados. Se você não vir o gráfico de linha, ajuste o intervalo de tempo do gráfico para exibir o período durante o qual você executou o comando `fio`.

    <img src="images/graph.png" alt="escolha e arraste" width="600" >


56. Pare o ponteiro na linha de dados no gráfico. O valor deve ser *105M*.

    <img src="images/throughput.png" alt="Taxa de transferência" width="600" >

    A taxa de transferência do Amazon EFS é dimensionada à medida que o sistema de arquivos cresce. As cargas de trabalho baseadas em arquivos normalmente têm picos. Elas impulsionam níveis altos de taxa de transferência por curtos períodos e níveis baixos no restante do tempo. Por causa desse comportamento, o Amazon EFS foi projetado para atingir altos níveis de taxa de transferência por períodos. Todos os sistemas de arquivos, independentemente do tamanho, podem atingir 100 MiB/s de taxa de transferência. Para obter mais informações sobre as características de performance do sistema de arquivos do EFS, consulte a <a href="http://docs.aws.amazon.com/efs/latest/ug/performance.html" target="_blank">documentação oficial do Amazon Elastic File System</a>.

57. Na guia **All metrics (Todas as métricas)**, *desmarque* a caixa para **PermittedThroughput**.

58. Marque a caixa de seleção **DataWriteIOBytes**.

    <i class="fas fa-comment"></i> Se você não enxergar *DataWriteIOBytes* na lista de métricas, use a pesquisa **File System Metrics** para localizar.

59. Escolha a guia **Graphed metrics** (Métricas em gráfico).

60. Na coluna **Statistics** (Estatísticas), selecione **Sum** (Soma).

61. Na coluna **Period** (Período), selecione **1 Minute** (1 minuto).

62. Pare o ponteiro no pico do gráfico de linha. Pegue este número (em bytes) e divida-o pela duração em segundos (60 segundos). O resultado fornece a taxa de transferência de gravação (B/s) do seu sistema de arquivos durante o teste.

    <img src="images/Sum-1-minute.png" alt="Soma 1 minuto" width="600" >

    A taxa de transferência disponível para um sistema de arquivos é dimensionada conforme o sistema de arquivos cresce. Todos os sistemas de arquivos oferecem uma performance de linha de base consistente de 50 MiB/s por TiB de armazenamento. Além disso, todos os sistemas de arquivos (independentemente do tamanho) podem estourar para 100 MiB/s. Os sistemas de arquivos maiores que 1 TB podem atingir 100 MiB/s por TiB de armazenamento. À medida que você adiciona dados ao sistema de arquivos, a taxa de transferência máxima disponível para o sistema de arquivos é dimensionada linear e automaticamente com o armazenamento.

    A taxa de transferência do sistema de arquivos é compartilhada por todas as instâncias do EC2 conectadas a um sistema de arquivos. Para obter mais informações sobre as características de performance do sistema de arquivos do EFS, consulte a <a href="http://docs.aws.amazon.com/efs/latest/ug/performance.html" target="_blank">documentação oficial do Amazon Elastic File System</a>.

    <i class="far fa-thumbs-up" style="color:blue"></i> Parabéns! Você criou um sistema de arquivos do EFS, o montou em uma instância do EC2 e executou um teste de benchmark de E/S para examinar as características de performance dele.

<br/>
## Enviar o trabalho

63. Na parte superior destas instruções, escolha <span id="ssb_blue">Submit</span> (Enviar) para gravar seu progresso e, quando solicitado, escolha **Yes** (Sim).

64. Se os resultados não forem exibidos após alguns minutos, volte ao topo destas instruções e escolha <span id="ssb_voc_grey">Grades</span> (Notas).

    **Dica**: você pode enviar seu trabalho várias vezes. Depois de alterar o trabalho, escolha **Submit** (Enviar) novamente. Seu último envio é o que será gravado para este laboratório.

65. Para ver o feedback detalhado do seu trabalho, escolha <span id="ssb_voc_grey">Details</span> (Detalhes) e depois <i class="fas fa-caret-right"></i> **View Submission Report** (Visualizar relatório de envio).

<br/>
## Laboratório concluído <i class="fas fa-graduation-cap"></i>

<i class="fas fa-flag-checkered"></i> Parabéns! Você concluiu o laboratório.


66. Escolha <span id="ssb_voc_grey">End Lab</span> (Encerrar laboratório) na parte superior desta página e, em seguida, selecione <span id="ssb_blue">Yes</span> (Sim) para confirmar que você deseja encerrar o laboratório.

    Será exibido um painel com a mensagem: *DELETE has been initiated... You may close this message box now. (A EXCLUSÃO foi iniciada... Você pode fechar esta caixa de mensagem agora).*

67. Escolha o **X** no canto superior direito para fechar o painel.





*©2020, Amazon Web Services, Inc. e suas afiliadas. Todos os direitos reservados. Este trabalho não pode ser reproduzido ou redistribuído, no todo ou em parte, sem permissão prévia por escrito da Amazon Web Services, Inc. É proibido copiar, emprestar ou vender para fins comerciais.*