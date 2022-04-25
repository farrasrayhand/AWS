# Módulo 3 – Laboratório guiado: Hospedar um site estático
[//]: # "SKU: ILT-TF-200-ACACAD-2    Source Course: ILT-TF-100-ARCHIT-6 branch dev_65"

## Visão geral e objetivos do laboratório
Os sites estáticos têm conteúdo fixo sem processamento de back-end. Eles podem conter páginas HTML, imagens, folhas de estilo e todos os arquivos necessários para renderizar um site. No entanto, os sites estáticos não usam scripts no lado do servidor nem bancos de dados. Se você quiser que suas páginas da Web estáticas forneçam interatividade e executem lógica de programação, poderá usar JavaScript, que é executado no navegador da Web do usuário.

É possível hospedar facilmente um site estático no Amazon Simple Storage Service (Amazon S3) fazendo o upload do conteúdo e tornando-o publicamente acessível. Nenhum servidor é necessário, e você pode usar o Amazon S3 para armazenar e recuperar qualquer quantidade de dados a qualquer momento, de qualquer lugar da Web.

Depois de concluir este laboratório, você deverá ser capaz de:

- Criar um bucket no Amazon S3
- Fazer upload de conteúdo no bucket
- Permitir acesso aos objetos do bucket
- Atualizar o site

<br/>

## Duração

Este laboratório levará aproximadamente **20 minutos** para ser concluído.

<br/>
## Restrições de serviço da AWS
Neste ambiente de laboratório, o acesso aos serviços e ações de produto da AWS pode estar restrito àqueles necessários para concluir as instruções do laboratório. Você poderá encontrar erros se tentar acessar outros serviços ou executar ações além do que está descrito neste laboratório.

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
## Tarefa 1: criar um bucket no Amazon S3

Nesta tarefa, você criará um bucket do S3 e o configurará para hospedagem estática de sites.

5. No **Console de Gerenciamento da AWS**, no menu <span id="ssb_services">Services<i class="fas fa-angle-down"></i></span> (Serviços), clique em **S3**.

6. Escolha <span id="ssb_orange">Create bucket</span> (Criar Bucket)

   O nome dos buckets do S3 são globalmente exclusivos e o namespace é compartilhado por todas as contas da AWS. Depois de criar um bucket, o nome desse bucket não poderá ser usado por outra conta da AWS em qualquer região da AWS, a menos que você o exclua.

   Por esse motivo, para este laboratório, você usará um nome de bucket que inclua um número aleatório, como: _website-123_

7. Em **Bucket name**, (Nome do bucket), insira: `website-<123>` (substitua <_123_> por um número aleatório)

   O acesso público aos buckets é bloqueado por padrão. Como os arquivos do site estático precisarão ser acessíveis pela Internet, você deverá permitir o acesso público.

8. Desmarque **Block all public access** (Bloquear todo o acesso público), e, em seguida, selecione a caixa que indica **I acknowledge that the current settings may result in this bucket and the objects within becoming public** (Reconheço que as configurações atuais podem fazer com que este bucket e os objetos dentro dele se tornem públicos).

9. Escolha <span id="ssb_orange">Create bucket</span> (Criar bucket).

   É possível usar tags para adicionar mais informações a um bucket, como um código de projeto, centro de custo ou proprietário.

10. Escolha o nome do novo bucket.

11. Escolha a guia **Properties** (Propriedades).

12. Role até o painel **Tags**.

13. Escolha <span id="ssb_white">Edit</span> (Editar), em seguida escolha <span id="ssb_white">Add tag</span> (Adicionar tag) e digite:

    * **Chave:** `Departamento`
    * **Valor:** `Marketing`

14. Escolha <span id="ssb_orange">Save changes</span> (Salvar alterações) para salvar a tag.

    Configure agora o bucket para hospedagem de site estático.

15. Continue no console **Properties** (Propriedades).

16. Role até o painel **Static website hosting** (Hospedagem de site estático).

17. Escolha <span id="ssb_white">Edit</span> (Editar)

18. Defina as seguintes configurações:

    - **Hospedagem de site estático:** habilitado
    - **Tipo de hospedagem:** hospedagem de site estático
    - **Documento de índice:** `index.html`
       - **Nota**: é necessário inserir esse valor, mesmo que ele já esteja exibido.
    - **Documento de erro:** `error.html`

19. Escolha <span id="ssb_orange">Save changes</span> (Salvar alterações).

20. Escolha o link em **Bucket website endpoint** (Endpoint do site do bucket).

    Você receberá a mensagem *403 Forbidden* (Proibido) porque as permissões do bucket ainda não foram configuradas. Mantenha esta guia aberta no navegador da Web para que você possa retornar a ela mais tarde.

    O bucket agora foi configurado para hospedar um site estático.

<br/>
## Tarefa 2: fazer upload do conteúdo para o bucket

Nesta tarefa, você fará upload dos arquivos estáticos para o bucket.

21. Clique com o botão direito do mouse em cada um destes links e faça o download dos arquivos para o computador:

    <i class="fas fa-exclamation-triangle"></i> Cada arquivo deve manter o mesmo nome de arquivo, incluindo a extensão.

    - [index.html](../../../scripts/index.html)
    - [script.js](../../../scripts/script.js)
    - [style.css](../../../scripts/style.css)

22. Volte para o console do Amazon S3 e clique na guia **Objects** (Objetos).

23. Escolha <span id="ssb_orange">Upload</span> (Fazer upload)

24. Escolha <span id="ssb_white">Add files</span> (Adicionar arquivos)

25. Escolha os três arquivos que foram obtidos por download.

26. Escolha <i class="far fa-check-square"></i>I acknowledge that existing objects with the same name will be overwritten (Eu reconheço que objetos existentes com o mesmo nome serão substituídos).

27. Escolha <span id="ssb_orange">Upload</span> (Fazer upload)

    Os arquivos serão carregados para o bucket.

<br/>
## Tarefa 3: habilitar o acesso aos objetos

Os objetos armazenados no Amazon S3 são privados por padrão. Isso garante que os dados da sua organização permaneçam seguros.

Nesta tarefa, você tornará os objetos carregados publicamente acessíveis.

Primeiro, confirme que os objetos são privados no momento.

28. Retorne à guia do navegador que mostrou a mensagem *403 Forbidden* (Proibido).

29. Atualize <i class="fas fa-sync"></i>a página da Web.

    <i class="fas fa-comment"></i> Se você tiver fechado essa guia por engano, vá para a guia **Properties** (Propriedades) e, no painel **Static website hosting** (Hospedagem de site estático), escolha o link **Endpoint** novamente.

    Você ainda deverá ver a mensagem *403 Forbidden* (Proibido). Esta resposta é esperada! Essa mensagem indica que seu site estático está sendo hospedado pelo Amazon S3, mas que o conteúdo é privado.

    É possível tornar os objetos do Amazon S3 públicos de duas maneiras diferentes:

    - Para tornar público um bucket inteiro ou um diretório específico de um bucket, use uma *política de bucket*.
    - Para tornar públicos objetos individuais de um bucket, use uma *lista de controle de acesso (ACL)*.

    Normalmente, é mais seguro tornar públicos _objetos individuais_ porque isso evita tornar outros objetos públicos por engano. No entanto, se você souber que não há informações confidenciais em todo o intervalo, poderá usar uma _política de bucket_.

    Agora você configurará os objetos individuais no bucket para serem acessíveis publicamente.

30. Retorne à guia do navegador da Web com o console do Amazon S3 (mas não feche a guia do site).

31. Selecione <i class="far fa-check-square"></i>todos os três objetos.

32. No menu <span id="ssb_s3_blue">Actions<i class="fas fa-angle-down"></i></span> (Ações), escolha **Make public** (Tornar público).

    Uma lista dos três objetos é exibida.

33. Escolha <span id="ssb_s3_blue">Make public</span> (Tornar público)

    O site estático agora está acessível ao público.

34. Volte para a guia do navegador da Web com a mensagem *403 Forbidden* (Proibido).

35. Atualize <i class="fas fa-sync"></i>a página da Web.

    Agora você deve ver o site estático hospedado pelo Amazon S3.

<br/>
## Tarefa 4: atualizar o site

É possível fazer alterações no site editando o arquivo HTML e fazendo o upload dele novamente no bucket do Amazon S3.

O Amazon S3 é um _serviço de armazenamento de objetos_, portanto, você deve fazer upload de todo o arquivo. Essa ação substitui o objeto existente no bucket. Não é possível editar o conteúdo de um objeto, por isso, o objeto inteiro deve ser substituído.

36. No computador, carregue o arquivo **index.html** em um editor de texto (por exemplo, Bloco de Notas ou TextEdit).

37. Encontre o texto **Served from Amazon S3** (Enviado pelo Amazon S3) e substitua-o por `Created by <YOUR-NAME>` (Criado por <SEU-NOME>), substituindo seu nome por <*YOUR-NAME*> (<SEU-NOME>) (por exemplo, _Created by Jane_ (Criado pela Jane)).

38. Salve o arquivo.

39. Volte para o console do Amazon S3 e faça upload do arquivo **index.html** que você acabou de editar.

40. Selecione <i class="far fa-check-square"></i>**index.html** e use o menu **Actions** (Ações) para escolher a opção **Make public** (Tornar público) novamente.

41. Volte para a guia do navegador da Web com o site estático e atualize <i class="fas fa-sync"></i>a página.

O nome agora deverá aparecer na página.



O site estático agora está acessível na Internet. Como ele está hospedado no Amazon S3, o site tem alta disponibilidade e pode processar grandes volumes de tráfego sem usar nenhum servidor.

Também é possível usar seu próprio nome de domínio para direcionar os usuários para um site estático hospedado no Amazon S3. Para fazer isso, é possível usar o Domain Name System (DNS) do Amazon Route 53 junto com o Amazon S3.

<br/>
## Enviar o trabalho

42. Na parte superior destas instruções, escolha <span id="ssb_blue">Submit</span> (Enviar) para gravar seu progresso e, quando solicitado, escolha **Yes** (Sim).

43. Se os resultados não forem exibidos após alguns minutos, volte ao topo destas instruções e escolha <span id="ssb_voc_grey">Grades</span> (Notas).

**Dica**: você pode enviar seu trabalho várias vezes. Depois de alterar o trabalho, escolha **Submit** (Enviar) novamente. Seu último envio é o que será gravado para este laboratório.

44. Para ver o feedback detalhado do seu trabalho, escolha <span id="ssb_voc_grey">Details</span> (Detalhes) e depois <i class="fas fa-caret-right"></i> **View Submission Report** (Visualizar relatório de envio).

<br/>

## Laboratório concluído <i class="fas fa-graduation-cap"></i>

<i class="fas fa-flag-checkered"></i> Parabéns! Você concluiu o laboratório.

45. Escolha <span id="ssb_voc_grey">End Lab</span> (Encerrar laboratório) na parte superior desta página e, em seguida, selecione <span id="ssb_blue">Yes</span> (Sim) para confirmar que você deseja encerrar o laboratório.

    Um painel indica que *DELETE has been initiated... You may close this message box now (A EXCLUSÃO foi iniciada... Você pode fechar esta caixa de mensagem agora)*.

46. Escolha o **X** no canto superior direito para fechar o painel.



*©2020, Amazon Web Services, Inc. e suas afiliadas. Todos os direitos reservados. Este trabalho não pode ser reproduzido ou redistribuído, no todo ou em parte, sem permissão prévia por escrito da Amazon Web Services, Inc. É proibido copiar, emprestar ou vender para fins comerciais.*